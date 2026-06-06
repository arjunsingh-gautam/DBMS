# **<span style="color:#4FC3F7">Database Modification Operators in Relational Algebra</span>**

Until now, all relational algebra operators we studied were **query operators**.

They answer:

```text
"What data exists?"
```

But a database is not useful unless we can modify data.

Therefore relational systems support:

1. Insertion
2. Deletion
3. Update

These are called **Database Modification Operators**.

A crucial point:

> Query operations produce new relations.
>
> Modification operations change the actual database state.

This introduces a completely new problem:

```text
Maintaining Data Integrity
```

which is why modifications are much harder than queries.

---

# **<span style="color:#FFD54F">Mathematical View of a Database</span>**

Suppose:

Student

| SID | Name  |
| --- | ----- |
| 1   | Arjun |
| 2   | Rahul |

Mathematically:

```text
Student = {
(1,Arjun),
(2,Rahul)
}
```

A modification operation transforms:

```text
Old Database State
      ↓
Modification
      ↓
New Database State
```

Formally:

```text
DBold → DBnew
```

---

# **<span style="color:#4FC3F7">1. INSERTION</span>**

---

## **<span style="color:#FFD54F">Purpose</span>**

Add new tuples into a relation.

Notation:

```text
Student ← Student ∪ NewTuples
```

Notice:

Insertion is actually based on **Union**.

---

## **<span style="color:#81C784">Mathematical Definition</span>**

Suppose:

```text
R = existing relation
T = new tuples
```

Insertion:

```text
R ← R ∪ T
```

---

## **<span style="color:#81C784">Dry Run</span>**

Current Relation

Student

| SID | Name  |
| --- | ----- |
| 1   | Arjun |
| 2   | Rahul |

Insert:

```text
(3,Priya)
```

Represent as:

```text
T={(3,Priya)}
```

---

### Step 1

Current relation:

```text
{
(1,Arjun),
(2,Rahul)
}
```

---

### Step 2

New tuples:

```text
{
(3,Priya)
}
```

---

### Step 3

Union:

```text
{
(1,Arjun),
(2,Rahul),
(3,Priya)
}
```

Database state updated.

---

# **<span style="color:#FFD54F">Insertion Constraints</span>**

Inserted tuple must satisfy:

### Domain Constraint

Wrong:

```text
SID='ABC'
```

if SID is integer.

---

### NOT NULL Constraint

Wrong:

```text
SID=NULL
```

if SID mandatory.

---

### Primary Key Constraint

Wrong:

```text
(1,Priya)
```

because SID=1 already exists.

---

### Foreign Key Constraint

Student:

```text
DeptID=10
```

must exist in Department table.

---

# **<span style="color:#FFD54F">Insertion Anomalies</span>**

Insertion anomaly occurs when:

> We cannot insert a fact without inserting another unrelated fact.

Example:

Bad design:

| Student | Course | Faculty |
| ------- | ------ | ------- |
| Arjun   | DBMS   | Dr X    |

Suppose:

New Course:

```text
AI
Faculty: Dr Y
```

No students yet.

Cannot insert.

Why?

Student column required.

This is insertion anomaly.

---

## **<span style="color:#81C784">Solution</span>**

Normalization.

Split:

```text
Student
Course
Faculty
```

into separate relations.

---

# **<span style="color:#4FC3F7">2. DELETION</span>**

---

## **<span style="color:#FFD54F">Purpose</span>**

Remove tuples satisfying a condition.

Notation:

```text
R ← R − SelectedTuples
```

Deletion is based on Set Difference.

---

## **<span style="color:#81C784">Mathematical Definition</span>**

Delete:

```text
σcondition(R)
```

Then:

```text
R ← R − σcondition(R)
```

---

## **<span style="color:#81C784">Dry Run</span>**

Student

| SID | Name  |
| --- | ----- |
| 1   | Arjun |
| 2   | Rahul |
| 3   | Priya |

Delete:

```text
SID=2
```

---

### Step 1

Find tuple:

```text
σSID=2(Student)
```

Result:

```text
(2,Rahul)
```

---

### Step 2

Subtract:

```text
Student − {(2,Rahul)}
```

Result:

| SID | Name  |
| --- | ----- |
| 1   | Arjun |
| 3   | Priya |

---

Database updated.

---

# **<span style="color:#FFD54F">Deletion Constraints</span>**

Deletion may violate:

### Referential Integrity

Department

| DeptID |
| ------ |
| 10     |

Student

| SID | DeptID |
| --- | ------ |
| 1   | 10     |

Delete department:

```text
DeptID=10
```

Now student references non-existing department.

Broken database.

---

# **<span style="color:#FFD54F">Deletion Anomaly</span>**

Consider:

| Student | Course | Faculty |
| ------- | ------ | ------- |
| Arjun   | DBMS   | Dr X    |

Delete Arjun.

Result:

```text
Course DBMS disappears.
Faculty Dr X disappears.
```

Lost unrelated information.

This is deletion anomaly.

---

## **<span style="color:#81C784">Solution</span>**

Normalization.

Separate entities.

---

# **<span style="color:#4FC3F7">3. UPDATE</span>**

---

## **<span style="color:#FFD54F">Purpose</span>**

Modify existing tuples.

Update is actually:

```text
Delete Old Tuple
+
Insert New Tuple
```

There is no fundamental update operator mathematically.

---

## **<span style="color:#81C784">Mathematical Definition</span>**

