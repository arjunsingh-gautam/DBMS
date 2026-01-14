# <span style="color:orange">Why Concurrency Is Important for a Database</span>

From first principles:

> A database exists to be **shared**, and the moment data is shared, **concurrency becomes unavoidable**.

Concurrency is **not an optimization** — it is a **fundamental requirement**.

---

## <span style="color:red">Why Concurrency Is Required</span>

### <span style="color:#1e90ff">The Root Cause (First Principle)</span>

Databases serve:

- Multiple users
- Multiple applications
- Multiple processes
- Multiple machines

All of them:

- Read
- Write
- Update
  **at the same time**

Without concurrency control:

> **Correct data cannot be guaranteed.**

---

### <span style="color:#8a2be2">Single-User World vs Real World</span>

**Single user**

- One operation at a time
- No conflicts
- Simple

**Real world**

- 1000s of operations per second
- Overlapping reads & writes
- Conflicting intentions

Concurrency exists because **time overlaps**.

---

## <span style="color:red">How Concurrency Works in a Database</span>

### <span style="color:#1e90ff">Core Idea</span>

> Allow many operations **at the same time**
> While making the result appear as if they happened **one by one**

This illusion is called **Isolation**.

---

### <span style="color:#8a2be2">Key Tools Used by Databases</span>

- Locks
- Timestamps
- Versions (MVCC)
- Schedulers

All exist to control **who sees what and when**.

---

### <span style="color:#8a2be2">Analogy: Bank Account Counter</span>

Imagine:

- One bank account
- ₹1000 balance
- Two clerks working simultaneously

#### Clerk A:

Withdraw ₹500

#### Clerk B:

Withdraw ₹700

---

### <span style="color:#ff4500">Without Concurrency Control</span>

1. Both clerks read balance = 1000
2. Clerk A writes 500
3. Clerk B writes 300

Final balance = **300**

❌ ₹500 + ₹700 = ₹1200 withdrawn from ₹1000
❌ Money created from nowhere

---

### <span style="color:#32cd32">With Concurrency Control</span>

- Clerk A locks account
- Clerk B waits
- Balance updated correctly
- No corruption

Final state is **logically valid**

---

### <span style="color:#8a2be2">How Databases Implement This</span>

#### Lock-based Concurrency

- Read lock (shared)
- Write lock (exclusive)

#### MVCC (Multi Version Concurrency Control)

- Readers see old versions
- Writers create new versions
- No blocking reads

---

## <span style="color:red">What Happens If Concurrency Breaks?</span>

### <span style="color:#1e90ff">The Inevitable Failures</span>

If concurrency is not controlled, databases suffer from **anomalies**.

---

### <span style="color:#8a2be2">1. Lost Update</span>

Two transactions update same row:

- One update overwrites the other
- One change is lost

---

### <span style="color:#8a2be2">2. Dirty Read</span>

- Transaction reads uncommitted data
- Other transaction rolls back
- Data read never actually existed

---

### <span style="color:#8a2be2">3. Non-Repeatable Read</span>

- Same query
- Different result within same transaction

---

### <span style="color:#8a2be2">4. Phantom Read</span>

- Rows appear or disappear
- Aggregate results change unexpectedly

---

### <span style="color:#ff0000">Real-World Impact</span>

- Incorrect balances
- Double bookings
- Inventory mismatch
- Financial loss
- Legal issues

---

## <span style="color:red">Why Concurrency Is Important for Data Consistency</span>

### <span style="color:#1e90ff">Consistency = Trust</span>

Consistency means:

> Database rules are never violated.

Concurrency directly threatens consistency because:

- Operations interleave
- Partial updates become visible

---

### <span style="color:#8a2be2">ACID Connection</span>

Concurrency primarily protects:

- **Isolation**
- **Consistency**

Without isolation:

- Consistency collapses

---

### <span style="color:#8a2be2">Invariant Protection</span>

Databases enforce invariants like:

- Balance ≥ 0
- Stock ≥ 0
- Unique constraints

Concurrency control ensures:

- Invariants hold **across time overlaps**

---

### <span style="color:#8a2be2">First Principle Insight</span>

> A single operation may be correct, but **interleaved correct operations can produce an incorrect state**.

Concurrency control prevents this.

---

## <span style="color:red">One-Line First Principle Summary</span>

> **Concurrency is important because databases exist to be shared, and without strict control over simultaneous operations, data correctness, consistency, and trust collapse.**

---
