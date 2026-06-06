# **<span style="color:#4FC3F7">Relational Algebra: The 6 Fundamental Operators</span>**

Relational Algebra is the **mathematical foundation of SQL and relational databases**.

A relational database can be viewed mathematically as a collection of **relations (tables)** and relational algebra provides operations to manipulate those relations.

Think of it like:

- Numbers → Arithmetic (+, -, ×, ÷)
- Sets → Set Theory Operations (∪, ∩, −)
- Tables → Relational Algebra Operations

The six operators you mentioned are considered the fundamental building blocks:

1. Selection (σ)
2. Projection (π)
3. Cartesian Product (×)
4. Union (∪)
5. Set Difference (−)
6. Rename (ρ)

---

# **<span style="color:#FFD54F">Before Learning Operators: What is a Relation?</span>**

Mathematically:

A relation is a subset of a Cartesian Product.

Example:

Student

| SID | Name  | Dept |
| --- | ----- | ---- |
| 1   | Arjun | E&TC |
| 2   | Rahul | CS   |
| 3   | Priya | IT   |

Mathematically:

```text
Student ⊆ SID × Name × Dept
```

Every row is called a tuple.

```text
(1,Arjun,E&TC)
(2,Rahul,CS)
(3,Priya,IT)
```

A relation is simply a set of tuples.

---

# **<span style="color:#FFD54F">1. Selection (σ)</span>**

## **<span style="color:#81C784">Purpose</span>**

Select specific rows.

Equivalent SQL:

```sql
WHERE
```

Notation:

```text
σ(condition)(Relation)
```

---

## **<span style="color:#81C784">Mathematical Meaning</span>**

Selection is a filtering operation.

Given:

```text
R = {t | tuples}
```

Selection:

```text
σP(R)
```

means:

```text
{ t ∈ R | P(t) is true }
```

Read as:

> Keep tuple t if predicate P is true.

---

## **<span style="color:#81C784">Dry Run</span>**

Student

| SID | Name  | Dept |
| --- | ----- | ---- |
| 1   | Arjun | E&TC |
| 2   | Rahul | CS   |
| 3   | Priya | IT   |

Query:

```text
σDept='CS'(Student)
```

---

### Step 1

Take first row

```text
(1,Arjun,E&TC)
```

Check:

```text
Dept='CS' ?
```

False

Discard

---

### Step 2

```text
(2,Rahul,CS)
```

Check condition

True

Keep

---

### Step 3

```text
(3,Priya,IT)
```

False

Discard

---

Result

| SID | Name  | Dept |
| --- | ----- | ---- |
| 2   | Rahul | CS   |

---

## **<span style="color:#81C784">Constraints</span>**

Selection:

- Does not change columns
- Only reduces rows
- Schema remains same

Input:

```text
(SID,Name,Dept)
```

Output:

```text
(SID,Name,Dept)
```

Same schema.

---

## **<span style="color:#81C784">Best Use</span>**

Whenever you want:

```sql
WHERE
```

Examples:

```text
salary > 50000
age < 25
dept='CS'
```

---

# **<span style="color:#FFD54F">2. Projection (π)</span>**

## **<span style="color:#81C784">Purpose</span>**

Choose columns.

Equivalent SQL:

```sql
SELECT column_list
```

Notation:

```text
π(attribute_list)(Relation)
```

---

## **<span style="color:#81C784">Mathematical Meaning</span>**

Projection maps tuples to smaller tuples.

Given:

```text
πName(Student)
```

Keep only Name attribute.

---

## **<span style="color:#81C784">Dry Run</span>**

Student

| SID | Name  | Dept |
| --- | ----- | ---- |
| 1   | Arjun | E&TC |
| 2   | Rahul | CS   |
| 3   | Priya | IT   |

Operation

```text
πName(Student)
```

---

Row 1

```text
(1,Arjun,E&TC)
```

becomes

```text
(Arjun)
```

---

Row 2

