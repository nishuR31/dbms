

# Module I — DBMS Concepts and ER Modeling

## 1. DBMS Concepts

### What is DBMS

A Database Management System stores, organizes, and manages data with controlled access, integrity, and reliability.

### DBMS vs File System

| Feature           | File System | DBMS       |
| ----------------- | ----------- | ---------- |
| Redundancy        | High        | Controlled |
| Concurrency       | No          | Yes        |
| Security          | Weak        | Strong     |
| Searching         | Slow        | Optimized  |
| ACID Support      | No          | Yes        |
| Backup & Recovery | Manual      | Built-in   |

**Real example:**
A text file storing 10,000 user records will require full scanning. A DB table indexed on `email` fetches results in O(log n).

---

## 2. Schema and Instance

| Term     | Meaning             | Example                   |
| -------- | ------------------- | ------------------------- |
| Schema   | Structure/blueprint | `Students(id, name, age)` |
| Instance | Actual data         | `(1, 'Nishu', 20)`        |

---

## 3. Data Independence

| Type        | Description                                         |
| ----------- | --------------------------------------------------- |
| Logical DI  | Change in schema without affecting application      |
| Physical DI | Change in internal storage without affecting schema |

---

## 4. DB Languages

### DDL

```sql
CREATE TABLE student (
  id INT PRIMARY KEY,
  name VARCHAR(50)
);
```

### DML

```sql
INSERT INTO student VALUES (1, 'Nishu');
```

### DQL

```sql
SELECT * FROM student;
```

### DCL

```sql
GRANT SELECT ON student TO user1;
```

### TCL

```sql
COMMIT;
ROLLBACK;
```

---

## 5. ER Modeling

### Entities, Attributes, Relationships

Example entities:

* Student(id, name, age)
* Course(cid, cname)
* Relationship: Student–Enrolls–Course

### Keys

| Type          | Meaning                | Example              |
| ------------- | ---------------------- | -------------------- |
| Super Key     | Any unique combination | {id}, {email, phone} |
| Candidate Key | Minimal Super Key      | {id}, {email}        |
| Primary Key   | Main chosen key        | {id}                 |

### Generalization

Common parent entity.
Example:
Employee → Manager, Developer

### Aggregation

Representing relationship as entity.
Example:
Student + Project → Assignment entity

### ER Diagram to Table Conversion

* Entity → Table
* Relationship 1:M → FK in many-side
* Relationship M:N → Separate mapping table
* Weak entity → PK + FK combination

Example M:N:

```
Student(id)
Course(cid)
Enrollment(id, cid, date)
```

---

# Module II — Relational Model and SQL

## 1. Integrity Constraints

| Type        | Description      | Example               |
| ----------- | ---------------- | --------------------- |
| Domain      | Valid data type  | Age > 0               |
| Entity      | PK not null      | id cannot be NULL     |
| Referential | FK must match PK | student_id must exist |

---

## 2. Relational Algebra (RA)

| Operator | Meaning       | SQL Equivalent |
| -------- | ------------- | -------------- |
| σ        | Selection     | WHERE          |
| π        | Projection    | SELECT columns |
| ∪        | Union         | UNION          |
| ∩        | Intersection  | INTERSECT      |
| −        | Difference    | EXCEPT         |
| ×        | Cross product | Cartesian join |
| ⨝        | Join          | JOIN           |

Example RA:

```
π name (σ age > 18 (Student))
```

SQL Version:

```sql
SELECT name FROM student WHERE age > 18;
```

---

## 3. SQL Concepts

### SQL Data Types

INT, VARCHAR, TEXT, DATE, FLOAT, JSON, ENUM

### SQL Commands and Examples

#### Select, Insert, Update, Delete

```sql
SELECT name FROM student;

INSERT INTO student VALUES (2, 'Aditi', 21);

UPDATE student SET age = 22 WHERE id = 2;

DELETE FROM student WHERE id = 2;
```

### Joins

```sql
SELECT s.name, c.cname
FROM student s
JOIN enrollment e ON s.id = e.sid
JOIN course c ON c.cid = e.cid;
```

### Views

```sql
CREATE VIEW active_students AS
SELECT name FROM student WHERE active = 1;
```

### Index

```sql
CREATE INDEX idx_email ON users(email);
```

### Aggregate Functions

```sql
SELECT AVG(marks), COUNT(*) FROM students;
```

### Subqueries

```sql
SELECT name 
FROM student
WHERE marks > (SELECT AVG(marks) FROM student);
```

