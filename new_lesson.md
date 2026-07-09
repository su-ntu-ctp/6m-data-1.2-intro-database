# 📚 Lesson 1.2: Data Modeling & Schema Design — Facilitator Guide

> **This is a facilitator guide, not a lecture script.**
> It assumes learners have already completed `pre-class.md` (watched the video, read the concepts, set up dbdiagram.io). Your job is **not** to re-teach what they read — it is to **activate** it through activities, give **real-time feedback**, and **bridge** the gaps the pre-class left open.
>
> **Self-learner note:** If you are studying alone with no facilitator, every section below marked `🧍 SOLO` tells you exactly how to substitute the missing peer/teacher feedback. You can run this entire lesson by yourself.

---

## 🎯 The One Idea This Lesson Must Land

> *"Before you can analyse data, someone has to decide where each fact lives. Good design = each fact in exactly one place — except when business needs it somewhere else."*

Everything below serves that one sentence. If you only have 10 minutes, spend it on the Receipt → E-Commerce → FoodFast thread (Part 3 + assignment), not on re-explaining database types.

---

## ✅ Before Class — Facilitator Checklist

- [ ] Learners completed `pre-class.md` (video + concepts + dbdiagram.io account)
- [ ] You have opened [dbdiagram.io](https://dbdiagram.io/d) and pasted the Car Insurance schema once, so you can demo live
- [ ] You have the local copy of the normalization viz ready: `visualisation/normalization-interactive.html` (don't rely on the online link alone — offline learners will be stuck)
- [ ] You have read the **Facilitator Cheat-Sheet** at the bottom of this file (common mistakes to watch for)
- [ ] 🧍 **SOLO:** You have the `assignment.md` (FoodFast) open in a second tab — that's your "teacher" for the day

---

## 🗺️ Session Overview (3 hours, redistributed)

| Time | Part | What happens (facilitator role) | Lecture time |
|------|------|--------------------------------|--------------|
| 0:00 – 0:05 | Part 1 | **Anchor** — 5-min wake-up, no re-teaching | 5 min |
| 0:05 – 0:20 | Part 1 | **Activity 1:** Sorting Game (apply types) | 0 |
| 0:20 – 0:30 | Part 1 | **Bridge:** Why we focus on SQL, not NoSQL/Vector today | 5 min |
| 0:30 – 0:32 | Break | — | — |
| 0:32 – 0:34 | Part 2 | **Wake-up** — 2-min PK/FK recall | 2 min |
| 0:34 – 0:49 | Part 2 | **Activity 2.1:** Code-Along Car Insurance | 0 |
| 0:49 – 1:04 | Part 2 | **Activity 2.2:** School System workshop | 0 |
| 1:04 – 1:14 | Break | — | — |
| 1:14 – 1:16 | Part 3 | **Bridge:** Receipt Challenge → E-Commerce (2 min) | 2 min |
| 1:16 – 1:24 | Part 3 | **Interactive viz** (8-step normalization tour) | 0 |
| 1:24 – 1:44 | Part 3 | **Activity 3:** E-Commerce Deconstruction | 0 |
| 1:44 – 1:59 | Part 3 | **Activity 3.2:** Movie Rental practice | 0 |
| 1:59 – 2:10 | Break | — | — |
| 2:10 – 2:50 | Assignment | **FoodFast** challenge (many-to-many + history) | 0 |
| 2:50 – 3:00 | Wrap-Up | Takeaways, bridge to Lesson 1.3, hand off | 5 min |

**Total passive lecture: ~19 min** (down from ~60+ in the original). The rest is doing.

---

## 🏃 Part 1: The Data Landscape (30 min)

### 🔑 0:00 – 0:05 — Anchor (DO NOT re-teach)

Say this, then stop talking:

> *"At home you met three database 'vibes': SQL = bank vault, NoSQL = messy desk, Vector = brain. Quick show of hands — which one stores your bank balance? Which one finds 'songs like Jazz'? Good. That's all I'll say about types. In 15 minutes you'll prove you actually get it."*

**Facilitator note:** If learners look blank, that's a signal they skipped pre-class. Don't lecture — send them to `pre-class.md` Step 2 for 5 min, then rejoin. Never re-read the Three Pillars table aloud.

### 🛠️ 0:05 – 0:20 — Activity 1: The Sorting Game (15 min)

**Run it as a rapid-fire quiz, not a reading exercise.** Put each scenario on screen, call on someone, demand the *why* not just the label.

1. **User Profile Pictures** → NoSQL/Object store for the binary; SQL for the file path
2. **Payment Processing** → SQL (needs ACID / transactions)
3. **"Find me songs that sound like Jazz"** → Vector
4. **Live chat for a video game** → NoSQL (high volume, simple structure)

<details>
<summary>Facilitator answers + the "why" to push for</summary>

1. **User Profile Pictures:** The image bytes go to an object store (S3-like) or NoSQL; the *metadata* (path, owner) lives in SQL. Push learners to separate **binary** from **reference**.
2. **Payment Processing:** ACID is the keyword — money can't be lost or doubled. SQL guarantees this.
3. **"Songs like Jazz":** Similarity by *meaning*, not exact match → Vector.
4. **Live game chat:** Massive write volume, loose structure → NoSQL wins on speed/flexibility.

**Watch for:** Learners who just "label" without the trade-off. Always ask *"Why not the other two?"* That's the real test.
</details>

🧍 **SOLO:** Write your answer for each of the 4 before opening the spoiler. Then force yourself to add one sentence: *"The reason it's NOT the other two is ___."* If you can't, re-read `pre-class.md` Step 2.

### 🌉 0:20 – 0:30 — Bridge: "So why are we only building SQL today?" (5 min)

**This is the gap the original lesson never closed.** Spend real time here.

> *"You just proved you can pick a database type. But for the rest of today — and the next lesson where we write SQL — we only build **Relational** schemas. Why?*
> *Because 90% of the data you'll analyse as a data scientist lives in relational systems. And the skill of 'where does each fact belong' transfers directly to NoSQL and Vector design later. We're not ignoring them — we're mastering the foundation first."*

Then make it concrete with FoodFast (the assignment):

> *"FoodFast has users, orders, restaurants, couriers. Could you build it in NoSQL? Sure. But the moment you need 'total revenue by product last year', SQL's strict structure saves you. We'll see exactly why in the assignment's denormalization trade-off."*

**Facilitator note:** This single bridge fixes the "why did we spend 50 min on NoSQL if we never use it" confusion. Don't skip it.

🧍 **SOLO:** In your notes, write one line: *"FoodFast uses SQL (not NoSQL) because ___."* You'll confirm it's right when you hit the assignment's Scenario 2 (join performance).

---

## 🏃 Part 2: Building the Blueprint — ERD (32 min)

### 🔑 0:32 – 0:34 — Wake-up (2 min)

> *"PK = passport number (unique ID of a row). FK = that passport number written on a flight manifest (a reference to someone else's PK). Open dbdiagram.io. Type `Table users { id int [pk, increment] }`. See the diagram appear? That's DBML. Go."*

**Do not** re-explain `int`, `varchar`, or `[pk, increment]` — the pre-class covered it. If someone asks, point them to the SQL Data Types table in `lesson.md` / `reference.md`.

### 🛠️ 0:34 – 0:49 — Activity 2.1: Code-Along — Car Insurance (15 min)

Paste this into dbdiagram.io and have learners type along. Read the inline comments as you go — they ARE the teaching.

```dbml
// --- 1. Define the Customer ---
Table customers {
  id int [pk, increment]
  name varchar
  address varchar
  phone varchar
  email varchar
}

// --- 2. Define the Car ---
Table cars {
  id int [pk, increment]
  make varchar
  model varchar
  year int
  car_plate varchar
  customer_id int // FK → customers
}

// --- 3. The Relationship ---
// '>' = "One-to-Many". Read: "One Customer has Many Cars"
Ref: cars.customer_id > customers.id

// --- 🟢 CHALLENGE ---
// Add an 'accidents' table: date, location, description, linked to a CAR.
```

<details>
<summary>Solution</summary>

```dbml
Table accidents {
  id int [pk, increment]
  date datetime
  location varchar
  description text
  car_id int // FK → cars
}
Ref: accidents.car_id > cars.id
```
</details>

**Facilitator feedback points (walk the room, look for these):**
- `Ref` arrow pointing the wrong way (e.g., `customers.id > cars.customer_id`) — the **many** side holds the FK, not the one side.
- Forgetting `[pk, increment]` on a new table.
- Putting accident data *inside* `cars` instead of a separate table (missed the "each fact its own place" instinct).

🧍 **SOLO:** After pasting, deliberately break one thing (e.g., flip the `Ref` arrow) and watch what the diagram does. Then fix it. Self-correction by breaking is the fastest way to learn the rule.

### 🛠️ 0:49 – 1:04 — Activity 2.2: School System Workshop (15 min)

> Each student belongs to one class. Each teacher may teach many classes; each class may have many teachers.
> Entities: Student(id, name, address, phone, email, class_id) · Teacher(id, name, address, phone, email) · Class(id, name)

<details>
<summary>Sample solution</summary>

```dbml
Table students {
  id int [pk, increment]
  name varchar
  address varchar
  phone varchar
  email varchar
  class_id int
}
Table teachers {
  id int [pk, increment]
  name varchar
  address varchar
  phone varchar
  email varchar
}
Table classes {
  id int [pk, increment]
  name varchar
}
// Junction: class ↔ teacher is many-to-many
Table class_teachers {
  class_id int
  teacher_id int
}
Ref: students.class_id > classes.id
Ref: class_teachers.class_id > classes.id
Ref: class_teachers.teacher_id > teachers.id
```
</details>

**Facilitator note:** The key teaching moment is the **junction table** `class_teachers`. Many learners will try to put `teacher_id` on `classes` (works only if one teacher per class). Call this out: *"The spec says a class can have MANY teachers — so a single FK column can't hold that. You need a bridge table."* This is the seed for FoodFast's `order_items` junction.

🧍 **SOLO:** Submit your DBML to yourself — i.e., re-read it cold 10 minutes later and check: (a) every table has a PK, (b) every `Ref` arrow starts at the "many" side. If both hold, you're correct.

---

## 🏃 Part 3: Normalization (45 min)

### 🌉 1:14 – 1:16 — Bridge: Receipt → E-Commerce (2 min)

> *"In pre-class you did the Receipt Challenge: is the price on a receipt a Transaction fact or a Master Record fact? You found it's BOTH — the price *paid* is a transaction, the price *today* is master data. That tension is exactly what normalization manages. Let's see it on a real messy spreadsheet."*

### 🖥️ 1:16 – 1:24 — Interactive Visualization (8 min)

Point learners to **`visualisation/normalization-interactive.html`** (local file — double-click it). Walk the 8 steps. Tell them: *"Don't read the lesson text yet — let the animation show you what 1NF/2NF/3NF actually DO, then the written rules will make sense."*

🧍 **SOLO:** Open the local HTML file. Use arrow keys. After step 8, write in your own words: *"1NF = ___, 2NF = ___, 3NF = ___."* Then check against the rules below.

### 🛠️ 1:24 – 1:44 — Activity 3: The E-Commerce Deconstruction (20 min)

**Starting messy spreadsheet:**

| OrderID | ItemID | ItemName | ItemPrice | CustomerID | CustomerName | OrderDate |
|---------|--------|----------|-----------|-----------|-------------|-----------|
| 100 | 10 | iPhone | 1000 | 1 | John | 2021-01-01 |
| 100 | 20 | iPad | 500 | 1 | John | 2021-01-01 |
| 200 | 30 | Macbook | 2000 | 1 | John | 2021-01-02 |
| 300 | 10 | iPhone | 1000 | 2 | Mary | 2021-01-03 |
| 300 | 30 | Macbook | 2000 | 2 | Mary | 2021-01-03 |

**Guide learners through the 3 steps (don't give the answer — coach):**

- **Step 1 (1NF):** "R1 has two items — is `OrderID` alone a unique key? No. What do we add?" → `LineNumber` → composite key `(OrderID, LineNumber)`.
- **Step 2 (2NF):** "CustomerName depends on CustomerID, not on LineNumber. Where does it go?" → split `Customers` + `Orders`.
- **Step 3 (3NF):** "ItemName/ItemPrice depend on ItemID, not the order. And CustomerName depends on CustomerID. Split again." → `Products` + `Customers`.

<details>
<summary>Final 4-table DBML</summary>

```dbml
Table customers {
  id int [pk, increment]
  name varchar
}
Table products {
  id int [pk, increment]
  name varchar
  price decimal
}
Table orders {
  id int [pk, increment]
  customer_id int
  order_date date
}
Table order_line_items {
  order_id int
  line_number int
  product_id int
}
Ref: orders.customer_id > customers.id
Ref: order_line_items.order_id > orders.id
Ref: order_line_items.product_id > products.id
```
</details>

**Facilitator feedback point:** The biggest mistake is skipping 1NF and jumping to 4 tables. Make them show you the `(OrderID, LineNumber)` composite key first.

### 🛠️ 1:44 – 1:59 — Activity 3.2: Movie Rental (15 min)

Same 3-step method on: `RentalID, CustomerName, CustomerPhone, MovieTitle, MovieGenre, RentalDate, ReturnDate`.

<details>
<summary>Sample solution</summary>

```dbml
Table customers { id int [pk, increment]  name varchar  phone varchar }
Table movies { id int [pk, increment]  title varchar  genre varchar }
Table rentals { id int [pk, increment]  customer_id int  rental_date date  return_date date }
Table rental_line_items { rental_id int  line_number int  movie_id int }
Ref: rentals.customer_id > customers.id
Ref: rental_line_items.rental_id > rentals.id
Ref: rental_line_items.movie_id > movies.id
```
</details>

🧍 **SOLO:** Time yourself. Can you normalize Movie Rental to 3NF in under 10 minutes without peeking? That's your mastery check for Part 3.

---

## 🚀 Assignment: FoodFast (40 min) — the real test

Hand off to `assignment.md`. This is where Part 2 (junction table) and Part 3 (history/denormalization) fuse into one real case.

**Facilitator role during assignment:**
- Circulate. The two hard parts are **Challenge 1 (many-to-many → `order_items` junction)** and **Challenge 2 (history → `price_at_purchase`)**.
- For Challenge 1, remind them of `class_teachers` from Part 2.
- For Challenge 2, remind them of the Receipt Challenge from pre-class ("price paid vs price today").
- Challenge 3 (export SQL) is a **look-ahead** to Lesson 1.3 — tell them it's fine if the SQL looks alien. They'll learn it next.

🧍 **SOLO:** The assignment IS your teacher. After writing your DBML, open the Solution Key and diff it line-by-line against yours. Every difference is a lesson. Then read the **Post-Class Reading: Normalisation Trade-offs** — Scenario 1 & 2 close the loop on why `price_at_purchase` matters and why real DBs sometimes denormalize.

---

## 🎁 Wrap-Up (10 min)

### Key Takeaways (say these out loud)
1. **Each fact lives in exactly one place** — that's normalization in one sentence.
2. **Junction tables resolve many-to-many** — one FK column can't hold "many."
3. **Transactions snapshot history** (`price_at_purchase`) — live references lie about the past.
4. **Good design balances integrity with business need** — sometimes you deliberately denormalize.

### 🌉 Bridge to Lesson 1.3 (manage expectations!)
> *"Today you designed tables. You still can't ASK them questions — that's SQL, next lesson. Design-first is deliberate: a well-modeled database is what makes SQL fast and trustworthy later. The FoodFast SQL you exported today? That's your head-start for 1.3."*

**Facilitator note:** Self-learners especially need this. They've built schemas but can't yet query — name the "delayed payoff" so they don't lose momentum.

---

## 📋 Facilitator Cheat-Sheet — Common Mistakes to Watch For

| Symptom | Root cause | What to say |
|---------|-----------|-------------|
| `Ref` arrow points from the "one" side | Confused which table holds the FK | "The MANY side holds the reference. Cars hold customer_id, not the other way." |
| Puts accident/movie data inside parent table | Missed "each fact its own place" | "If a car can have many accidents, accidents need their own table." |
| Tries `teacher_id` on `classes` for school system | Forgot many-to-many needs a bridge | "A class has many teachers AND a teacher has many classes → junction table." |
| Skips 1NF, jumps to 4 tables in E-Commerce | Wants the answer, skips the method | "Show me the composite key first: (OrderID, LineNumber)." |
| In FoodFast, puts `item_id` directly in `orders` | Doesn't see many-to-many | "One order has many items, one item is in many orders → `order_items` junction." |
| In FoodFast, links price to `menu_items` only | Forgot the history problem | "What if the price changes Tuesday? Your Monday receipt lies. Snapshot `price_at_purchase`." |

---

## 🧍 Self-Learner Adaptation — Full Solo Path

If no facilitator/peers exist, this lesson is still fully runnable:

1. **Replace "call on someone"** → write your answer before revealing any spoiler.
2. **Replace "teacher feedback"** → after each activity, diff your DBML against the provided solution and the Cheat-Sheet above.
3. **Replace "group breakout"** → do E-Commerce and Movie Rental solo, then time yourself (mastery = <10 min each).
4. **Replace "Discord peer review"** → re-read your own schema cold after 10 min; if every table has a PK and every `Ref` starts at the many-side, you're correct.
5. **The assignment's Solution Key + Post-Class Trade-offs** are your teacher for the hard parts.
6. **Local viz file** `visualisation/normalization-interactive.html` is your classroom — don't depend on the online link.

**You are not missing anything essential by studying alone** — the only thing a facilitator adds is faster error-correction, which the Cheat-Sheet + Solution Keys replicate.

---

## 📂 Material Map

| Need | File |
|------|------|
| Concepts before class | `pre-class.md` |
| This facilitator guide | `new_lesson.md` |
| The real-world challenge | `assignment.md` |
| DBML syntax / ERD / normalization cheat-sheet | `reference.md` |
| Interactive normalization tour (local) | `visualisation/normalization-interactive.html` |
| Next lesson (writing the SQL) | Lesson 1.3 (DDL) — not in this folder |
