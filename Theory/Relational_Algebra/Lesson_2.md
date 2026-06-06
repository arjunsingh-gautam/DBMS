# **<span style="color:#4FC3F7">Derived Operators in Relational Algebra</span>**

The 6 fundamental operators are enough to express any query, but writing complex queries only with those primitives becomes cumbersome.

For convenience, relational algebra defines **derived operators**.

Think of them like this:

```text
Machine Code  -> Fundamental Operators
High-Level Language -> Derived Operators
```

Just as a compiler converts:

```cpp
a += b;
```

into many machine instructions,

a DBMS converts:

```text
R ⋈ S
```

into combinations of:

```text
σ, π, ×, ∪, −, ρ
```

---

# **<span style="color:#FFD54F">1. Join (⋈)</span>**

## **<span style="color:#81C784">Purpose</span>**

Combine tuples from two relations based on a condition.

Mathematically:

```text
R ⋈θ S
```

where θ is some condition.

Example:

```text
Student.SID = Enrollment.SID
```

---

## **<span style="color:#81C784">Mathematical Definition</span>**

Join is not primitive.

It is defined as:

```text
R ⋈θ S
=
σθ(R × S)
```

This is extremely important.

Join = Selection + Cartesian Product

---

## **<span style="color:#81C784">Dry Run</span>**

Student

| SID | Name  |
| --- | ----- |
| 1   | Arjun |
| 2   | Rahul |

Enrollment

| SID | Course |
| --- | ------ |
| 1   | DBMS   |
| 2   | OS     |

---

### Step 1: Cartesian Product

```text
Student × Enrollment
```

Produces:

| SID | Name  | SID | Course |
| --- | ----- | --- | ------ |
| 1   | Arjun | 1   | DBMS   |
| 1   | Arjun | 2   | OS     |
| 2   | Rahul | 1   | DBMS   |
| 2   | Rahul | 2   | OS     |

Total:

```text
2 × 2 = 4 rows
```

---

### Step 2: Selection

Condition:

```text
Student.SID = Enrollment.SID
```

Keep:

| SID | Name  | SID | Course |
| --- | ----- | --- | ------ |
| 1   | Arjun | 1   | DBMS   |
| 2   | Rahul | 2   | OS     |

---

Result = Join

---

## **<span style="color:#81C784">Constraints</span>**

No schema compatibility required.

Need a meaningful join condition.

---

## **<span style="color:#81C784">Best Use</span>**

Combining related tables.

Examples:

- Student + Enrollment
- Customer + Orders
- Employee + Department

---

# **<span style="color:#FFD54F">2. Natural Join (⋈)</span>**

## **<span style="color:#81C784">Purpose</span>**

Automatically join on columns having same name.

---

## **<span style="color:#81C784">Mathematics</span>**

Natural Join:

```text
R ⋈ S
```

Equivalent to:

```text
Projection(
Selection(
R × S
)
)
```

where:

1. Equal named attributes must match
2. Duplicate join columns removed

---

## **<span style="color:#81C784">Dry Run</span>**

Student

| SID | Name  |
| --- | ----- |
| 1   | Arjun |
| 2   | Rahul |

Enrollment

| SID | Course |
| --- | ------ |
| 1   | DBMS   |
| 2   | OS     |

---

### Product

4 rows generated.

---

### Selection

Condition automatically inferred:

```text
Student.SID=Enrollment.SID
```

Keep matching rows.

---

### Projection

Remove duplicate SID.

Result:

| SID | Name  | Course |
| --- | ----- | ------ |
| 1   | Arjun | DBMS   |
| 2   | Rahul | OS     |

---

## **<span style="color:#81C784">Constraints</span>**

Attribute names matter.

Danger:

Same name but different meaning.

Example:

```text
Department.ID
Student.ID
```

Natural join may be wrong.

---

## **<span style="color:#81C784">Best Use</span>**

Quick joins when schemas are carefully designed.

---