### Triggers

```sql
CREATE TRIGGER log_delete
AFTER DELETE ON student
FOR EACH ROW
INSERT INTO logs(message) VALUES ('Student deleted');
```

---

# Module III — Database Design & Normalization

## 1. Functional Dependencies (FD)

A → B means A determines B.

Example:
roll → name, age

### Types of Dependencies

* Partial
* Transitive
* Multivalued

---

## 2. Normal Forms

### 1NF

No repeating groups.

Bad:

```
phone = 9876,4567
```

Good:
Separate table for phone numbers.

### 2NF

No partial dependency on composite key.

### 3NF

No transitive dependency.

### BCNF

Every determinant must be a candidate key.

---

## 3. Decomposition Rules

### Lossless Decomposition

R → R1 ⋈ R2 restores original.

### Dependency Preservation

All FDs must remain representable.

---

## 4. Query Optimization

### Heuristic Rules

* Apply filters early
* Project only needed columns
* Replace product + filter with join

### Cost-Based

DB chooses optimal plan using statistics.

---

# Module IV — Transaction Processing

## 1. Transaction and ACID

### ACID Properties

| Property    | Meaning                   |
| ----------- | ------------------------- |
| Atomicity   | All or nothing            |
| Consistency | Must maintain rules       |
| Isolation   | Should not affect another |
| Durability  | Permanent after commit    |

---

## 2. Schedules

### Conflict Types

* Read–Write
* Write–Read
* Write–Write

### Conflict Serializability

No conflicting operations appear out of order.

### View Serializability

Based on:

* Reads-from relation
* Writes-from relation
* Final writes

---

## 3. Recovery Systems

### Techniques

* Immediate update
* Deferred update
* Log-based recovery

### Checkpoints

Used to reduce log processing time.

---

# Module V — Concurrency Control & Distributed Databases

## 1. Concurrency Control

### Locking

| Lock          | Meaning |
| ------------- | ------- |
| Shared (S)    | Read    |
| Exclusive (X) | Write   |

### Two-Phase Locking (2PL)

* Growing Phase: acquire locks
* Shrinking Phase: release locks

Guarantees serializability.

---

## 2. Deadlocks

### Detection

Wait-for graph.

### Prevention

Resource ordering, timeouts.

---

## 3. Timestamping Protocol

Older timestamp transactions get priority.
No deadlock but may cause starvation.

---

## 4. Multiversion Concurrency Control (MVCC)

Multiple versions exist. Readers never block writers.
Used in PostgreSQL, MySQL InnoDB.

---

## 5. Distributed Databases

### Key Concepts

* Fragmentation
* Replication
* 2 Phase Commit (2PC)
* Distributed query processing
* Parallel databases

---
---
---

# Module I — DBMS Concepts and ER Modeling

## 1. DBMS Concepts

### Purpose of DBMS

A DBMS provides:

* Centralized storage
* Controlled access
* Multi-user concurrent operations
* Data integrity & security
* High-level querying language
* Backup & recovery

A DBMS behaves like a structured, optimized infrastructure layer between applications and raw data.

### Why File Systems Fail

File systems store data in isolated text/binary files.
Key limitations:

1. No atomicity: partial updates cause corruption.
2. No concurrency: simultaneous writes overwrite each other.
3. No integrity checks: duplicates and incorrect types appear easily.
4. Difficult queries: requires manual parsing; no indexing.
5. No security model: any user may access any file.
6. No recovery system: after crash, data may become inconsistent.

DBMS solves all of this through ACID, indexing, schema constraints, isolation, locks, and a query language.

---

## 2. Schema, Metadata, and Instance

### Schema

A logical description of the structure of the database:

* tables
* columns
* constraints
* relationships
* views
* indexes

Schema is static (changes rarely).

### Instance

Actual database content at any moment.
Instances change frequently as data is inserted/updated/deleted.

Example:

Schema:

```
Student(id INT, name VARCHAR(50), dept VARCHAR(20))
```

Instance:

```
1 | Aditi | CS
2 | Nishu | IT
```

---

## 3. Data Independence

A crucial feature in maintaining large systems over time.

### Physical Data Independence

Internal storage structures can change (indexes, file organization, compression) without affecting external schema.

Example:

* Adding a B+ tree index
* Switching storage from SSD to cloud block storage
  Application code remains same.

### Logical Data Independence

Changes in logical schema do not affect application views.

Example:

* Adding a new column `gpa`
* Splitting table into two normalized tables
  Application that uses a view still works.

