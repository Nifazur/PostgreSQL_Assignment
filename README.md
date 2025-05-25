
# Bonus Questions



## 1. Explain the purpose of the ```WHERE``` clause in a ```SELECT``` statement.

**Explanation:** ```WHERE``` keyword টি নির্দিষ্ট শর্তের (condition) উপর ভিত্তি করে টেবিলের rows গুলোকে ফিল্টার করে এবং যেসব row ঐ condition পূরণ করে, শুধুমাত্র সেগুলোই দেখায়।

### ✅ Example: Using ```WHERE``` Clause with ```SELECT```

```sql
SELECT * FROM species
    WHERE conservation_status = 'Endangered';
```


---


**Explanation:** এই কোয়ারিতে conservation_status = 'Endangered' শর্তটি ব্যবহার করা হয়েছে ডেটা ফিল্টার করার জন্য। এটি species টেবিল থেকে সেই সকল রেকর্ড (বা সারি) select করবে যাদের conservation_status মান ‘Endangered’। সহজভাবে বললে, এই কোডটি এমন সব প্রাণীর তথ্য দেখাবে যাদের সংরক্ষণ অবস্থা ‘বিপন্ন’ হিসেবে চিহ্নিত।

---
এই কোয়ারির মাধ্যমে বোঝানো হয়েছে: species টেবিল থেকে তাদেরকেই দেখাও(select করো), যাদের conservation_status = ‘Endangered’।



---

## 2. What are the ```LIMIT``` and ```OFFSET``` clauses used for?

**Explanation:** ```LIMIT``` এবং ```OFFSET``` ক্লজ ব্যবহার করে SQL-এ নির্দিষ্ট সংখ্যক row ফলাফল হিসেবে পাওয়া যায় এবং কোন row থেকে শুরু করে কতগুলো row দেখতে চাই তা নির্ধারণ করা যায়।

---

```LIMIT``` নির্ধারণ করে সর্বোচ্চ কতটি row প্রদর্শন করতে হবে, আর ```OFFSET``` নির্দেশ করে শুরুতে কতটি row বাদ দিতে হবে।

---
এই দুটি ক্লজ একসাথে ব্যবহার করে website এ Pagination (প্রতি page এ নির্দিষ্ট সংখ্যক ডেটা লোড) সহজেই করা যায়।

### ✅ Example: Using ```LIMIT``` Clause with ```OFFSET```

```sql
-- ******** Pagination using SQL (LIMIT + OFFSET) ********

-- Page No | Skip (OFFSET) | Limit
--    1    |       0       |   5
--    2    |       5       |   5
--    3    |      10       |   5
--    4    |      15       |   5
--   ...   |      ...      |  ...

-- OFFSET = (Page Number - 1) × Page Size

-- Page 1
SELECT * FROM students
LIMIT 5 OFFSET 0;

-- Page 2
SELECT * FROM students
LIMIT 5 OFFSET 5;

-- Page 3
SELECT * FROM students
LIMIT 5 OFFSET 10;
```


---


**Explanation:** এখানে ```LIMIT``` এবং ```OFFSET``` ব্যবহার করে pagination-এর একটি উদাহরণ দেখানো হয়েছে।

---
প্রথম page এ নিচের কোয়েরিটি ব্যবহার করে:
```sql
SELECT * FROM students
LIMIT 5 OFFSET 0;
```
প্রথম page এ প্রথম ৫টি ডেটা (row) দেখানো হয়েছে।

---

এরপর দ্বিতীয় পেজে:

```sql
SELECT * FROM students
LIMIT 5 OFFSET 5;
```
এখানে OFFSET 5 ব্যবহার করে প্রথম ৫টি row স্কিপ করা হয়েছে এবং পরের ৫টি row দেখানো হয়েছে।

---

এইভাবে প্রতি পেজে ৫টি করে row দেখানো হচ্ছে। অর্থাৎ: 	
```
Page 1: 0 টা স্কিপ → ১ম–৫ম row 	
Page 2: ৫টা স্কিপ → ৬ষ্ঠ–১০ম row 
Page 3: ১০টা স্কিপ → ১১তম–১৫তম row 
```
এই Pagination পদ্ধতিতে অনেক বড় ডেটাসেটকে সহজে ধাপে ধাপে ব্যবহারকারীর সামনে দেখানো যায়।

---

## 3. How can you modify data using ```UPDATE``` statements?

**Explanation:** কোনো টেবিলের ডেটা পরিবর্তন করতে ```UPDATE``` স্টেটমেন্ট ব্যবহার করা হয়। এর জন্য প্রথমে ```UPDATE``` লিখে টেবিলের নাম দিতে হয়, তারপর ```SET``` ক্লজ ব্যবহার করে কোন কলামের মান পরিবর্তন করতে চাই তা উল্লেখ করতে হয়।

---
যদি নির্দিষ্ট শর্ত অনুযায়ী ডেটা আপডেট করতে চাই, তাহলে ```WHERE``` ক্লজ ব্যবহার করে শর্তটি দিতে হয়।


### ✅ Example: Using ```UPDATE``` Clause

```sql
UPDATE species
    SET conservation_status = 'Historic'
    WHERE EXTRACT(YEAR FROM discovery_date) < 1800;
```


---


