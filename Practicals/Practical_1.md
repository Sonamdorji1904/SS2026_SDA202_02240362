# Chapter 8 Review: Design a URL Shortener
*System Design Interview — Critical Analysis*

---

## 1. Problem Analysis

In this chapter it discusses about a deceptively simple problem: given a long URL, produce a short one that redirects reliably. The real challenge isn't the transformation of URL, it's about doing this transformation at large scale while keeping it fast, consistent, and cheap.

### Requirements Gathered

| Dimension | Constraint |
|-----------|------------|
| Traffic | 100 million URLs generated per day |
| Characters | `[0-9, a-z, A-Z]` 62 possible characters |
| Mutability | URLs cannot be deleted or updated |
| Read/Write ratio | 10:1 (reads dominate) |

### Back-of-Envelope Estimates

- **Write ops/sec:** 100M / 86,400 ≈ 1,160
- **Read ops/sec:** 1,160 × 10 = 11,600
- **Records over 10 years:** 365 billion
- **Storage over 10 years:** 365 billion × 100 bytes = **365 TB**

#### Note:
* ops - operations
* sec - seconds

Driven by a 10:1 read/write ratio, the architecture optimizes for fast reads via caching while keeping the write path straightforward.

---

## 2. Solution Analysis

### Two Core API Endpoints

```
POST /api/v1/data/shorten       → { longUrl } → shortURL
GET  /api/v1/{shortUrl}         → 301/302 redirect to longURL
```

### 301 vs 302 Redirect 

| | 301 Permanent | 302 Temporary |
|---|---|---|
| Browser caches? | Yes | No |
| Every request hits the server? | First only | Every time |
| Good for analytics? | No | Yes |
| Good for reducing load? | Yes | No |

**Key Takeaway:** Most commercial URL shorteners use 302 redirect because it is a better choice as it can
track click rate and source of the click more easily.

---

## Hash Function — Two Approaches

### Approach 1: Hash + Collision Resolution

- Applies well-known hash functions like  CRC32 / MD5 / SHA-1 to the long URL
- It takes the first 7 characters
- And if collision is detected → it appends a predefined string until no more collision is detected
- Uses a technique called **bloom filter** to reduce expensive DB lookups during collision checks by testing if an element is a member of a set

**Pros:** 
* Fixed URL length, no ID generator needed

**Cons:** 
* Collision resolution overhead, expensive DB queries per request

### Approach 2: Base 62 Conversion *(Common)*

- Uses a distributed unique ID generator to produce a globally unique integer
- Convert that integer to base 62 (`[0-9a-zA-Z]`)
- Base of 62 is used as there are 62 possible characters for hashValue
- 62^7 ≈ 3.5 trillion > 365 billion needed, therefore 7 characters is sufficient

**Pros:** 
- Collision is not possible (ID uniqueness guarantees URL uniqueness)

**Cons:** 
- As ID numbers get larger, URLs become longer. We need a distributed system to generate these IDs, but sequential ones are security risks because they can be guessed.

--- 

### Comparison Table

| Property | Hash + Collision | Base 62 |
|----------|-----------------|---------|
| URL length | Fixed | Grows with ID |
| ID generator needed | No | Yes |
| Collision possible | Yes (must resolve) | No |
| Next URL predictable? | No | Yes (security risk) |

---

### URL Shortening Flow (Base 62)

```
Input longURL
    ↓
Is longURL already in DB?
    |

    ├── YES → Return existing shortURL

    |

    └── NO  → Generate new unique ID
                  ↓
              Convert ID to base 62 (7 chars)
                  ↓
              Save (id, shortURL, longURL) to DB
                  ↓
              Return shortURL
```

**Concrete example:**
- Input: `https://en.wikipedia.org/wiki/Systems_design`
- Unique ID generated: `2009215674938`
- Base 62 conversion: `zn9edcu`
- Short URL: `https://tinyurl.com/zn9edcu`

---

### URL Redirecting Flow

```
User clicks short URL
    ↓
Load balancer → Web server
    ↓
shortURL in cache?
    |

    ├── YES → Return longURL directly

    |

    └── NO  → Query DB for longURL
                  |

                  ├── Found → Cache it, return longURL

                  |

                  └── Not found → Invalid shortURL (error)
```

The cache stores `<shortURL, longURL>` pairs. Since reads are 10× writes, this dramatically reduces DB load.

---

### Data Model

```
url Table
┌───────────────────────────┐
│ id (PK, auto increment)   │
│ shortURL                  │
│ longURL                   │
└───────────────────────────┘
```

A relational DB is used instead of an in-memory hash table because memory resources are limited and expensive at this scale.

---

## 3. My Understanding, Confusions & Topics to Explore

### What I Understood from this Topic

**Read-to-write ratio shapes architecture.** Optimize for fast reads using caches, and accept higher complexity for write operations. This is a common design pattern.

**Choosing between 301 and 302 redirects is a smart, balanced decision.** This single decision impacts caching, analytics, and server load simultaneously. It's a trade-off, not a one-size-fits-all solution.

**Base 62 over hashing is a best method** when we already need a unique ID for the DB primary key, we're getting the shortURL by just converting that integer.

---

### Confusions & Gaps


**1. Cache invalidation is never discussed.**
- In this chapter author describes caching `<shortURL, longURL>` pairs but avoids the cache invalidation problem by assuming URLs never change. In a real system where users can edit or delete links, how do we stop the cache from serving outdated link data, and how do we keep every node in a distributed cluster synced?

**2. The bloom filter mention is too brief.**
- The **blooom filter** is mentioned in one sentence as a performance optimization for collision detection and immediately dropped. But it lefts some questions unanswered: How many mistakes are okay? How much memory for 365 billion URLs? What happens if it makes a mistake?

**3. The base 62 security concern is flagged but not solved.**
- Table 8-3 correctly explains that sequential IDs make it easy to enumerate all valid short URLs, a serious privacy leak if anyone shortens a link to a sensitive document. In this chapter it just labels this "a security concern" with no solution or any explanation.


---

### Topics to Explore Further

- **Distributed unique ID generation** 
- **Bloom filters**
- **Cache Discarding strategies**


## 4. Critical Analysis Summary

This chapter is a high-level design guide. It focuses on planning the system rather than building a full working version. It clearly explains key ideas like when to use 301 vs 302 redirects and how Base 62 encoding works. Since the system is mostly read-heavy, it correctly emphasizes caching, making it useful for system design interviews.

However, it skips over the hardest real-world challenges. By assuming URLs never change, it avoids dealing with cache invalidation and managing data across multiple systems. Important topics like generating unique IDs across servers, database sharding, and tuning Bloom filters are only briefly mentioned, not fully explained. To actually build this system, you would still need to solve these practical issues, including security and data consistency.