DBMSs like MySQL, Oracle, PostgreSQL enforce this separation through abstraction layers.

---

## 4. DBMS Architecture (Three-Level Model)

### External Level

User-specific views.

### Conceptual Level

Logical design of data.
ER structure, relationships, constraints.

### Internal Level

Physical storage details:

* indexes
* hashing
* compression
* page organization
* file layouts

This model provides abstraction and independence.

---

## 5. DB Languages

### DDL – defines structure

```sql
CREATE TABLE employee (
  id INT PRIMARY KEY,
  name VARCHAR(50),
  salary DECIMAL(10,2)
);
```

### DML – data manipulation

```sql
UPDATE employee SET salary = salary + 1000 WHERE id = 2;
```

### DCL – permissions

```sql
GRANT SELECT, INSERT ON employee TO user1;
```

### TCL – transaction control

```sql
BEGIN;
DELETE FROM employee WHERE id=3;
ROLLBACK;
```

---

## 6. ER Modeling (Entity–Relationship)

ER modeling captures:

* entities
* attributes
* relationships
* constraints
* keys

### Entity Types

Strong entity → has primary key
Weak entity → depends on another entity

Example:
Room depends on Building → (RoomNo, BuildingID)

### Relationship Cardinality

* 1:1
* 1:N
* N:M

Example:
Student–Enrolled–Course → many-to-many
Resolved using Enrollment table.

### Participation Constraints

Total participation: every instance must participate
Partial participation: optional involvement

### Attributes Types

| Type        | Meaning           | Example             |
| ----------- | ----------------- | ------------------- |
| Simple      | cannot be divided | age                 |
| Composite   | can be divided    | name → fname, lname |
| Derived     | computed          | age from DOB        |
| Multivalued | multiple values   | phone numbers       |

---

## 7. Keys in ER

### Super Key

Any attribute set that uniquely identifies a tuple.

### Candidate Key

Minimal super key.

### Primary Key

Chosen candidate key.

### Alternate Keys

Other candidate keys not selected as PK.

Example Table:

```
(id, roll, email)
```

Super keys: {id}, {roll}, {email}, {email, roll}
Candidate keys: {id}, {roll}, {email}
Primary key: {id}

---

## 8. ER Diagram to Table Conversion

### Rule 1: Entity → Table

Entity attributes → table columns
PK → primary key

### Rule 2: 1:N

Foreign key on N-side.

Example:
Department (DeptID) — 1
Employee (EmpID, DeptID) — N

### Rule 3: N:M

Create a new table with 2 foreign keys.

### Rule 4: Weak Entity

PK = own partial key + owner PK
FK constraint required.

---

# Module II — Relational Model and SQL

## 1. Relational Model

### Structure

Data is represented in:

* Tables (relations)
* Rows (tuples)
* Columns (attributes)

### Properties of a Relation

* No duplicate tuples
* Order of rows/columns does not matter
* Each attribute has a domain
* Atomic values (1NF)

---

## 2. Types of Integrity Constraints

### Domain Constraint

Column must follow defined type/domain.

Example:

```sql
age INT CHECK(age > 0)
```

### Entity Integrity

Primary key must:

* be unique
* not be NULL

### Referential Integrity

Foreign key must refer to an existing primary key.

Example:

```sql
FOREIGN KEY (dept_id) REFERENCES department(id)
```

---

## 3. Relational Algebra (Detailed)

### 1. Selection (σ)

Filters rows.

```
σ age > 20 (Student)
```

### 2. Projection (π)

Selects columns.

```
π name, age (Student)
```

### 3. Union (∪)

Tables must be union-compatible.

### 4. Set Difference (−)

Rows in A not in B.

### 5. Cartesian Product (×)

Used rarely; expensive.

### 6. Join (⨝)

Combines rows based on condition.

Example:

```
Student ⨝ Student.id = Enrollment.sid Enrollment
```

### 7. Division (÷)

Used for "for all" queries.

Example:
Find students who enrolled in all courses.

---

## 4. SQL In Depth

### Data Types

| Category  | Types                       |
| --------- | --------------------------- |
| Numeric   | INT, BIGINT, FLOAT, DECIMAL |
| String    | CHAR, VARCHAR, TEXT         |
| Date/Time | DATE, TIME, DATETIME        |
| Misc      | JSON, ENUM, BOOLEAN         |

---

## 5. SQL Queries

### Join Types

#### Inner Join

