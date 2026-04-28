# 🎓 Instructor Guide — Lesson 1.2: Data Modeling & Schema Design

> **Branch:** `feature/instructor-guide`
> **Audience:** Instructors and teaching assistants
> **Companion to:** `lesson.md`, `pre-class.md`, `assignment.md`

---

## 1. Lesson Overview & Instructor Objectives

| | |
|---|---|
| **Duration** | 3 hours |
| **Format** | Flipped Classroom + Code-Along (dbdiagram.io) |
| **Learner entry point** | Completed Lesson 1.1; familiar with basic data concepts; no SQL experience yet |

By the end of this lesson learners should be able to:

1. Choose between Relational (SQL), NoSQL, and Vector databases for a given business problem.
2. Draw and interpret an Entity-Relationship Diagram (ERD) using Primary and Foreign Keys.
3. Normalize a messy spreadsheet to 3rd Normal Form (3NF) and articulate *why* each step matters.

**Instructor's primary job:** Normalization is the hardest concept in this lesson — it requires learners to hold the "before" and "after" state in their heads simultaneously. Use the activities to make it hands-on rather than lecturing through the rules abstractly.

---

## 2. Concept Analogies

### Database Type Selection

**The "Tool for the Job" Analogy**

> "You wouldn't use a hammer to cut wood. Databases are the same — each type is optimised for a different job."

| Database Type | Analogy | When to Reach for It |
|---------------|---------|---------------------|
| Relational (SQL) | A bank vault — strict rules, organised, auditable | Financial records, user accounts, any data where *accuracy* matters more than *speed* |
| NoSQL | A messy desk — fast to throw things on, fast to grab | High-volume feeds, chat logs, sensor data — when structure is flexible or unknown upfront |
| Vector | A "related songs" playlist — finds things by *meaning*, not by exact match | Recommendation engines, image similarity search, LLM memory |

**The "ACID" question often comes up for SQL:** Explain ACID (Atomicity, Consistency, Isolation, Durability) using a bank transfer analogy. When you transfer $100, *either* it leaves your account AND arrives in theirs, *or* nothing happens. The database never leaves the money "in transit." NoSQL databases often sacrifice some ACID guarantees for speed.

---

### ERD & Primary / Foreign Keys

**The NRIC / Passport Analogy**

> "Every row in a database needs a unique identity — like a national ID number or passport. That's the Primary Key. A Foreign Key is when one table holds someone *else's* ID card to say: 'I belong to that person.'"

**The Practical Extension:**

Walk through a real scenario: an `orders` table with a `customer_id` column. Ask: "Why don't we just put the customer's name directly in the order row?" Common wrong answers: "It's faster." The correct answer: if the customer changes their name or address, you'd need to update every single order row. With a Foreign Key pointing to the customers table, you update *one cell*.

This connects directly back to normalization — so use it as a preview of Part 3.

---

### Normalization (1NF → 2NF → 3NF)

The Coffee Shop analogy in the lesson is strong — reinforce it:

> "Every time a coffee shop customer buys a drink, the cashier writes your complete home address on each receipt. If you move, they'd need to update *every receipt ever written.* That's an Update Anomaly — and Normalization fixes it."

**An alternative analogy learners find intuitive: The Contacts App**

- Imagine you have a contacts app where, instead of storing each person's phone number once, you wrote their phone number next to *every message* you sent them. If they change their number, you have to update hundreds of messages. Normalization says: put the phone number in the contact profile (one place), and link messages to the contact.

**For 3NF specifically — the Chain Dependency:**

> "Imagine an employee record that stores: EmployeeID → DepartmentCode → DepartmentName. The DepartmentName doesn't depend on the Employee — it depends on the Department Code. That's a chain, not a direct relationship. 3NF says: break the chain."

---

## 3. Real-World Use Cases

### Database Type Selection

| Industry | Real Example | Database Type | Why |
|----------|-------------|---------------|-----|
| Banking | Transaction records | Relational (PostgreSQL) | ACID compliance — a half-processed transaction is a catastrophe |
| Social Media | Instagram photo feed | NoSQL (Cassandra) | Millions of writes per second; posts don't need strict schema |
| E-Commerce | "Customers also bought..." | Vector (Pinecone) | Semantic similarity between products |
| IoT | Smart factory sensor readings | NoSQL (MongoDB) | Unstructured, high-frequency sensor events |
| HR system | Employee → Department → Benefits | Relational | Clean relationships, frequent reports |

**Discussion prompt:** "What database type does your bank use, and why?" (Almost certainly Relational — a NoSQL transaction that fails halfway would be a financial and legal nightmare.)

### ERDs in Production

Real companies maintain ERDs as living documentation. When a new engineer joins a company, the ERD is one of the first things they study to understand how the business data is structured. Bad ERD design causes bugs that take months to find — for example, storing a customer's email in 3 different tables with slightly different formats, so a search only finds 2 out of 3.

### Normalization: The E-Commerce Cost of Denormalization

Amazon's original database had serious normalization problems in their early years — one well-documented issue was that a product's category description was stored on every product record. When they rebranded category names, it required updating millions of rows. The migration took weeks and caused several incidents. After normalization, changing a category name meant updating *one row* in the categories table.

---

## 4. Activity Facilitation Notes

### Part 1: The Sorting Game (10 min)

**This is a quick warm-up** — the goal is to get learners comfortable making decisions, not to produce perfect answers.