# **<span style="color:#FFD54F">3. Outer Join</span>**

## **<span style="color:#81C784">Problem With Normal Join</span>**

Normal join loses unmatched rows.

Student

| SID | Name  |
| --- | ----- |
| 1   | Arjun |
| 2   | Rahul |
| 3   | Priya |

Enrollment

| SID | Course |
| --- | ------ |
| 1   | DBMS   |
| 2   | OS     |

Priya disappears.

---

# **<span style="color:#FFD54F">Left Outer Join (⟕)</span>**

Keep all rows from left table.

Missing values become NULL.

Result:

| SID | Name  | Course |
| --- | ----- | ------ |
| 1   | Arjun | DBMS   |
| 2   | Rahul | OS     |
| 3   | Priya | NULL   |

---

## **<span style="color:#81C784">Mathematics</span>**

Conceptually:

```text
Inner Join
+
Unmatched Left Rows
```

---

## **<span style="color:#81C784">Best Use</span>**

Find missing relationships.

Example:

Students without enrollments.

---

# **<span style="color:#FFD54F">Right Outer Join (⟖)</span>**

Keep every row from right relation.

Example:

Enrollment

| SID | Course |
| --- | ------ |
| 1   | DBMS   |
| 2   | OS     |
| 4   | AI     |

Result:

| SID | Name  | Course |
| --- | ----- | ------ |
| 1   | Arjun | DBMS   |
| 2   | Rahul | OS     |
| 4   | NULL  | AI     |

---

## **<span style="color:#81C784">Best Use</span>**

Find orphan records in right table.

---

# **<span style="color:#FFD54F">Full Outer Join (⟗)</span>**

Keep all rows from both sides.

Result:

| SID | Name  | Course |
| --- | ----- | ------ |
| 1   | Arjun | DBMS   |
| 2   | Rahul | OS     |
| 3   | Priya | NULL   |
| 4   | NULL  | AI     |

---

## **<span style="color:#81C784">Best Use</span>**

Data reconciliation.

Comparing datasets.

---

# **<span style="color:#FFD54F">4. Intersection (∩)</span>**

## **<span style="color:#81C784">Purpose</span>**

Common tuples in both relations.

---

## **<span style="color:#81C784">Mathematical Meaning</span>**

Set Theory:

```text
A={1,2,3}
B={2,3,4}
```

Result:

```text
A∩B={2,3}
```

---

## **<span style="color:#81C784">Derived Form</span>**

Using primitives:

```text
R ∩ S
=
R − (R − S)
```

---

## **<span style="color:#81C784">Dry Run</span>**

R

```text
1
2
3
```

S

```text
2
3
4
```

---

Compute:

```text
R-S
=
{1}
```

Then:

```text
R-(R-S)
=
{2,3}
```

---

## **<span style="color:#81C784">Constraint</span>**

Union compatible schemas.

---

## **<span style="color:#81C784">Best Use</span>**

Students taking both courses.

Employees in both projects.

---

# **<span style="color:#FFD54F">5. Division (÷)</span>**

This is the most difficult relational algebra operator.

---

## **<span style="color:#81C784">Purpose</span>**

Answer:

> "Who is related to ALL values?"

---

## **<span style="color:#81C784">Example</span>**

Enrollment

| Student | Course |
| ------- | ------ |
| A       | DBMS   |
| A       | OS     |
| A       | AI     |
| B       | DBMS   |
| B       | OS     |

Required:

Students who completed ALL courses:

```text
{DBMS,OS}
```

Answer:

```text
A
B
```

---

## **<span style="color:#81C784">Mathematics</span>**

Given:

```text
R(X,Y)
S(Y)
```

Division:

```text
R ÷ S
```

returns:

```text
X values paired with every Y in S
```

---

## **<span style="color:#81C784">Intuition</span>**

Multiplication:

```text
2 × 3 = 6
```

Division:

```text
6 ÷ 3 = 2
```