```sql
SELECT s.name, c.title
FROM student s
JOIN course c ON s.cid = c.id;
```

#### Left Join

Keeps all left rows.

#### Right Join

Keeps all right rows.

#### Full Join

Keeps all rows from both sides.

#### Cross Join

Produces N × M combinations.

---

## 6. Subqueries

### Single Row

```sql
SELECT name
FROM student
WHERE age = (SELECT MAX(age) FROM student);
```

### Multi Row

```sql
SELECT name
FROM student
WHERE age IN (SELECT age FROM student WHERE dept='CS');
```

---

## 7. Views

Used for abstraction.

```sql
CREATE VIEW cs_students AS
SELECT name, roll FROM student WHERE dept = 'CS';
```

---

## 8. Indexing

### Purpose

Improves searching speed.

### Types

* B-tree index
* Hash index
* Full-text index

Example:

```sql
CREATE INDEX idx_roll ON student(roll);
```

---

## 9. Triggers

```sql
CREATE TRIGGER after_insert
AFTER INSERT ON student
FOR EACH ROW
INSERT INTO audit(msg) VALUES ('Inserted');
```

---

# Module III — Normalization and Dependencies

## 1. Functional Dependencies (FD)

### Definition

A → B means:
For every A value, B value is uniquely determined.

Examples:

* roll → name
* email → id
* id → email, name, dept

### Properties

* Reflexive
* Augmentation
* Transitivity

---

## 2. Normalization Forms (Detailed)

### First Normal Form (1NF)

No multivalued or composite attributes.

### Second Normal Form (2NF)

No partial dependency.

Example bad design:

```
(roll, course) → marks
roll → student_name
```

### Third Normal Form (3NF)

No transitive dependency.

### Boyce–Codd Normal Form (BCNF)

Every determinant must be a candidate key.

Example violation:

```
teacher → subject
subject → room
```

---

## 3. Lossless Decomposition

Condition:
R = R1 ⋈ R2 must hold.

### How to check

Common attribute must be:

* a key of at least one relation

---

## 4. Dependency Preservation

All FDs must remain enforceable on decomposed tables.

---

# Module IV — Transaction Processing

## 1. Transaction Properties

### Atomicity

Transaction must execute fully or not at all.

### Consistency

DB must remain valid.

### Isolation

Intermediate results of a transaction must be hidden from others.

### Durability

Changes after commit persist even after crash.

---

## 2. Schedules

### Serial Schedule

One transaction runs completely then the next.

### Non-serial Schedule

Interleaving operations.

### Conflicts

Only these conflict:

* R-W
* W-R
* W-W

### Conflict Serializable

If equivalent to a serial schedule via swapping non-conflicting actions.

---

## 3. Recoverability

### Types

| Type        | Meaning                    |
| ----------- | -------------------------- |
| Recoverable | Child commits after parent |
| Cascadeless | No dirty reads             |
| Strict      | No R/W on uncommitted data |

Strict is strongest.

---

## 4. Recovery Techniques

### Log-based Recovery

Log contains:

* start T
* write T, A, old, new
* commit T

### Checkpoint

Reduces log scanning on recovery.

---

# Module V — Concurrency Control

## 1. Lock-Based Protocols

### Shared (S) Lock

For reading.

### Exclusive (X) Lock

For writing.

### Two-Phase Locking (2PL)

1. Growing phase: obtain locks
2. Shrinking phase: release locks

Guarantees conflict serializability.

---

## 2. Deadlocks

### Detection using Wait-for Graph

Cycle indicates deadlock.

### Prevention

* Timestamp ordering
* One-directional resource ordering
* No-wait protocol

---

## 3. Timestamp Ordering Protocol

Older transactions get priority.

Rules:

* If TS(T) < TS of conflicting write/read, allow
* Otherwise abort younger transaction

Ensures serializability.

---

## 4. Multiversion Concurrency Control (MVCC)

Keeps multiple versions:

* readers get snapshot
* writers create new version

Used in PostgreSQL and InnoDB to avoid read locks.

---

## 5. Distributed Databases

### Key Concepts

* Fragmentation (horizontal/vertical)
* Replication (master-slave, multi-master)
* Distributed commits (2PC, 3PC)
* Consistency levels (strong, eventual)

### Two-Phase Commit (2PC)

1. Prepare
2. Commit

Ensures atomic distributed transactions.

---
---
---
Alright Nishu — let’s rebuild this whole topic cleanly, from the ground up, in a **GitHub-friendly, deeply descriptive, exam-proof** format.
I’ll fix the notations, remove the confusing shorthand, and give **clear, stepwise logic + tables + conflict checks + complete examples**.

