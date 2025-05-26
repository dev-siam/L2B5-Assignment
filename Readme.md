# Some Important QNA about PostgreSQL
![ PostgreSQL](https://cdn.prod.website-files.com/618a899ac6938d341ce4004a/66daee6bfda6d0cf86708ce0_66cd9c4595881a51fb1104d6_6464b93f55b890753f513589_what%252520is%252520postgresql.webp)
# 1. What is PostgreSQL?

**PostgreSQL** হলো একটি শক্তিশালী, ওপেন-সোর্স (open-source), রিলেশনাল ডাটাবেস ম্যানেজমেন্ট সিস্টেম (RDBMS)। এটি সাধারণত টেবিল, সারি এবং কলামের মধ্যে একটি কাঠামোগত বিন্যাসে ডেটা সংরক্ষণ, পরিচালনা এবং পুনরুদ্ধারের জন্য ব্যবহৃত হয় । একে সংক্ষেপে Postgres নামে  ডাকা হয়।

এটি SQL-এর স্ট্যান্ডার্ড অনুসরণ করে, আবার কিছু নিজস্ব অতিরিক্ত ফিচারও আছে। SQL হলো (Structured Query Language) 

 SQL হলো ভাষা — PostgreSQL হলো সেই ভাষা বোঝে এমন একটি সফটওয়্যার (ডেটাবেস সিস্টেম)।

---

![](https://cdn.prod.website-files.com/6130fa1501794e37c21867cf/6359ee850b1a324c795bdad2_Understanding%20Database%20Schemas.png)
# 2. What is the purpose of a database schema in PostgreSQL?

PostgreSQL-এ Database Schema এর উদ্দেশ্য কী?
**Schema** মানে কী?
Schema হলো ডেটাবেসের একটা আলাদা অংশ বা বিভাগ, যেখানে টেবিল, ভিউ, ফাংশন, ইত্যাদি রাখা হয়।

---

তুমি যেভাবে তোমার কম্পিউটারে ফোল্ডার তৈরি করো ফাইলগুলো গুছিয়ে রাখার জন্য ― ঠিক তেমনভাবেই একটা ডেটাবেসে স্কিমা ব্যবহার করা হয় বিভিন্ন টেবিল বা অবজেক্ট গুছিয়ে রাখার জন্য।

Schema এর মূল কাজ কী?
- Organization: অনেক টেবিল থাকলে সব এক জায়গায় রাখলে বিশৃঙ্খলা তৈরি হয়। Schema ব্যবহার করে টেবিলগুলো ভাগ করে রাখা যায়।
- User Permissions : একেক স্কিমাতে একেকজন ইউজারকে অ্যাক্সেস দেওয়া যায়।

---
 উদাহরণ:
ধরো তোমার একটা ডেটাবেস আছে যার নাম school_db. এখন তুমি সেখানে দুইটা স্কিমা তৈরি করলে:

- students_schema → শুধুমাত্র ছাত্রদের ডেটা রাখবে
- teachers_schema → শুধুমাত্র শিক্ষকদের ডেটা রাখবে

- ### স্কিমা তৈরি করা
```sql
CREATE SCHEMA students_schema;
CREATE SCHEMA teachers_schema;
```

```sql
CREATE TABLE students_schema.profile (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100)
);

CREATE TABLE teachers_schema.profile (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  subject VARCHAR(100)
);
```
এখানে দুইটা profile টেবিল আছে, কিন্তু তারা দুইটা আলাদা স্কিমার মধ্যে আছে, তাই কনফ্লিক্ট হয়নি।

সংক্ষেপে: Schema মানে কী? ডেটাবেসের ভিতরের আলাদা আলাদা বিভাগ, ডেটা গুছিয়ে রাখা, নিরাপত্তা নিশ্চিত করা, এবং একই নামের টেবিল রাখা।
উদাহরণ	students_schema.profile এবং teachers_schema.profile – দুইটা আলাদা স্কিমায় একই টেবিল নাম

---

# 3. What is the Primary Key and Foreign Key in PostgreSQL?
![](https://d8it4huxumps7.cloudfront.net/uploads/images/65044910e1123_primary_key_versus_foreign_key_02.jpg?d=2000x2000)
---

##  Primary Key কী?

**Primary Key** হলো এমন একটি কলাম বা একাধিক কলাম, যা একটি টেবিলের প্রতিটি রো (সারি) কে **অন্যদের থেকে আলাদা করে চিহ্নিত করে**।

####  বৈশিষ্ট্য:
- প্রতিটি ভ্যালু **অবশ্যই ইউনিক (অনন্য)** হতে হবে
- **NULL** (খালি) থাকতে পারবে না

```sql
CREATE TABLE students (
  student_id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  age INT
);
```
এখানে student_id হলো Primary Key কারণ:
প্রতিটি ছাত্রের student_id আলাদা, কোনো student_id খালি বা NULL নয়
তাই Primary Key দিয়ে যেকোনো রেকর্ড খুঁজে বের করা সহজ।

## Foreign Key কী?
**Foreign Key** হলো এমন একটি কলাম, যেটা অন্য কোনো টেবিলের **Primary Key-এর সাথে সম্পর্ক তৈরি করে।**

এটা ব্যবহার করে আমরা টেবিলগুলোর মধ্যে সম্পর্ক (relation) তৈরি করতে পারি।

#### বৈশিষ্ট্য:
এটা অন্য টেবিলের Primary Key-এর মানকে রেফার করে Referential Integrity (সংযোগের শুদ্ধতা) নিশ্চিত করে

```sql
CREATE TABLE sightings (
  sighting_id SERIAL PRIMARY KEY,
  student_id INT REFERENCES students(student_id),
  species_name VARCHAR(100),
  date DATE
);
```
এখানে student_id হলো Foreign Key, কারণ:
এটা students টেবিলের student_id কে রেফার করে এমন কোনো student_id এখানে দেওয়া যাবে না, যেটা students টেবিলে নেই
তাই Foreign Key দিয়ে দুই টেবিলের মধ্যে সংযোগ তৈরি হয়।


---


# 4. VARCHAR ও CHAR প্রধান কি?
![](https://images.shiksha.com/mediadata/ugcDocuments/images/wordpressImages/2023_01_MicrosoftTeams-image-247.jpg)

* `CHAR(n)` হলে নির্ধিস্টিত n লেংথের fixed-length string স্টোর করে।
* `VARCHAR(n)` হলে একটি variable-length string যা সর্বোচ্চ n পর্যন্ত চলে।

উদাহরণ:

```sql
name CHAR(10)      -- ঠিক 10 characters নিবে, না হলে padding দিবে।
name VARCHAR(10)   -- যতটুকু লাগে ততটুকু নিবে (10-এর কমও হতে পারে)
```

---

# 5. The `WHERE` Clause in a SELECT Statement in PostgreSQL 
![](https://cdn.educba.com/academy/wp-content/uploads/2020/01/PostgreSQL-WHERE-Clause.jpg)

##  What is the `WHERE` Clause?

`WHERE` হলো একটি শর্ত (condition) যা **`SELECT`, `UPDATE`, বা `DELETE`** SQL স্টেটমেন্টে ব্যবহার করা হয় — **যাতে ডেটাবেজ থেকে নির্দিষ্ট রো (row)** নির্বাচন করা যায় **যা শর্ত পূরণ করে**।


`WHERE` Clause ব্যবহার করে আমরা যেকোনো টেবিল থেকে এমন সব ডেটা নির্বাচন করতে পারি যা আমাদের নির্দিষ্ট প্রয়োজন মেটায়। এটি SQL কোয়েরিকে আরও **সুনির্দিষ্ট** করে তোলে।

---

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition;
```
উদাহরণ: ধরি আমাদের কাছে একটি students টেবিল আছে:
```sql
CREATE TABLE students (
  student_id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  age INT
);
```
এখন আমরা যদি ১৮ বছরের বেশি বয়সী ছাত্রদের তালিকা চাই:

```sql
SELECT * FROM students
WHERE age > 18;
```
এই কোয়েরি শুধুমাত্র সেই রেকর্ডগুলো রিটার্ন করবে যাদের age > 18।

আরও কিছু উদাহরণ:
- 1.নির্দিষ্ট নাম খুঁজে বের করা:

```sql
SELECT * FROM students
WHERE name = 'Siam';
```
- 2.একাধিক শর্ত:
```sql
SELECT * FROM students
WHERE age > 18 AND name = 'Siam';
```

- 3.LIKE দিয়ে আংশিক মিল:

```sql
SELECT * FROM students
WHERE name LIKE 'S%'; 
```

---

# 6. Understanding `LIMIT` and `OFFSET` Clauses in PostgreSQL
![](https://images.shiksha.com/mediadata/ugcDocuments/images/wordpressImages/2022_07_MicrosoftTeams-image-223.jpg)

##  কী হলো `LIMIT` এবং `OFFSET`?

PostgreSQL-এ **`LIMIT`** এবং **`OFFSET`** ক্লজ ব্যবহার করা হয় **ডেটা নির্বাচন (SELECT) করার সময় রেজাল্টের পরিমাণ এবং শুরু পজিশন নিয়ন্ত্রণ করতে**।

---

##  `LIMIT` এর কাজ কী?

`LIMIT` ক্লজ দিয়ে নির্ধারণ করা হয় **কতটি রেকর্ড (row)** রিটার্ন করা হবে।

```sql
SELECT * FROM students
LIMIT 5;
```
উপরের কোয়েরি শুধুমাত্র প্রথম ৫টি রেকর্ড দেখাবে, যত বড়ই টেবিল হোক না কেন।

## OFFSET এর কাজ কী?
 OFFSET ক্লজ দিয়ে বলে দেওয়া হয় কতটি রেকর্ড স্কিপ (বা বাদ) করতে হবে শুরুতে।


```sql
SELECT * FROM students
OFFSET 3;
```
এই কোয়েরি প্রথম ৩টি রেকর্ড স্কিপ করে বাকি রেকর্ডগুলো দেখাবে।

## LIMIT ও OFFSET একসাথে ব্যবহার
তুমি যখন ডেটার নির্দিষ্ট একটি অংশ (একধরনের পেজিনেশন) দেখতে চাও তখন দুটো একসাথে ব্যবহার করো।


```sql
SELECT * FROM students
LIMIT 5 OFFSET 10;
```
উপরের কোয়েরি প্রথম ১০টি রেকর্ড স্কিপ করে, পরবর্তী ৫টি রেকর্ড দেখাবে।

---

# 7. How to Modify Data Using `UPDATE` Statements in PostgreSQL
![](https://i.ytimg.com/vi/YcAMivVvd_g/hq720.jpg?sqp=-oaymwEhCK4FEIIDSFryq4qpAxMIARUAAAAAGAElAADIQj0AgKJD&rs=AOn4CLByplZXPZycmbYdEpXYrSBUEjS6Jw)

##  `UPDATE` কি?

`UPDATE` হল একটি SQL কমান্ড যা ব্যবহার করে **টেবিলের বিদ্যমান ডেটা পরিবর্তন বা আপডেট** করা হয়।

যখন কোনো রেকর্ডের তথ্য পরিবর্তন করতে হয় — যেমন কারো নাম, বয়স, ঠিকানা ইত্যাদি — তখন `UPDATE` ব্যবহার করা হয়।

---


```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```
 - উদাহরণ ১: একটি নির্দিষ্ট রেকর্ড আপডেট করা
```sql
UPDATE students
SET name = 'Rahim'
WHERE student_id = 1;
```
উপরের কোয়েরিতে students টেবিলের যেখানে student_id = 1, সেখানে name পরিবর্তন করে 'Rahim' করা হয়েছে।

- উদাহরণ ২: একাধিক কলাম একসাথে আপডেট করা
```sql
UPDATE employees
SET salary = 50000, position = 'Senior Developer'
WHERE employee_id = 7;
```
employee_id ৭ যাদের, তাদের salary ও position আপডেট করা হলো।

- সাবধানতা:
WHERE ক্লজ ছাড়া UPDATE দিলে সব রেকর্ড আপডেট হয়ে যাবে।

- Example (ভুলভাবে ব্যবহৃত):
```sql
UPDATE students
SET name = 'Unknown';
```
এটি সব ছাত্রের নাম ‘Unknown’ করে দেবে! তাই WHERE সবসময় ব্যবহার করো, যদি না সব রেকর্ড ইচ্ছাকৃতভাবে আপডেট করতে চাও।

---

# 8. What is the Significance of the `JOIN Operation`, and How Does It Work in PostgreSQL?
![](https://arc2r.github.io/book/images/joins.png)
##  JOIN কি?

`JOIN` হল একটি SQL অপারেশন যার মাধ্যমে **একাধিক টেবিলের তথ্য একসাথে মিলিয়ে** দেখানো যায়।  
যখন তোমার ডেটাবেজে তথ্য আলাদা টেবিলে থাকে, তখন এই টেবিলগুলো **আপসের সম্পর্কযুক্ত কলামের মাধ্যমে** একসাথে এনে কাজ করাতে JOIN ব্যবহার করা হয়।

---

## কেন JOIN ব্যবহার করব?

ধরো, তোমার দুটি টেবিল আছে:

- `students` টেবিলে ছাত্রদের তথ্য আছে
- `marks` টেবিলে ছাত্রদের নাম্বার আছে

তুমি যদি প্রতিটি ছাত্রের নামের পাশে তার নাম্বার দেখতে চাও, তখন `JOIN` করতে হবে।

---

##  JOIN এর প্রকারভেদ (Types of JOIN):

| JOIN এর নাম | কাজ |
|-------------|------|
| `INNER JOIN` | শুধুমাত্র মিল পাওয়া রেকর্ডগুলোকেই দেখায় |
| `LEFT JOIN`  | বাম (প্রথম) টেবিলের সব রেকর্ড দেখায়, আর ডান টেবিল থেকে যেটা মিলে |
| `RIGHT JOIN` | ডান (দ্বিতীয়) টেবিলের সব রেকর্ড দেখায় |
| `FULL JOIN`  | দুই টেবিলের সব রেকর্ড দেখায়, যেগুলা মিলে বা মিলে না |

---

###  উদাহরণ: ধরি, দুইটা টেবিল আছে:

### `students` টেবিল:

| id | name     |
|----|----------|
| 1  | Alice    |
| 2  | Bob      |
| 3  | Carol    |

### `marks` টেবিল:

| id | student_id | score |
|----|------------|-------|
| 1  | 1          | 90    |
| 2  | 2          | 85    |

---

### 1. INNER JOIN

```sql
SELECT students.name, marks.score
FROM students
INNER JOIN marks
ON students.id = marks.student_id;
```

### Result:

|name|	score       |
|----|------------|
|Alice |	90  |
|Bob |	85  |

শুধু যাদের students.id এবং marks.student_id মিলে গেছে, তাদেরই দেখায়।

### 2. LEFT JOIN
```sql
SELECT students.name, marks.score
FROM students
LEFT JOIN marks
ON students.id = marks.student_id;
```
### Result:
|name|	score       |
|----|------------|
|Alice |	90  |
|Bob    |	85   |
|Carol |	NULL  |


বাম টেবিলের সব তথ্য থাকবে, আর মিল না থাকলে NULL।

### 3. RIGHT JOIN
```sql
SELECT students.name, marks.score
FROM students
RIGHT JOIN marks
ON students.id = marks.student_id;
```
ফলাফল একই, যদি ডান টেবিলে সব student_id students টেবিলে থাকে।

- JOIN ব্যবহার করার সময় যেসব কলাম দুই টেবিলকে যুক্ত করে — সেগুলোকে আমরা কী (key) বলি। 
- অনেক সময় Foreign Key-এর সাথেই JOIN করা হয়।
ব্যবহারিক উদাহরণ: ধরি, তুমি চাও প্রতিটি ranger এর নাম এবং সে কয়টা sightings করেছে:

```sql
SELECT r.name, COUNT(s.sighting_id) AS total_sightings
FROM rangers r
LEFT JOIN sightings s ON r.ranger_id = s.ranger_id
GROUP BY r.name;
```
এখানে rangers এবং sightings টেবিলকে যুক্ত করা হয়েছে তাদের ranger_id দিয়ে।

---