```text
(2,Rahul,CS)
```

becomes

```text
(Rahul)
```

---

Row 3

```text
(3,Priya,IT)
```

becomes

```text
(Priya)
```

Result

| Name  |
| ----- |
| Arjun |
| Rahul |
| Priya |

---

## **<span style="color:#81C784">Duplicate Elimination</span>**

Very important.

Relational algebra assumes relations are sets.

Sets cannot contain duplicates.

Example:

| Dept |
| ---- |
| CS   |
| CS   |
| IT   |

After projection:

```text
πDept(Student)
```

Result:

| Dept |
| ---- |
| CS   |
| IT   |

Duplicate removed.

---

## **<span style="color:#81C784">Constraints</span>**

- Cannot create new rows
- Only removes columns
- Duplicate elimination occurs

---

## **<span style="color:#81C784">Best Use</span>**

When you need:

```sql
SELECT Name
SELECT Dept
SELECT Salary
```

---

# **<span style="color:#FFD54F">3. Cartesian Product (×)</span>**

## **<span style="color:#81C784">Purpose</span>**

Combine every row of one relation with every row of another.

Notation:

```text
R × S
```

---

## **<span style="color:#81C784">Mathematical Meaning</span>**

From Set Theory:

If

```text
A={1,2}
B={x,y}
```

then

```text
A×B=
{
(1,x),
(1,y),
(2,x),
(2,y)
}
```

Every combination.

---

## **<span style="color:#81C784">Dry Run</span>**

R

| A   |
| --- |
| 1   |
| 2   |

S

| B   |
| --- |
| x   |
| y   |

---

Take row 1 from R

```text
1
```

Pair with all rows of S

```text
(1,x)
(1,y)
```

---

Take row 2

```text
2
```

Pair with all rows

```text
(2,x)
(2,y)
```

Result

| A   | B   |
| --- | --- |
| 1   | x   |
| 1   | y   |
| 2   | x   |
| 2   | y   |

---

## **<span style="color:#81C784">Cardinality Rule</span>**

If

```text
|R| = m
|S| = n
```

Then

```text
|R×S| = m×n
```

---

## **<span style="color:#81C784">Constraints</span>**

Can explode in size.

Example:

```text
1000 × 1000
=
1,000,000 rows
```

Very expensive.

---

## **<span style="color:#81C784">Best Use</span>**

Foundation of joins.

Join is actually:

```text
Selection(
Cartesian Product
)
```

More later.

---

# **<span style="color:#FFD54F">4. Union (∪)</span>**

## **<span style="color:#81C784">Purpose</span>**

Merge rows from two relations.

Notation:

```text
R ∪ S
```

---

## **<span style="color:#81C784">Mathematical Meaning</span>**

Set union.

```text
A={1,2}
B={2,3}
```

Result

```text
{1,2,3}
```

---

## **<span style="color:#81C784">Dry Run</span>**

R

| ID  |
| --- |
| 1   |
| 2   |

S

| ID  |
| --- |
| 2   |
| 3   |

Combine

```text
1
2
2
3
```

Remove duplicate

```text
1
2
3
```

---

## **<span style="color:#81C784">Constraint: Union Compatibility</span>**

Must have:

Same number of columns.

Example:

Valid

```text
(ID,Name)
(ID,Name)
```

Invalid

```text
(ID,Name)
(ID)
```

---

## **<span style="color:#81C784">Best Use</span>**

Combine results.

Equivalent SQL:

```sql
UNION
```

---

# **<span style="color:#FFD54F">5. Set Difference (−)</span>**

## **<span style="color:#81C784">Purpose</span>**

Find tuples in first relation but not second.

Notation:

```text
R − S
```

---

## **<span style="color:#81C784">Mathematical Meaning</span>**

Set subtraction.

```text
A={1,2,3}
B={2}
```

Result

```text
{1,3}
```

---

## **<span style="color:#81C784">Dry Run</span>**

R