No fluff. No shortcuts. Just proper DBMS.

---

# Timestamp Ordering Protocol + Serializability + Recoverability

### Full Descriptive Notes (GitHub-friendly)

---

# 1. Transactions and Operations

A **transaction** is a sequence of read/write operations executed as a single unit.

### Notation (clear, simple)

* `T1`, `T2`, `T3` → Transactions
* `R1(X)` → Transaction T1 **Reads** item X
* `W2(Y)` → Transaction T2 **Writes** item Y
* `C1` → Commit of T1
* `A1` → Abort of T1

### Example Transaction T1

```
R1(X)
X = X + 10
W1(X)
C1
```

### Conflicting Operations (VERY IMPORTANT)

Two operations **conflict** if:

| Operation 1 | Operation 2 | Same Data Item? | Conflict? |
| ----------- | ----------- | --------------- | --------- |
| Read        | Read        | Yes             | No        |
| Read        | Write       | Yes             | Yes       |
| Write       | Read        | Yes             | Yes       |
| Write       | Write       | Yes             | Yes       |

A conflict exists when both:

1. Operations belong to **different transactions**
2. Both operate on **same data item**
3. At least one is **Write**

---

# 2. Types of Serializability

There are **two main types**:

1. **Conflict Serializability**
2. **View Serializability**

Let’s break both down.

---

# 3. Conflict Serializability

A schedule is conflict-serializable **if it can be converted into a serial schedule** by swapping non-conflicting operations.

### Steps to Check Conflict Serializability

1. Identify all conflicting operation pairs.
2. Draw a **precedence graph** (also called a serialization graph).
3. Nodes = transactions
4. Direction: `Ti → Tj` if Ti’s conflicting operation occurs earlier than Tj’s
5. If graph has a cycle → **Not conflict serializable**
6. If no cycle → **Conflict serializable**

---

## Example 1 (clear + explainable)

Schedule S:

```
R1(X)
W2(X)
W1(X)
C1
C2
```

### Step 1: Identify Conflicts

Check all pairs:

* `R1(X)` and `W2(X)` → Read/Write → Conflict → direction: T1 → T2
* `W2(X)` and `W1(X)` → Write/Write → Conflict → direction: T2 → T1

### Step 2: Build Precedence Graph

Edges:

* T1 → T2
* T2 → T1

### Step 3: Check Cycle

T1 → T2 → T1
Cycle exists → **Not conflict serializable**

---

## Example 2 (no cycle)

```
R1(A)
W1(A)
R2(A)
W2(A)
```

Conflicts:

* `W1(A)` before `R2(A)` → T1 → T2
* `W1(A)` before `W2(A)` → T1 → T2

Graph:

* T1 → T2

No cycle → **Conflict serializable**
Equivalent serial order = **T1 → T2**

---

# 4. View Serializability

More relaxed than conflict-serializability.

A schedule is **view-serializable** if:

1. **Same initial reads** (each T reads the same initial value)
2. **Same intermediate reads** (if Ti reads value written by Tj → must match)
3. **Same final writes** (final write on each item must match)

Even if conflicts exist, view-serializable may still hold.

### Example (View-Serializable but Not Conflict-Serializable)

```
W1(X)
W2(X)
C1
C2
```

Conflicts:

* W1(X) → W2(X) (T1 → T2)

Graph has no cycle → conflict serializable.

Now reverse:

```
W2(X)
W1(X)
```

Conflicts:

* T2 → T1

Both are conflict serializable.

But consider this:

```
W1(X)
W2(X)
W1(X)
```

This may break conflict serializability but still achieve same final write → view-serializable.

---

# 5. Recoverability, Cascading, Strictness

These are exam favourites.
Let’s define them cleanly.

---

## 5.1 Recoverable Schedule

Definition:
A schedule is **recoverable** if **no transaction commits until the transaction it depends on commits**.

Meaning:
If `T2` reads a value written by `T1` (W1(X) → R2(X)), then:

```
C1 must occur before C2
```

### Example (Recoverable)

```
W1(X)
R2(X)
C1
C2
```

Correct because T2 commits **after** T1.

### Example (Non-Recoverable = Irrecoverable)

```
W1(X)
R2(X)
C2
C1
```

T2 committed before T1 → If T1 aborts, T2 used dirty data → cannot recover.

This is **irrecoverable schedule**.

---

