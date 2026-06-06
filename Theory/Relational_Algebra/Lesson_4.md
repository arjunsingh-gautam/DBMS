# **<span style="color:#4FC3F7">Drawbacks and Limitations of Relational Algebra</span>**

A common misconception is:

> "Since relational algebra is the mathematical foundation of relational databases, it can express every database query."

This is **false**.

Relational Algebra is powerful, but it has fundamental theoretical limitations.

These limitations arise because Classical Relational Algebra is based on:

```text
Set Theory
+
First Order Logic (FOL)
```

and First Order Logic itself has expressiveness limits.

---

# **<span style="color:#FFD54F">Understanding Expressive Power</span>**

Before studying limitations, understand what expressive power means.

Suppose we ask:

```text
Find all students from CS department.
```

Relational algebra can express it.

Suppose we ask:

```text
Find all employees who indirectly report to the CEO
through any number of managerial levels.
```

Now things become interesting.

The question becomes:

> Can relational algebra express this query using a finite number of operators?

Sometimes the answer is **No**.

---

# **<span style="color:#4FC3F7">1. Limitation: First Order Logic (FOL) Expressiveness</span>**

---

## **<span style="color:#FFD54F">What is First Order Logic?</span>**

Relational algebra is equivalent in expressive power to:

First-order logic

First Order Logic allows reasoning about:

```text
Objects
Attributes
Relationships
```

using:

```text
∀  (for all)
∃  (exists)
```

Example:

```text
There exists a student in CS.
```

FOL can express it.

---

## **<span style="color:#FFD54F">What FOL Cannot Express</span>**

FOL cannot naturally express:

```text
Infinite repetition
Arbitrary recursion
Transitive closure
```

This becomes a database limitation.

---

# **<span style="color:#81C784">Example: Even Number of Records</span>**

Suppose relation:

R

| Value |
| ----- |
| 1     |
| 2     |
| 3     |
| 4     |

Question:

```text
Does R contain an even number of tuples?
```

---

Relational algebra cannot answer.

Why?

Because RA works on tuples.

It has operators:

```text
σ
π
×
∪
−
```

None can count arbitrarily.

---

To determine parity:

```text
4 -> Even
5 -> Odd
1000 -> Even
1001 -> Odd
```

requires reasoning about an unbounded quantity.

FOL cannot express this.

Therefore RA cannot express it.

---

## **<span style="color:#FFD54F">Why Exactly Does It Fail?</span>**

Let's dry run mentally.

Suppose:

```text
R = {1,2,3,4}
```

You attempt:

```text
Selection?
Projection?
Join?
Difference?
```

No operator can produce:

```text
COUNT(R)
```

in classical relational algebra.

There is simply no counting mechanism.

---

# **<span style="color:#81C784">Another Example: Majority Query</span>**

Question:

```text
Did more than 50% of students pass?
```

Need:

```text
COUNT(pass)
COUNT(total)
```

and comparison.

Classical relational algebra cannot express it.

---

## **<span style="color:#FFD54F">Constraint Causing Failure</span>**

Relational Algebra operates on:

```text
Individual tuples
Finite set operations
```

It does not operate on:

```text
Cardinality properties
Global database properties
```

---

# **<span style="color:#4FC3F7">2. No Recursive Closure (Most Important Limitation)</span>**

This is the biggest theoretical limitation.

---

## **<span style="color:#FFD54F">What is Recursion?</span>**

Recursion means:

```text
Apply relationship repeatedly
until no new result appears.
```

Examples:

```text
Employee hierarchy
Family tree
Network routing
Graph traversal
Friend-of-friend
Dependency analysis
```

---

# **<span style="color:#81C784">Example: Employee Hierarchy</span>**

Employee

| Emp | Manager |
| --- | ------- |
| A   | B       |
| B   | C       |
| C   | D       |
| D   | NULL    |

Meaning:

```text
A reports to B
B reports to C
C reports to D
```

Question:

```text
Who reports to D directly or indirectly?
```

Answer:

```text
A
B
C
```

---

## **<span style="color:#FFD54F">Why Join Is Not Enough?</span>**

Let's try.

---

### Step 1

Direct reports

```text
Employee ⋈ Employee
```

Find:

```text
A -> C
B -> D
```

Two levels.

---

### Step 2

Join again.

```text
(Employee ⋈ Employee) ⋈ Employee
```

Now:

```text
A -> D
```

Three levels.

---

Problem:

How many joins needed?

Unknown.

Could be:

```text
3 levels
30 levels
300 levels
3000 levels
```

Relational algebra requires a fixed query.

You cannot write:

```text
Keep joining until finished.
```

---

## **<span style="color:#FFD54F">The Failure</span>**

Relational algebra queries must be finite.

Example:

```text
Join 3 times
```

works only for depth ≤3.

If hierarchy depth becomes 4:

query fails.

If hierarchy depth becomes 100:

query fails.

---

### Root Cause

Relational algebra lacks:

```text
Loop
Iteration
Recursion
Fixpoint computation
```

---