| ID  |
| --- |
| 1   |
| 2   |
| 3   |

S

| ID  |
| --- |
| 2   |

---

Check each row.

```text
1 → not in S → keep
2 → in S → remove
3 → not in S → keep
```

Result

| ID  |
| --- |
| 1   |
| 3   |

---

## **<span style="color:#81C784">Constraints</span>**

Must be union compatible.

Same schema required.

---

## **<span style="color:#81C784">Best Use</span>**

Examples:

Students not enrolled.

Employees not assigned.

Products not sold.

---

# **<span style="color:#FFD54F">6. Rename (ρ)</span>**

## **<span style="color:#81C784">Purpose</span>**

Rename relation or attributes.

Notation:

```text
ρNewName(Relation)
```

or

```text
ρ(A,B,C)(Relation)
```

---

## **<span style="color:#81C784">Why Needed?</span>**

Consider Employee table twice.

Manager and Employee are same table.

Without rename:

```text
Employee × Employee
```

Ambiguous.

Which Name?

Which ID?

---

Use:

```text
ρE1(Employee)
ρE2(Employee)
```

Now clear.

---

## **<span style="color:#81C784">Best Use</span>**

Self joins.

Temporary relation names.

Complex expressions.

---

# **<span style="color:#4FC3F7">How These 6 Operators Create Joins?</span>**

Natural Join is not fundamental.

It is derived.

Suppose

Student

| SID | Name  |
| --- | ----- |
| 1   | Arjun |

Enrollment

| SID | Course |
| --- | ------ |
| 1   | DBMS   |

---

Join:

```text
Student ⋈ Enrollment
```

Can be written as:

```text
σStudent.SID=Enrollment.SID
(
Student × Enrollment
)
```

Join = Selection + Cartesian Product

Huge insight.

---

# **<span style="color:#4FC3F7">Why Are These Operators Sufficient?</span>**

This is called **Relational Completeness**.

Theoretical result by Edgar F. Codd.

Any query expressible in relational calculus can be expressed using these primitive operators.

Think like digital logic:

```text
AND
OR
NOT
```

can build

```text
Adder
Multiplier
CPU
Computer
```

Similarly:

```text
σ
π
×
∪
−
ρ
```

can build

```text
Join
Intersection
Division
Semi Join
Outer Join
Aggregation foundations
Complex SQL queries
```

---

# **<span style="color:#4FC3F7">Derived Operators Built From These Six</span>**

## **<span style="color:#FFD54F">Intersection</span>**

Using:

```text
R ∩ S
=
R − (R − S)
```

---

## **<span style="color:#FFD54F">Theta Join</span>**

Using:

```text
σcondition(R × S)
```

---

## **<span style="color:#FFD54F">Natural Join</span>**

Using:

```text
Projection(
Selection(
Cartesian Product
))
```

---

## **<span style="color:#FFD54F">Division</span>**

Using combinations of:

```text
Projection
Difference
Cartesian Product
```

---

# **<span style="color:#4FC3F7">Deep Intuition: What Each Operator Really Does</span>**

| Operator     | Changes Rows? | Changes Columns? | Set Theory Meaning    |
| ------------ | ------------- | ---------------- | --------------------- |
| σ Selection  | Yes           | No               | Filter elements       |
| π Projection | No            | Yes              | Coordinate projection |
| × Product    | Adds rows     | Adds columns     | Cartesian Product     |
| ∪ Union      | Combines rows | No               | Set Union             |
| − Difference | Removes rows  | No               | Set Subtraction       |
| ρ Rename     | No            | No               | Relabeling            |

Every relational query fundamentally needs only four capabilities:

1. Filter tuples
2. Choose attributes
3. Combine relations
4. Add/remove tuples from sets

These six operators provide exactly those capabilities. Everything else in relational algebra is a convenient shorthand built on top of them, which is why they form the mathematical core of relational database systems and the theoretical foundation beneath SQL query processing.