Update:

```text
R ← (R − OldTuples)
     ∪
     NewTuples
```

---

## **<span style="color:#81C784">Dry Run</span>**

Student

| SID | Name  |
| --- | ----- |
| 1   | Arjun |
| 2   | Rahul |

Update:

```text
Rahul → Rohit
```

---

### Step 1

Delete old tuple

```text
(2,Rahul)
```

Remaining:

```text
(1,Arjun)
```

---

### Step 2

Insert:

```text
(2,Rohit)
```

---

### Step 3

Result:

| SID | Name  |
| --- | ----- |
| 1   | Arjun |
| 2   | Rohit |

---

# **<span style="color:#FFD54F">Update Constraints</span>**

Same constraints as:

```text
Delete
+
Insert
```

because update is both.

---

Example:

Changing primary key:

```text
SID=1 → SID=5
```

May break foreign keys.

---

# **<span style="color:#FFD54F">Update Anomaly</span>**

Consider bad design:

| Student | Course | Faculty |
| ------- | ------ | ------- |
| Arjun   | DBMS   | Dr X    |
| Rahul   | DBMS   | Dr X    |
| Priya   | DBMS   | Dr X    |

Faculty changes:

```text
Dr X → Dr Y
```

Need update in 3 rows.

Miss one row:

| Student | Course | Faculty |
| ------- | ------ | ------- |
| Arjun   | DBMS   | Dr Y    |
| Rahul   | DBMS   | Dr X    |

Now inconsistent.

This is update anomaly.

---

## **<span style="color:#81C784">Solution</span>**

Store faculty once.

Normalize.

---

# **<span style="color:#4FC3F7">Data Integrity Problems Caused by Modifications</span>**

Every modification operation threatens integrity.

---

## **<span style="color:#FFD54F">1. Entity Integrity</span>**

Rule:

```text
Primary Key
≠ NULL
```

Violation:

```text
INSERT Student(NULL,Arjun)
```

RDBMS rejects.

---

## **<span style="color:#FFD54F">2. Referential Integrity</span>**

Rule:

Foreign key must reference existing tuple.

Violation:

```text
INSERT Student(1,DeptID=99)
```

when DeptID=99 doesn't exist.

Rejected.

---

## **<span style="color:#FFD54F">3. Domain Integrity</span>**

Rule:

Attribute value must belong to domain.

Violation:

```text
Age=-10
```

Rejected.

---

## **<span style="color:#FFD54F">4. User Defined Integrity</span>**

Example:

```text
Salary > 0
```

or

```text
Age >= 18
```

Implemented using CHECK constraints.

---

# **<span style="color:#4FC3F7">How RDBMS Protects Integrity</span>**

When a modification occurs:

```text
User Query
    ↓
Parser
    ↓
Constraint Checker
    ↓
Transaction Manager
    ↓
Storage Engine
```

The DBMS does not directly write data.

It first verifies all rules.

---

# **<span style="color:#FFD54F">Mechanism 1: Constraints</span>**

Examples:

```sql
PRIMARY KEY
FOREIGN KEY
UNIQUE
NOT NULL
CHECK
DEFAULT
```

These prevent invalid modifications.

---

# **<span style="color:#FFD54F">Mechanism 2: Cascading Actions</span>**

Suppose:

Department

```text
DeptID=10
```

Student references DeptID=10.

Delete Department.

What should happen?

---

### CASCADE

Automatically delete dependent rows.

```sql
ON DELETE CASCADE
```

---

### SET NULL

```sql
ON DELETE SET NULL
```

Student.DeptID becomes NULL.

---

### RESTRICT

Prevent deletion.

```sql
ON DELETE RESTRICT
```

Most common.

---

# **<span style="color:#FFD54F">Mechanism 3: Transactions</span>**

Suppose:

Transfer ₹1000.

```text
Account A -= 1000
Account B += 1000
```

System crashes after first step.

Money lost.

---

Transaction ensures:

```text
All
or
Nothing
```

This is ACID.

Without transactions:

modification anomalies become catastrophic.

---

# **<span style="color:#FFD54F">Mechanism 4: Locking and Concurrency Control</span>**

Two users:

```text
A updates salary
B updates salary
```

simultaneously.

Can create:

```text
Lost Update
Dirty Read
Non-repeatable Read
Phantom Read
```

RDBMS uses:

```text
Locks
MVCC
Timestamp Ordering
```

to maintain consistency.

---

# **<span style="color:#4FC3F7">Complete View of Modification Operators</span>**

| Operation | Mathematical Basis | Main Risk         |
| --------- | ------------------ | ----------------- |
| Insert    | Union              | Insertion anomaly |
| Delete    | Difference         | Deletion anomaly  |
| Update    | Delete + Insert    | Update anomaly    |

---

# **<span style="color:#4FC3F7">Most Important Interview Insight</span>**

A common interview question is:

> "Why do insertion, deletion, and update anomalies occur?"

The root cause is **poor schema design and redundancy**.

If the same fact is stored in multiple places:

- Insert becomes difficult → Insertion anomaly
- Delete removes extra facts → Deletion anomaly
- Update requires multiple changes → Update anomaly

The relational model solves this primarily through:

1. **Normalization**
2. **Integrity Constraints**
3. **Foreign Keys**
4. **Transactions (ACID)**
5. **Concurrency Control**

Together, these mechanisms allow relational databases to modify data while preserving correctness, consistency, and recoverability.