# **<span style="color:#81C784">Graph Example</span>**

Roads

| From | To  |
| ---- | --- |
| A    | B   |
| B    | C   |
| C    | D   |

Question:

```text
Can A reach D?
```

Need:

```text
A→B→C→D
```

Transitive closure.

Classical RA cannot compute transitive closure.

---

## **<span style="color:#FFD54F">Why Modern SQL Can?</span>**

Modern SQL adds recursion.

Example:

```sql
WITH RECURSIVE
```

This is beyond classical relational algebra.

---

# **<span style="color:#4FC3F7">3. No Native Aggregation</span>**

Classical RA cannot perform:

```text
COUNT
SUM
AVG
MIN
MAX
```

---

Example:

Student

| Name | Marks |
| ---- | ----- |
| A    | 90    |
| B    | 80    |
| C    | 70    |

Question:

```text
Average Marks?
```

Need:

```text
(90+80+70)/3
```

No classical operator performs arithmetic reduction.

---

## **<span style="color:#FFD54F">Failure Mechanism</span>**

RA transforms relations into relations.

Aggregation transforms:

```text
Many tuples
→
One value
```

This violates original RA assumptions.

Hence aggregation was later added as:

```text
γ (gamma)
```

Extended Relational Algebra.

---

# **<span style="color:#4FC3F7">4. No Ordering Concept</span>**

Relations are sets.

Sets have no order.

---

Suppose:

Student

| Marks |
| ----- |
| 90    |
| 80    |
| 70    |

Relational algebra sees:

```text
{70,80,90}
```

Same as:

```text
{90,80,70}
```

---

Question:

```text
Top 3 students
```

requires ordering.

Classical RA cannot express:

```text
ORDER BY
```

because order does not exist mathematically.

---

# **<span style="color:#4FC3F7">5. No Duplicate Handling</span>**

Relations are sets.

Sets eliminate duplicates automatically.

---

Example:

| Dept |
| ---- |
| CS   |
| CS   |
| IT   |

RA interprets:

```text
{CS,IT}
```

---

Problem:

Sometimes duplicates matter.

Example:

```sql
COUNT(*)
```

depends on duplicates.

Classical RA loses them.

---

Modern SQL uses:

```text
Bag (Multiset) Semantics
```

instead of pure set semantics.

---

# **<span style="color:#4FC3F7">6. Poor Practical Usability</span>**

Humans rarely write relational algebra directly.

Example:

Relational Algebra:

```text
πName(
σDept='CS'
(
Student ⋈ Enrollment
)
)
```

Equivalent SQL:

```sql
SELECT Name
FROM Student
JOIN Enrollment
WHERE Dept='CS';
```

SQL is far easier.

Thus RA is mostly:

```text
Theoretical Foundation
Query Optimization
Compiler Internal Representation
```

not an end-user language.

---

# **<span style="color:#4FC3F7">7. Cannot Express General Computation</span>**

Relational algebra is not Turing Complete.

It cannot:

```text
Run loops
Run recursion
Perform arbitrary algorithms
Maintain state
Execute procedural logic
```

---

Example:

```text
Generate Fibonacci numbers
Compute shortest path
Run DFS
Run Dijkstra
```

Impossible in pure RA.

---

# **<span style="color:#4FC3F7">Why These Limitations Exist?</span>**

The designers intentionally restricted relational algebra.

Why?

Because unrestricted computation leads to:

```text
Undecidability
Non-termination
Optimization impossibility
```

By limiting RA to First Order Logic:

```text
Every query terminates
Every query is finite
Optimization becomes possible
Formal proofs become possible
```

This tradeoff made relational databases practical.

---

# **<span style="color:#4FC3F7">Summary Table</span>**

| Limitation                | Why It Happens                   | Example            |
| ------------------------- | -------------------------------- | ------------------ |
| First Order Logic Bound   | No higher-order reasoning        | Even-number query  |
| No Recursive Closure      | No loops/fixpoint                | Employee hierarchy |
| No Transitive Closure     | Infinite traversal impossible    | Graph reachability |
| No Aggregation            | Cannot reduce relation to scalar | COUNT, AVG         |
| No Ordering               | Relations are sets               | Top-K queries      |
| No Duplicate Preservation | Set semantics                    | COUNT duplicates   |
| Not Turing Complete       | No procedural computation        | DFS, Dijkstra      |
| Poor Human Readability    | Mathematical notation            | Complex joins      |

# **<span style="color:#4FC3F7">Most Important Database Theory Insight</span>**

The single most important limitation of classical relational algebra is:

```text
No Recursive Closure
```

because it prevents computation of:

```text
Transitive Closure
Graph Reachability
Organizational Hierarchies
Bill of Materials
Dependency Trees
Social Network Traversal
```

This limitation led to decades of research and eventually to recursive query languages such as:

- Datalog
- Recursive SQL (`WITH RECURSIVE`)
- Graph databases such as Neo4j

which extend the relational model to support recursive and graph-oriented queries while retaining much of the mathematical rigor of relational algebra.
