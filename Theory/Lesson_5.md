# <span style="color:orange">Why Availability Is Important in a Database</span>

From first principles:

> A database exists to **serve data when it is needed**.
> If data exists but cannot be accessed, **it is equivalent to data not existing**.

Availability is not about performance — it is about **existence in time**.

---

## <span style="color:red">Why Availability Is Important</span>

### <span style="color:#1e90ff">The Core Question (First Principle)</span>

Why store data at all if:

- You cannot read it?
- You cannot write to it?
- It disappears during failures?

A database without availability:

> **Fails its fundamental purpose**.

---

### <span style="color:#8a2be2">Real-World Dependency on Availability</span>

Databases back:

- Banking systems
- Hospitals
- E-commerce
- Trading systems

Downtime means:

- Money loss
- Trust loss
- Operational paralysis

---

### <span style="color:#8a2be2">Availability vs Convenience</span>

Availability is not:

- A luxury
- An optimization

It is:

> **A correctness requirement for real systems**

---

## <span style="color:red">Why Availability Is Important for Data Persistency & System Failures</span>

### <span style="color:#1e90ff">First Principle of Persistence</span>

Persistence means:

> Data must **survive time, crashes, and failures**.

Availability ensures:

- Persisted data is **reachable**
- Stored data can be **used**

Persistence without availability is **theoretical**.

---

### <span style="color:#8a2be2">Types of Failures That Threaten Databases</span>

1. Hardware failure (disk crash)
2. Power failure
3. OS crash
4. Network failure
5. Process crash
6. Data center outage

All are **inevitable**, not exceptions.

---

### <span style="color:#8a2be2">What Happens Without Availability</span>

- Database becomes unreachable
- Applications block
- Requests fail
- System halts

Even if:

- Data exists on disk
  It is **effectively dead** during downtime.

---

### <span style="color:#8a2be2">Analogy: Library During a Fire</span>

- Books are intact
- Building is closed

Knowledge exists
But is **unusable**

Availability keeps the **doors open**.

---

## <span style="color:red">How Availability Is Achieved in Databases</span>

### <span style="color:#1e90ff">Core Idea</span>

> Accept that failures will happen
> Design systems that **continue operating anyway**

---

### <span style="color:#8a2be2">1. Replication</span>

**Idea**:

- Multiple copies of data

**Why**:

- If one node fails, another serves requests

Types:

- Primary-Replica
- Multi-Primary

---

### <span style="color:#8a2be2">2. Redundancy</span>

- Extra disks
- Extra machines
- Extra networks

No single point of failure.

---

### <span style="color:#8a2be2">3. Failover Mechanisms</span>

**Process**:

- Detect failure
- Promote replica
- Redirect traffic

Done:

- Automatically
- Quickly

---

### <span style="color:#8a2be2">4. Logging & Recovery</span>

Write-ahead logs:

- Changes recorded before commit
- Enables crash recovery

Ensures:

- Database restarts in consistent state

---

### <span style="color:#8a2be2">5. Backups & Snapshots</span>

Used when:

- Data corruption
- Catastrophic failures

Availability over **long time scales**.

---

### <span style="color:#8a2be2">6. Distributed Databases</span>

- Data spread across nodes
- Local failures isolated

Trade-off:

- Strong consistency vs availability (CAP)

---

### <span style="color:#8a2be2">7. Graceful Degradation</span>

If parts fail:

- Read-only mode
- Limited functionality

Better than full outage.

---

## <span style="color:red">Availability vs Consistency (First Principle Insight)</span>

> A system must choose what to prioritize during failures.

- Strong consistency → may sacrifice availability
- High availability → may relax consistency

This is not a design flaw
It is a **law of distributed systems**

---

## <span style="color:red">One-Line First Principle Summary</span>

> **Availability is important because data has value only when it can be accessed, and failures are inevitable in real systems. Databases must survive and serve despite those failures.**

---