Similarly:

```text
(Student,Course)
÷
(Course)
=
(Student)
```

---

## **<span style="color:#81C784">Best Use</span>**

"ALL" queries.

Examples:

- Student completed all mandatory courses.
- Supplier supplies every part.
- Employee knows every required skill.

---

# **<span style="color:#FFD54F">6. Assignment (←)</span>**

## **<span style="color:#81C784">Purpose</span>**

Store intermediate result.

Notation:

```text
Temp ← expression
```

---

## **<span style="color:#81C784">Example</span>**

```text
CSStudents ← σDept='CS'(Student)

Result ← πName(CSStudents)
```

---

## **<span style="color:#81C784">Why Needed</span>**

Improves readability.

Avoids repeating expressions.

---

## **<span style="color:#81C784">Equivalent Programming Analogy</span>**

```cpp
int x=5;
```

Same idea.

---

# **<span style="color:#FFD54F">7. Aggregation (γ)</span>**

Aggregation is not part of classical relational algebra.

Extended relational algebra introduces it.

---

## **<span style="color:#81C784">Purpose</span>**

Compute summaries.

Functions:

```text
COUNT
SUM
AVG
MIN
MAX
```

---

## **<span style="color:#81C784">Example</span>**

Employee

| Dept | Salary |
| ---- | ------ |
| CS   | 10     |
| CS   | 20     |
| IT   | 30     |

---

Expression

```text
γDept,AVG(Salary)
```

---

Result

| Dept | AVG |
| ---- | --- |
| CS   | 15  |
| IT   | 30  |

---

## **<span style="color:#81C784">Mathematics</span>**

Partition relation into groups.

Apply function to each group.

This is basically:

```text
Partition
+
Reduction
```

---

## **<span style="color:#81C784">Best Use</span>**

Reports.

Analytics.

Business intelligence.

---

# **<span style="color:#FFD54F">8. Generalized Projection</span>**

Classical projection:

```text
π(Name)
```

only selects columns.

---

Generalized projection allows computations.

Example:

Employee

| Name | Salary |
| ---- | ------ |
| A    | 100    |

Expression:

```text
π(Name, Salary*12)
```

---

Result

| Name | AnnualSalary |
| ---- | ------------ |
| A    | 1200         |

---

## **<span style="color:#81C784">Mathematics</span>**

Each tuple is transformed using a function.

Classical projection:

```text
f(t)=attribute selection
```

Generalized projection:

```text
f(t)=computed expression
```

---

## **<span style="color:#81C784">Best Use</span>**

Derived attributes.

Examples:

```text
MonthlySalary × 12
Price × Quantity
Age from DOB
```

---

# **<span style="color:#4FC3F7">Operator Dependency Map</span>**

```text
Fundamental Operators
│
├── Selection (σ)
├── Projection (π)
├── Product (×)
├── Union (∪)
├── Difference (-)
└── Rename (ρ)

Derived Operators
│
├── Join
│    └── σ + ×
│
├── Natural Join
│    └── σ + × + π
│
├── Intersection
│    └── -
│
├── Division
│    └── π + × + -
│
├── Outer Join
│    └── Join + Union + NULL Extension
│
├── Aggregation
│    └── Extended Algebra
│
├── Assignment
│    └── Naming Mechanism
│
└── Generalized Projection
     └── Extended Projection
```

# **<span style="color:#4FC3F7">Most Important Interview Insight</span>**

If an interviewer asks:

> "Which derived operator is hardest to understand and most likely to be asked in DBMS interviews?"

The answer is:

1. **Division (÷)** — tests understanding of "for all" queries.
2. **Natural Join** — tests understanding of schema matching.
3. **Outer Join** — tests NULL and missing-data handling.
4. **Aggregation** — bridges relational algebra and SQL's `GROUP BY`.

Mastering these four gives you a strong grasp of how high-level SQL queries are translated into the mathematical operations that relational database engines actually execute.
