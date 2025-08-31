📚 Caching Strategies in System Design
1. Read Caching Strategies
🔹 Cache-Aside (Lazy Loading)

Flow:
App → Cache → if miss → DB → update cache.

✅ Pros

Fault tolerant: If cache fails, DB still works.

Flexible schema: Cache & DB can store different structures.

❌ Cons

Risk of stale data unless cache invalidation is handled.

First request = slow (cache miss).

🔹 Read-Through

Flow:
App → Cache → if miss → Cache fetches from DB → updates cache.

✅ Pros

Fresh data if combined with Write-Through.

Reads are always from cache (fast after first hit).

❌ Cons

Cache failure = system downtime (since reads must go through cache).

Schema coupling: Cache must mirror DB structure.

2. Write Caching Strategies
🔹 Write-Aside (Lazy Caching)

Flow:
App → DB (only). Cache updated later on read.

✅ Pros

Faster writes (DB only).

Cache independence: cache failure doesn’t block writes.

❌ Cons

Reads may be slow (cache miss on first read).

Risk of stale data if cache isn’t refreshed.

🔹 Write-Through

Flow:
App → Cache → Cache → DB (synchronous).

✅ Pros

No stale data (cache + DB always consistent).

Reads are fast (cache always populated).

❌ Cons

Slower writes (two writes per operation).

Cache dependency: cache failure may block writes.

🔹 Write-Behind (Write-Back)

Flow:
App → Cache → (async background write) → DB.

✅ Pros

Very fast writes (instant cache confirmation).

Reads are fast (cache always has recent data).

❌ Cons

Risk of data loss if async DB update fails.

Cache dependency: cache failure can block persistence.

✅ Quick Comparison Table
Strategy	Speed (Read)	Speed (Write)	Freshness	Cache Dependency	Best Use Case
Cache-Aside	⚡ Fast (after 1st hit)	✅ Fast	❌ Stale possible	No	General-purpose, flexible
Read-Through	⚡ Always fast	N/A	✅ Fresh (with Write-Through)	Yes	Systems needing consistent reads
Write-Aside	❌ Slow on 1st read	✅ Fast	❌ Risk of stale	No	Write-heavy systems
Write-Through	⚡ Fast	❌ Slower	✅ Always fresh	Yes	Read-heavy, consistency needed
Write-Behind	⚡ Fast	⚡ Very fast	❌ Risky (DB lag)	Yes	High-write workloads (logs, analytics)

📌 Rule of Thumb for Interviews

If asked:

Read-heavy system? → Cache-Aside or Read-Through.

Write-heavy system? → Write-Aside or Write-Behind.

Consistency critical? → Write-Through + Read-Through.

Performance critical (eventual consistency ok)? → Write-Behind.
