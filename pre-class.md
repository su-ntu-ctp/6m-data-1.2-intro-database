# 📚 Pre-Class: Intro to Database

**Estimated Time:** 30–45 minutes
**Prerequisites:** Lesson 1.1 — The Data Landscape

> In Lesson 1.1 you learned what Data Analytics, Data Science, and AI are. This lesson goes one level deeper: before data can be analysed, it needs to be *stored* somewhere. This pre-class reading introduces how databases are structured and why the design decisions matter.

Complete the steps below **before your class session**. The in-class activities (building schemas for a Car Insurance company and an E-Commerce platform) build directly on these concepts.

---

## Step 1 — Watch the Video (10 min)

▶️ [A Brief History of Databases and Their Applications](https://www.youtube.com/watch?v=WDTbDiQqPdA)

A concise overview of why databases were invented, how they evolved from flat files to relational systems, and where modern databases (NoSQL, Vector) fit in. Watching this first will give you the historical context for everything in Steps 2 and 3.

---

## Step 2 — Read the Core Concepts (15 min)

### The Database Landscape

Not all data is stored the same way. There are three main categories:

- **Relational (SQL):** Think of a **Bank Vault** — strict, organised, and safe. Used for financial records, user accounts, and inventory. (Examples: PostgreSQL, MySQL)
- **NoSQL:** Think of a **Messy Desk** — flexible and fast. You can store documents (JSON) without worrying about rigid structure. Used for social media feeds or sensor data. (Examples: MongoDB)
- **Vector:** Think of a **Brain** — stores data by *meaning* rather than keywords. Used for AI and similarity searches (e.g., "Find me a song that sounds sad"). (Examples: Pinecone, Weaviate)

### The Anatomy of a Table

In this lesson, we focus on **Relational (SQL)** databases. Tables are linked together using special IDs called **Keys**:

- **Primary Key (PK):** The unique ID of a row. Think of it like your **Passport Number** — every person in the system has one, and no two are the same. It distinguishes "John Smith (ID: 1)" from "John Smith (ID: 2)".
- **Foreign Key (FK):** A reference to someone else's Primary Key. Think of it like your **Passport Number written on a Flight Manifest** — the manifest doesn't copy your whole passport, it just holds a reference that points back to you.

### Normalization — The Art of Tidying Up

Normalization means organising data to avoid redundancy. Imagine a spreadsheet where you write a customer's home address on every order row. If they move house, you'd have to update hundreds of rows.

- **Un-normalized:** Storing the address 500 times across 500 order rows.
- **Normalized:** Storing the address *once* in a `Customers` table, and referring to a `customer_id` in the `Orders` table.

> **See it in action:** The [Database Normalization Interactive Visualization](https://su-ntu-ctp.github.io/6m-data-1.2-intro-database/) walks you through this step-by-step in your browser. Highly recommended.

---

## Step 3 — Tool Setup (5 min)

We will use a browser-based tool to draw database diagrams in class. Set it up now so you're not doing it during the session.

1. Go to [**dbdiagram.io**](https://dbdiagram.io/)
2. Click **"Go to App"** — the free version is all you need
3. Try typing `Table users { id int }` on the left panel and watch the diagram appear on the right. That's DBML — the language you'll use in class.

---

## Step 4 — Think About This

Before the session, consider: **Why do we need a database at all? Why not just use a giant Excel spreadsheet?**

Write down your thoughts, then check the suggested answer below.

<details>
<summary>Suggested answer</summary>

Excel works well for small, human-curated datasets — but it breaks down quickly at scale and in multi-user environments. A few specific problems:

- **Concurrency:** If two people edit the same Excel file at the same time, one person's changes get overwritten. Databases handle thousands of simultaneous writes safely.
- **Scale:** Excel starts to struggle past ~1 million rows. Databases are designed to handle billions.
- **Integrity:** Excel lets you type anything into any cell — no validation. Databases enforce rules: a `customer_id` column can only contain integers, an `email` column must be unique, etc.
- **Relationships:** In Excel, linking two sheets together is manual and fragile. Databases have Foreign Keys that enforce relationships automatically.
- **Querying:** Finding all customers who spent more than $500 last month in Excel requires complex formulas. In a database, it's one SQL line.

</details>

---

## 🎯 Pre-Class Activity

*Complete this short thought experiment before the session. It's the conceptual foundation for the E-Commerce Deconstruction activity in class.*

**The Receipt Challenge**

Find a physical receipt (grocery store, café, or online order) or imagine one. Look at the data on it and decide: does each piece of data belong to the **Transaction** (happened once, specific to this purchase) or the **Master Record** (stays the same regardless of how many times someone buys)?

| Data Item | Transaction or Master Record? |
|---|---|
| Date & Time | |
| Store Address | |
| Item Name (e.g., "Milk") | |
| Item Price | *(Tricky — what happens if the price of milk changes next week?)* |

*If attending live: bring your answers to class — we'll use them to kick off the E-Commerce activity.*
*If self-studying: work through it yourself first, then check the answer key below.*

<details>
<summary>Answer Key — The Receipt Challenge</summary>

| Data Item | Category | Reasoning |
|---|---|---|
| Date & Time | **Transaction** | The date is specific to this purchase. Every order has a different date. |
| Store Address | **Master Record** | The store's address doesn't change from transaction to transaction. It belongs in a `Stores` table, referenced by the order. |
| Item Name ("Milk") | **Master Record** | "Milk" is a product that exists independently of any single purchase. It belongs in a `Products` table. |
| Item Price | **Both — depending on context** | The *current* price of milk belongs in the `Products` table. But the *price you actually paid* on this specific date must be snapshotted into the Transaction row — because if the price changes next week, your old receipt should still show what you paid, not the new price. This is exactly the "History Problem" you'll solve in the in-class assignment. |

**The key insight:** Some data lives on the receipt *because it was true at that moment*, even if the master record changes later. This is why transaction tables often store a `price_at_purchase` column alongside a `product_id`.

</details>