## 5.2 Cascading Abort (Cascading Rollback)

Definition:
If T2 reads uncommitted data from T1 (dirty read), and T1 aborts, T2 must also abort → cascading.

### Example (Cascading)

```
W1(X)
R2(X)   ← dirty read
A1      ← abort
A2      ← must abort
```

This is **recoverable**, but has cascading aborts.

---

## 5.3 Cascadeless Schedule

Cascadeless means:
**No transaction ever reads uncommitted data.**

Rule:
Before T2 does `R2(X)`, the writer T1 must commit.

### Example (Cascadeless)

```
W1(X)
C1
R2(X)
```

No dirty read → no need for cascaded abort.

---

## 5.4 Strict Schedule (Strongest)

Definition:
A schedule is **strict** if:

* No Read or Write of X until previous Write(X) commits.

Rule:
If T1 wrote X, no other transaction can read/write X until T1 commits.

### Why Strict > Cascadeless?

Strict schedules prevent:

* Dirty reads
* Dirty writes
* Cascading aborts
* Lost updates

Most DBMSs (MySQL InnoDB, PostgreSQL) implement strictness automatically.

---

# 6. Timestamp Ordering Protocol (TO Protocol)

This is a concurrency control method that ensures **serializability using timestamps**.

### Purpose

Every transaction gets a timestamp:

* Smaller timestamp → older transaction
* Larger timestamp → younger transaction

Timestamp ordering enforces serial order based on timestamps.

---

## 6.1 Timestamp Fields for Each Data Item X

Every item X has:

1. **Read Timestamp (RTS(X))**
   = largest timestamp of any T that successfully read X

2. **Write Timestamp (WTS(X))**
   = largest timestamp of any T that successfully wrote X

These define whether an incoming operation is allowed.

---

## 6.2 Rules of Timestamp Protocol

### Case 1: Read Request `R(Ti, X)`

Read is allowed if:

```
TS(Ti) ≥ WTS(X)
```

Else:

* Ti is trying to read an **old version** → Ti aborted.

After valid read:

```
RTS(X) = max(RTS(X), TS(Ti))
```

---

### Case 2: Write Request `W(Ti, X)`

Write is allowed if:

```
TS(Ti) ≥ RTS(X)
TS(Ti) ≥ WTS(X)
```

If either condition fails → Ti aborts.

After valid write:

```
WTS(X) = TS(Ti)
```

---

## 6.3 Example (Fully Explained)

Initial Values:

```
RTS(X) = 0
WTS(X) = 0
```

Transactions:

```
TS(T1) = 5
TS(T2) = 10
```

Schedule:

```
R1(X)
W2(X)
W1(X)   ← Will this succeed?
```

### Step 1: R1(X)

TS(T1)=5 ≥ WTS(X)=0 → allowed
Update:

```
RTS(X) = max(0,5) = 5
```

### Step 2: W2(X)

TS(T2)=10 ≥ RTS(X)=5 → allowed
TS(T2)=10 ≥ WTS(X)=0 → allowed
Update:

```
WTS(X) = 10
```

### Step 3: W1(X)

TS(T1)=5
Check conditions:

1. 5 ≥ RTS(X)=5 → OK
2. 5 ≥ WTS(X)=10 → FAILED

Younger transaction already wrote X, older cannot overwrite.
**Timestamp protocol aborts T1.**

---

## Why Timestamp Protocol Ensures Serializability?

Because:

* It enforces order based strictly on timestamps
* No cycles are possible in precedence graph
* Older transactions always appear first

---

# 7. Checking Serializability + Recoverability Together

### Example Schedule

```
W1(X)
R2(X)
W2(Y)
R1(Y)
C1
C2
```

### Step 1: Conflicts

Check each pair:

* W1(X) before R2(X) → T1 → T2
* W2(Y) before R1(Y) → T2 → T1

Cycle exists → Not conflict serializable.

### Step 2: Recoverability

Since T2 reads X from T1:

```
C1 must happen before C2.
```

Given:

```
C1
C2
```

This is **recoverable**.

If instead:

```
C2
C1
```

It becomes:

* Irrecoverable
* Dirty read committed early

---

# 8. Summary Table (Exam-Perfect)

| Property    | Dirty Read Allowed? | Cascading Abort? | Safe?     |
| ----------- | ------------------- | ---------------- | --------- |
| Recoverable | Yes                 | Yes              | Medium    |
| Cascadeless | No                  | Yes              | High      |
| Strict      | No                  | No               | Very High |

---



