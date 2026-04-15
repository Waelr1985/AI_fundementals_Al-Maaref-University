# Database Normalization — 1NF, 2NF, 3NF Table Examples

**Source:** https://www.freecodecamp.org/news/database-normalization-1nf-2nf-3nf-table-examples/

**Additional Sources:**
- https://www.digitalocean.com/community/tutorials/database-normalization
- https://www.geeksforgeeks.org/dbms/normal-forms-in-dbms/

---

## What Is Database Normalization?

Database normalization is a step-by-step approach to structuring data in a way that reduces redundancy and preserves data integrity. The process is organized into a series of normal forms — 1NF, 2NF, 3NF, and BCNF — each designed to resolve specific types of data anomalies and structural problems.

## Why Normalize?

Without normalization, databases suffer from:
- **Update anomalies** — changing data in one place but not another leads to inconsistencies
- **Insertion anomalies** — inability to add data without other unrelated data
- **Deletion anomalies** — deleting data unintentionally removes other needed data
- **Data redundancy** — the same information stored in multiple places wastes storage and creates inconsistency risks

## First Normal Form (1NF)

### Rule
Each column in a table must contain only **atomic (indivisible) values**, and each row must be uniquely identifiable. There should be no repeating groups or arrays in a single cell.

### What It Fixes
Eliminates repeating groups — no multi-valued fields.

### Example
**Before 1NF (Unnormalized):**
| StudentID | Name | Courses |
|-----------|------|---------|
| S101 | Ali | CHEM101, PHAR201 |

**After 1NF:**
| StudentID | Name | Course |
|-----------|------|--------|
| S101 | Ali | CHEM101 |
| S101 | Ali | PHAR201 |

Each cell now has one value. The composite primary key becomes (StudentID, Course).

## Second Normal Form (2NF)

### Rule
The table must satisfy 1NF, AND every non-key column must depend on the **entire** primary key — not just part of it. This means there should be no **partial dependencies**.

### What It Fixes
Removes partial dependencies — non-key attributes that depend on only part of a composite key are moved to separate tables.

### Example
**Before 2NF (in 1NF):**
| StudentID | Course | StudentName | Instructor |
|-----------|--------|-------------|------------|
| S101 | CHEM101 | Ali | Dr. Noor |

Problem: StudentName depends only on StudentID, not on (StudentID, Course).

**After 2NF:**

Students Table:
| StudentID | StudentName |
|-----------|-------------|
| S101 | Ali |

Enrollments Table:
| StudentID | Course |
|-----------|--------|
| S101 | CHEM101 |

Courses Table:
| Course | Instructor |
|--------|------------|
| CHEM101 | Dr. Noor |

## Third Normal Form (3NF)

### Rule
The table must satisfy 2NF, AND there should be no **transitive dependencies** — non-key columns should not depend on other non-key columns.

### What It Fixes
Removes transitive dependencies — if column A determines column B, and column B determines column C, then C transitively depends on A. Column C should be in a separate table.

### Example
**Before 3NF (in 2NF):**
| Course | Instructor | Department |
|--------|------------|------------|
| CHEM101 | Dr. Noor | Chemistry |

Problem: Department depends on Instructor (not directly on Course). This is a transitive dependency: Course → Instructor → Department.

**After 3NF:**

Courses Table:
| Course | Instructor |
|--------|------------|
| CHEM101 | Dr. Noor |

Instructors Table:
| Instructor | Department |
|------------|------------|
| Dr. Noor | Chemistry |

## Summary of Normal Forms

| Normal Form | Rule | What It Eliminates |
|-------------|------|-------------------|
| 1NF | Atomic values, no repeating groups | Multi-valued cells |
| 2NF | No partial dependencies | Attributes depending on part of a composite key |
| 3NF | No transitive dependencies | Attributes depending on non-key attributes |

## Benefits of Normalization

- Reduces data redundancy
- Prevents update, insertion, and deletion anomalies
- Improves data integrity and consistency
- Makes the database easier to maintain and scale
- 3NF is the most widely used normalization level in production databases