**Explanation:** এই কোয়েরিতে species টেবিলের সেই সব row-র conservation_status কলামের মান 'Historic' করা হয়েছে, যাদের discovery_date সাল ১৮০০ সালের আগের।

---
⚠️ Note:

```WHERE``` ক্লজ ব্যবহার না করলে টেবিলের সবগুলো row আপডেট হয়ে যাবে, যা অনেক সময় বিপজ্জনক হতে পারে।
তাই প্রয়োজনে অবশ্যই নির্দিষ্ট শর্তসহ ```WHERE``` ক্লজ ব্যবহার করা উচিত।


---

## 4. What is the significance of the ```JOIN``` operation, and how does it work in PostgreSQL?

**Explanation:** ```JOIN``` ক্লজ ব্যবহার করা হয় দুটি বা তার বেশি টেবিলের row গুলোকে একসাথে combine করার জন্য। এটি সাধারণত এমন দুটি টেবিলের মধ্যে প্রয়োগ করা হয় যেগুলোর মধ্যে সম্পর্ক থাকে — যেমন একটি টেবিলের primary key ও অন্য টেবিলের foreign key এর মাধ্যমে।

যদি tableA এবং tableB এর মধ্যে JOIN করতে চাও, তাহলে তাদের মধ্যে কমন কলাম (related column) থাকা আবশ্যক — যেমন, tableA তে থাকা একটি primary key, এবং tableB তে তার সংশ্লিষ্ট foreign key।

**Types of JOIN in PostgreSQL:** ```INNER JOIN```, ```LEFT JOIN```, ```RIGHT JOIN```, ```FULL JOIN```


### ✅ Example: Using ```INNER JOIN```, ```LEFT JOIN```, ```RIGHT JOIN```, ```FULL JOIN``` Clause

```sql
-- Inner Join

SELECT * FROM post
    JOIN users on post.user_id = users.id;

SELECT * FROM post
    INNER JOIN users on post.user_id = users.id;
````
**Explanation:** এই ```JOIN``` শুধুমাত্র সেইসব post এবং user এর তথ্য ফেরত দেয় যাদের মধ্যে user_id এবং id কলামে মিল আছে। অর্থাৎ, দুটি টেবিলে যেসব row একে অপরের সাথে সম্পর্কিত শুধু সেগুলোরই ডেটা পাওয়া যায়।

---
---

```sql
-- Left join

SELECT * FROM post
    LEFT JOIN users ON post.user_id = users.id;
````
**Explanation:** এই ```JOIN``` users টেবিলের সব row ফেরত দেয় — এমনকি যদি কোনো user-এর জন্য মিল থাকা post না-ও থাকে। মিল না থাকা post-এর ডেটা NULL হবে।

---

```sql
--Right join

SELECT * FROM post
    RIGHT JOIN users ON post.user_id = users.id;
````
**Explanation:** এই ```JOIN``` users টেবিলের সব row ফেরত দেয় — এমনকি যদি কোনো user-এর জন্য মিল থাকা post না-ও থাকে। মিল না থাকা post-এর ডেটা NULL হবে।

---
```sql
-- Full join

SELECT * FROM post
    FULL JOIN users ON post.user_id = users.id;
```
**Explanation:**এই JOIN post এবং users — দুই টেবিলের সব row ফেরত দেয়। যেসব row মিলে যায়, সেগুলো একসাথে দেখায়। যেগুলো মেলে না, সেগুলোর অন্য টেবিলের অংশে NULL দেখায়।

---

## 5. How can you calculate aggregate functions like ```COUNT()``` ```SUM()``` and ```AVG()``` in PostgreSQL?

**Explanation:** Aggregate functions একটি set of value এর উপর কাজ করে এবং সেই সেটের উপর ভিত্তি করে একটি মাত্র মান (single summary value) প্রদান করে।

---

এগুলি সাধারণত টেবিলের একাধিক row-এর উপর অপারেশন চালায় এবং প্রায়শই GROUP BY ক্লজের সাথে ব্যবহার করা হয়।

---

সহজভাবে বললে, aggregate functions পুরো টেবিলের ডেটাকে একটি সেট (set) হিসেবে বিবেচনা করে এবং সেই সেটের মধ্যে function use করে একটি মাত্র ফলাফল রিটার্ন করে।

### ✅ Example: Using Aggregate functions like ```COUNT()``` ```SUM()``` and ```AVG()```

```sql
-- 1. AVG() Aggregate Function

SELECT avg(age) FROM students;

```
---

**Explanation:** এই কোয়েরিটি students টেবিলের age কলামের সব row-এর মান যোগ করে তাদের গড় বের করবে। ফলাফল হিসেবে একটি সংখ্যাই (average age) দেখাবে।

---

```sql
-- 2. SUM() Aggregate Function

SELECT sum(marks) FROM students;

```
---

**Explanation:** students টেবিলের marks কলামের সব মান (প্রত্যেক student’s নম্বর) একসাথে যোগ করে মোট নম্বর হিসাব করা হবে। এটি একটি মাত্র মান (total marks) return করবে।

---

```sql
-- 3. COUNT() Aggregate Function

SELECT count(*) FROM students;

```
---

**Explanation:** এই কোয়েরিটি students টেবিলে মোট কতজন student আছে অর্থাৎ, মোট কতটি row আছে  সেটি count করে একটি সংখ্যা রিটার্ন করবে।

---



