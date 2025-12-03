

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