**Scenario 3 ("Find songs that sound like Jazz") is the most valuable one.** Most learners default to "NoSQL." Push them: "But what makes two songs *similar*? It's not a keyword match — it's the actual audio features (tempo, key, instrumentation). How would you compare those?" This introduces the concept of semantic similarity and the need for vectors without getting into the math.

**Scenario 1 (profile pictures) is intentionally tricky.** Accept "NoSQL object store for the binary, SQL for the file path" or "SQL stores the path, a file system/CDN stores the file." Either is correct in practice.

---

### Part 2: ERD Activities

**Activity 2.1 — Car Insurance Code-Along (15 min)**

Before learners paste the code into dbdiagram.io, spend 2 minutes reading through it *without running it*:

> "Before we run this, tell me: how many tables do we have? What does `customer_id` in the `cars` table mean? What does the `Ref:` line do?"

This "read before run" habit is one of the most important skills to instil early. Learners who code by trial-and-error without reading rarely catch structural errors.

**The challenge (accidents table):** Common mistakes:
- Not adding a foreign key to `cars` — learners create the `accidents` table but forget to link it.
- Using a non-existent column name (e.g., `car_plate_id` instead of `car_id`).
- Making the PK an arbitrary `VARCHAR` instead of `INT`.

Use the mistake as a teaching moment: "What happens if two accidents somehow get the same ID?" → Database throws an error. That's the Primary Key doing its job.

---

**Activity 2.2 — School System Workshop (15 min)**

**The key design challenge here is the many-to-many relationship** between classes and teachers. Most learners initially write a single FK on one table and get it wrong. Guide them with questions:

- "A teacher can teach *many* classes. A class can have *many* teachers. Can we express that with just one FK?"
- "What do you do when two things are many-to-many?" → Junction/bridge table.

**If time allows:** Draw the many-to-many on a whiteboard and ask "how many rows do we need?" for a concrete example (3 teachers, 4 classes, each teacher teaches 2 classes = up to 8 rows in the junction table).

---

### Part 3: Normalization Activities

**Strongly recommend using the [Interactive Visualization](https://su-ntu-ctp.github.io/6m-data-1.2-intro-database/)** before the written explanation. Learners who skip this consistently struggle more with the abstract rules.

**Activity 3 — The E-Commerce Deconstruction (20 min)**

This is the centrepiece of the lesson. Run it as a group exercise:

Step 1: Show the messy table on a projector/screen.
Step 2: Ask: "What's wrong with this data?" Let learners identify anomalies before labelling them.
Step 3: Walk through each NF step together rather than letting groups work separately — the table transformation needs to be visible to everyone simultaneously for the concept to land.

**Common learner confusions:**

| Confusion | Clarification |
|-----------|--------------|
| "Why do we need OrderLineItems? Can't we just keep items on the order?" | Ask: "What if one order has 5 different products? How would you store that?" — Forces them to see the many-items-per-order problem. |
| "Doesn't splitting into 4 tables make queries harder?" | Yes — but that's what JOINs are for (Lesson 1.5). The trade-off is: harder to read, but impossible to corrupt. |
| "What if the customer changes their name? Does that change all their orders?" | No — the orders only store the `customer_id`. The name is fetched from the customers table at query time. |

---

**Activity 3.2 — Movie Rental Service (15 min)**

This is the independent practice. Let learners work alone or in pairs. Walk around and look for:
- Are they creating a junction table for rentals with multiple movies? (RentalID alone can't uniquely identify a row when one rental has multiple movies.)
- Are they correctly putting `genre` on the `movies` table (not the rental)?
- Are they using proper FK syntax in DBML?

**Discussion — Deliberate Denormalization:**

Bring this up at the end of Part 3. Real analytics systems (Snowflake, BigQuery) intentionally denormalize for query performance — a dashboard that queries 10 joined tables on every page load is too slow. Data Warehouses often use Star Schema (one big fact table, several small dimension tables) which relaxes some 3NF rules. The key message: normalization is the *default right choice* for transactional systems; denormalization is an informed *exception* for analytical systems.

---

## 5. Timing & Pacing Notes

| Part | Planned | Common Overrun | Mitigation |
|------|---------|---------------|-----------|
| Part 1: Database Types | 50 min | Sorting game + ACID discussion eats into Code-Along time | Keep database type intro to 15 min max; move briskly to dbdiagram.io |
| Part 2: ERD | 50 min | Many-to-many discussion can extend | Set a 5-min timer for Activity 2.2; debrief is more valuable than individual work time |
| Part 3: Normalization | 50 min | Activity 3 group work often runs 30 min+ | Lead Activity 3 as a whole-class walkthrough rather than independent work |

---

## 6. Common Learner Questions

**Q: "Why not just use a spreadsheet for everything?"**
A: Spreadsheets have no referential integrity — you can type anything in any cell. A database enforces rules. If you try to add an order for a customer who doesn't exist, the database blocks it. Excel just accepts it silently.

**Q: "Is MongoDB always the wrong choice for financial data?"**
A: Not always — some fintech companies use MongoDB with application-level ACID guarantees. But the *default* choice for financials is SQL, and switching away from that default requires a deliberate architectural decision.

**Q: "What is DBML — is it a real language?"**
A: Yes — it's a domain-specific language for defining database schemas, used by dbdiagram.io. It's not SQL, but it maps directly to SQL CREATE TABLE statements. Think of it as a shorthand for drawing ERDs.

**Q: "Do companies really have ERDs?"**
A: The best ones do. Companies that don't have them often have "tribal knowledge" databases where only one senior engineer understands how the tables relate — and when that person leaves, there's a crisis.
