# Interview Preparation Guide – Roopkumar Kodamanchili

## 1. Self Introductions
## 1.1 Two-Minute Self Introduction

**Thank you for the opportunity. My name is Roopkumar Kodamanchili, and I’m currently working as a Lead Digital Platform Engineer – Business Intelligence at Agivant Technologies, supporting Google as a client. I bring over 10 years of experience in Business Intelligence, Data Architecture, and Cloud Analytics.**

**In my current role, I design and optimize scalable data platforms on Google Cloud and AWS, handling over 10 million rows of data daily. I’ve led projects in Process Mining, AI/ML-driven Data Quality solutions, and Generative AI applications, where we achieved up to 25% efficiency improvements in data transcription and reporting. I also build executive-level dashboards in Power BI and Looker Studio that provide real-time insights to business stakeholders.**

**Before this, I worked at Blubirch as a Senior Data Analyst, where I developed Returns Automation and Dealer Management platforms, and at MotifWorks Tech as a Software Engineer/Business Analyst, where I focused on ETL pipeline optimization and interactive dashboards. Across these roles, I’ve consistently delivered measurable business impact—such as 99% data acquisition accuracy and 12% margin improvements through ML models.**

**Alongside technical expertise, I hold certifications including Google Professional Data Engineer and ScrumMaster, and I’m passionate about integrating AI and cloud technologies into BI ecosystems to drive transformation. Looking ahead, I’m excited about opportunities where I can apply my data architecture skills and leadership experience to help enterprises scale their analytics, improve decision-making, and prepare for next-generation AI adoption.**

## 1.2 Thirty-Second Self Introduction

**Hi, I’m Roopkumar Kodamanchili. I’m a Lead Digital Platform Engineer – Business Intelligence with over 10 years of experience in Data Architecture, BI, and Cloud Analytics. I currently support Google as a client, where I design scalable data platforms, build executive dashboards, and integrate AI/ML solutions that improve efficiency and decision-making. I hold certifications including Google Professional Data Engineer and ScrumMaster, and I’m passionate about using cloud and Generative AI to transform BI ecosystems.**

## 2. Full Question Bank (40–50 Questions)
Data Architecture & Modeling
**1. Explain the difference between OLTP and OLAP systems.**
*Answer:*

OLTP (Online Transaction Processing) – These systems handle day-to-day operations with lots of small transactions. They are optimized for fast inserts and updates.

Example (from your work): In the Google client project, the ticketing system that records when a user opens, updates, or closes a ticket is an OLTP system. Each action is a quick transaction stored in the database.

OLAP (Online Analytical Processing) – These systems are built for analytics and reporting. They store large volumes of historical data, optimized for complex queries and aggregations.

Example (from your work): The Power BI and Looker Studio dashboards you built, which analyze 10M+ rows daily in BigQuery, are OLAP systems. They let managers see trends like average ticket resolution time or SLA compliance.

**2. How do you approach designing a scalable data warehouse?**

Answer:
I follow a step-by-step approach:

1.	Understand Business Needs
	*First, I gather requirements from stakeholders: What KPIs matter? Who are the users (analysts, CXOs, operations)?*
	*Example: At Google client project, we identified KPIs like ticket resolution time and process efficiency.*

2.	Define Data Sources
	*List where the data is coming from (transactional DBs, APIs, logs, flat files).*
	*Example: For Blubirch, I had to combine data from PostgreSQL, Firestore, and BigQuery.*

3.	Data Modeling
	*Use Star Schema (fact + dimensions) for OLAP workloads.*
	*Example: In Power BI dashboards, I modeled Fact_Tickets (open, close times, SLA breaches) linked with Dim_User, Dim_Region, Dim_Time.*

4.	Storage & Partitioning
	*Choose scalable cloud storage (BigQuery, Redshift, Snowflake).*
	*Apply partitioning (by date) and clustering (by region/product) for faster queries.*
	*Example: In BigQuery, partitioning daily ticket logs reduced cost by 30%.*

5.	Pipeline (ETL/ELT)
	*ELT preferred → load raw data first, then transform in warehouse.*
	*Use orchestration tools (Airflow, Dataflow, dbt).*
	*Example: At Agivant, I optimized ELT in BigQuery handling 10M+ rows daily.*

6.	Governance & Security
	*Define metadata catalog, data dictionary, RLS (row-level security) for compliance.*
	*Example: In Power BI, I set RLS so managers could only see their team’s tickets.?*

7.	Performance Optimization
	*Pre-aggregate common queries (materialized views).*
	*Cache frequent results.*
	*Monitor usage and adjust indexing/partitioning.*

👉 In simple terms:
***Start with KPIs → Model data → Use partitioning & ELT pipelines → Secure & optimize for scale.***

**3. What are fact tables and dimension tables? Provide examples.**

*Answer:*

*Fact Table*

Stores measurable events (transactions, metrics).

Contains numeric values you want to analyze (like counts, amounts, durations).

Usually very large (millions of rows).

Linked to dimensions using foreign keys.

*Dimension Table*

Stores descriptive attributes that give context to facts.

Contains textual info (names, categories, regions, dates).

Typically smaller in size, but provides filtering and grouping power.

**✅ Example from your projects:**

In your Google client ticketing dashboards (Process Mining POC):

Fact_Tickets → Each row = one ticket event (open, close, SLA breached).

Columns: Ticket_ID, Open_Time, Close_Time, SLA_Breach_Flag, Resolution_Duration.

Dim_User → Details of employees/customers.

Columns: User_ID, Name, Role, Team.

Dim_Time → Standardized calendar for analysis.

Columns: Date_ID, Day, Month, Quarter, Year.

Dim_Region → Location attributes.

Columns: Region_ID, Country, City.

So when a manager asks: “What’s the average resolution time for tickets in APAC handled by Team X last quarter?”
👉 The query joins Fact_Tickets with Dim_Region, Dim_User, and Dim_Time.

*👉 In short:*

Facts = numbers (what happened).

Dimensions = context (who, where, when, what).


**4. What is data normalization vs denormalization?**

*Short answer:*

Normalization = split data into related tables to reduce duplication and enforce consistency (best for OLTP).

Denormalization = combine data to reduce joins and speed up reads (best for OLAP/reporting).

*Normalization (OLTP-focused)*

Goal: avoid duplicates, maintain integrity.

How: 1NF/2NF/3NF; use foreign keys; small, focused tables.

Pros: fewer anomalies, cheaper updates, consistent master data.

Cons: more joins → slower analytics.

*Your example:*
At Blubirch, you kept Customers and Orders separate. Only customer_id is stored in orders; customer attributes live in customers. This made updates (e.g., phone change) consistent everywhere.

*Denormalization (OLAP-focused)*

Goal: faster reads/aggregations for analytics.

How: pre-join / duplicate descriptive fields into wide fact tables or materialized views.

Pros: fewer joins, faster dashboards and aggregates.

Cons: duplicate data, heavier updates, risk of drift without governance.

*Your example:*
For Google client dashboards in BigQuery + Power BI/Looker, you kept a wide fact_tickets with user/region/time keys and sometimes carried descriptive fields (e.g., region_name) to speed up common queries and reduce joins—cutting costs ~30% with partitioning + clustering.

Tiny table sketch

*Normalized (OLTP):*

customers(customer_id PK, name, email, region_id FK)
regions(region_id PK, region_name)
orders(order_id PK, customer_id FK, order_dt, amount)


*Denormalized (OLAP):*

fact_orders(order_id, order_dt, amount, customer_id, customer_name, region_id, region_name)

When to choose which

Use normalization for systems that are updated constantly (ticketing, orders, user profiles).

Use denormalization for analytics warehouses/lakes where read speed matters (dashboards, ad-hoc SQL, ML features).

*One-liner:*

Normalize to write fast & stay consistent; denormalize to read fast & analyze.

**5. How do you handle schema changes in a live warehouse?**

*Short answer:*
Use controlled, backward-compatible, versioned changes with staging/testing before pushing into production.

*Common Types of Schema Changes*

Add a column → safest, usually backward compatible.

Drop/rename a column → risky, breaks queries and dashboards.

Change data type → must be handled with care (migrate data gradually).

Partition/cluster changes → usually require creating a new table and migrating.

*Approach (Step-by-Step)*

*Version control your schema*

Use dbt, Liquibase, or SQL migration scripts in Git.

Example: In your Agivant/Google project, you kept ETL SQL scripts in Git and peer-reviewed schema updates.

*Test in staging first*

Apply changes on a staging dataset and validate against existing queries/dashboards.

Example: You tested adding an sla_breach_flag column in staging before BigQuery prod.

*Backward-compatible rollout*

Add new columns as nullable/defaults first.

Deprecate old columns gradually (mark as deprecated, keep for a while).

Update pipelines + dashboards

Adjust ETL to populate new columns.

Sync Power BI/Looker dashboards so measures don’t break.

Communicate with stakeholders

*Notify analysts/teams about schema changes.*

Example: When you added Resolution_Duration field, you shared a data dictionary update with CXO dashboard users.

Monitor after deployment

Run validation queries (row counts, aggregates).

Watch dashboards/logs for failures.

Example from your work

In the Google Process Mining POC, when you added the sla_breach_flag to Fact_Tickets, you:

First introduced it as a nullable column in staging.

Updated the ELT job in BigQuery to calculate it.

Modified Power BI dashboards to use it (while old queries still worked).

Only after validation, you enforced NOT NULL in production.

*One-liner takeaway*

“Add safe changes incrementally, test in staging, keep backward compatibility, and communicate with downstream users.”

## ETL / ELT & Cloud Platforms
**1. What’s the difference between ETL and ELT? Which do you prefer?**

*Short answer:*

ETL (Extract → Transform → Load): transform before loading.

ELT (Extract → Load → Transform): load raw first, then transform inside the warehouse.

ETL (Traditional Approach)

Process:

Extract data from sources.

Transform in an external ETL tool/server.

Load into warehouse.

Best for: legacy warehouses with limited compute (SQL Server, Oracle).

Pros: keeps warehouse clean; only transformed data enters.

Cons: slow, complex pipelines; external compute bottleneck.

Example from your work:
At MotifWorks Tech, ETL was used for QA/analytics systems. Transformations (joins, cleaning, formatting) happened before loading into SQL Server. This helped maintain clean transactional stores but was less scalable.

ELT (Modern Approach)

Process:

Extract data from sources.

Load raw data directly into warehouse (BigQuery, Snowflake, Redshift).

Transform inside the warehouse using SQL/dbt.

Best for: modern cloud warehouses with massive scale.

Pros: scalable, flexible, can re-run transformations on demand; raw data preserved.

Cons: requires governance to avoid a messy “data swamp.”

Example from your work:
At Agivant (Google project), you optimized BigQuery pipelines to load raw ticket logs first (10M+ rows daily), then applied transformations (partitioning, clustering, SLA calculations) inside BigQuery. This reduced pipeline complexity and enabled analysts to rerun queries on raw data without waiting for ETL jobs.

Preference

👉 You prefer ELT because:

Cloud warehouses (BigQuery, Snowflake, Redshift) are built for it.

Keeps raw + transformed data → flexibility.

Faster iteration for analytics and ML pipelines.

One-liner takeaway

ETL = old-school (transform outside, load clean), ELT = modern (load raw, transform inside). For cloud-scale projects like yours → ELT wins.

**2. How do you ensure data quality in pipelines?**

Short answer:
By embedding validation checks, monitoring, and governance into every stage of the pipeline — from ingestion to reporting.

Key Practices

Validation at Ingestion

Schema checks (column types, nullability).

Reject/flag invalid rows early.

Example: In Blubirch pipelines, you ensured that all sales amounts were non-negative before loading to BigQuery.

Automated Data Quality Frameworks

Use tools like Great Expectations or dbt tests.

Define rules: ranges, uniqueness, referential integrity.

Example: In your AI/ML Data Quality Solution POC, you built batch-wise checks with Great Expectations and displayed results in a Streamlit dashboard.

Quarantine & Alerts

Move failed records to a quarantine bucket for review.

Trigger Slack/Email alerts if thresholds break.

Example: In your POC, when >2% rows failed, pipeline raised alerts and stored failures separately.

Incremental Testing

Run validation on incremental loads, not just full reloads.

Example: In Google client ticket data, daily incremental loads were tested for duplicate Ticket_IDs and SLA outliers.

Data Governance & Documentation

Maintain data dictionary, ownership, and lineage.

Example: You updated data dictionaries when new columns like sla_breach_flag were added, so analysts had clear definitions.

Monitoring & SLA Tracking

Monitor freshness (how recent data is).

Monitor completeness (expected row counts).

Example: In Power BI dashboards, you added metadata KPIs (last refresh timestamp, load status) for transparency.

Real-world Example (from your profile)

In your Streamlit + Great Expectations pipeline, you:

Generated 10K rows per batch.

Ran checks (null %, min/max ranges, duplicates).

Quarantined failed rows.

Displayed batch quality scores (freshness, completeness) in a live dashboard for business users.

One-liner takeaway

“I embed automated checks, quarantine bad data, and surface quality metrics through dashboards — so business leaders trust the data before making decisions.”


**3. What are partitioning and clustering in BigQuery?**

Short answer:
Both are optimization techniques in BigQuery that reduce cost and improve query performance by scanning less data.

Partitioning

Definition: Splitting a large table into smaller chunks based on a column (commonly DATE, TIMESTAMP, or INTEGER RANGE).

Benefit: Queries only scan the partitions they need → lower cost + faster.

Example (your work): In your Google project, you partitioned fact_tickets by Open_Date. A query for “last 7 days” scanned only 7 partitions instead of the entire 10M+ row table, saving ~30% cost.

Clustering

Definition: Organizing data within a partition based on the values of one or more columns.

Benefit: Improves filter + aggregation performance (especially on high-cardinality columns).

Example (your work): You clustered fact_tickets by Region and Priority. So when CXOs filtered dashboards by APAC or Priority=High, BigQuery skipped irrelevant blocks, making queries much faster.

Combined Use

Partition by date/time (most common query filter).

Cluster by high-cardinality columns (like user_id, region, product).

Example from your project:

Partitioned by Open_Date.

Clustered by Region + Priority.

Result: SLA monitoring dashboards in Power BI refreshed in seconds instead of minutes.

One-liner takeaway

Partitioning reduces data scanned by time, clustering speeds up queries by sorting data within partitions.

**4. Explain how you’d migrate from on-prem SQL Server to BigQuery.**

Short answer:
Migration = Assess → Extract → Load → Transform → Validate → Optimize.

Step-by-Step Approach

Assessment & Planning

Analyze source schema (tables, indexes, stored procedures).

Map SQL Server datatypes → BigQuery equivalents.

Identify partitioning & clustering strategy.

Example: At Blubirch, you mapped money/decimal fields in SQL Server to NUMERIC in BigQuery to avoid precision loss.

Extract & Transfer

Use tools:

Dataflow / Datastream (GCP-native).

SQL Server Integration Services (SSIS).

Third-party tools (Informatica, Talend).

Transfer bulk data to Cloud Storage first.

Load into BigQuery

Use bq load or GCP Transfer Service to load from Cloud Storage.

For incremental sync: use change data capture (CDC) on SQL Server and stream updates.

Example: Daily incremental job in your Google project loaded ticket updates, while master reference tables (users, regions) were reloaded fully.

Transform in BigQuery (ELT)

Rewrite stored procedures into BigQuery SQL / dbt models.

Apply partitioning (by date) + clustering (by region or product).

Example: You transformed Fact_Orders into Fact_Tickets with calculated SLA fields directly inside BigQuery for CXO dashboards.

Validation

Compare record counts, sums, and aggregates between SQL Server & BigQuery.

Automate validation queries.

Example: In Blubirch migration, you validated SUM(sales_amount) and row counts for each day to ensure 99%+ accuracy.

Optimization

Create materialized views for heavy queries.

Set table expiration for staging data.

Monitor query cost & adjust partitioning strategy.

Example: After migration, you reduced costs by 30% by clustering BigQuery tables on Region.

Real-World Example (from your profile)

At Blubirch, when moving from PostgreSQL/SQL Server to BigQuery, you:

Loaded raw order/returns data into Cloud Storage.

Used BigQuery ELT for transformations.

Built dashboards in Power BI / Google Data Studio.

Optimized performance with partitioning by date + clustering by region.

One-liner takeaway

Migrate in phases: assess, load raw to BigQuery, transform with ELT, validate with aggregates, and optimize with partitioning/clustering.

**5. How do you manage incremental vs full data loads?**

Short answer:

Full load = reload everything.

Incremental load = load only new or changed data.
The choice depends on table size, update frequency, and business requirements.

Full Load

Definition: Truncate and reload the entire dataset.

Best for: small tables, reference/master data.

Pros: simple, no risk of missing updates.

Cons: heavy on large tables (slow + costly).

Example (your work):
In the Google project, you did a full reload of Dim_Region and Dim_User daily since they were small (few hundred rows).

Incremental Load

Definition: Only new or updated rows are processed.

Methods:

Timestamps (e.g., last_updated > yesterday).

Change Data Capture (CDC) from source DB logs.

Primary key comparison.

Best for: large fact tables.

Pros: efficient, faster, cheaper.

Cons: requires reliable tracking mechanism.

Example (your work):
For Fact_Tickets (~10M+ rows daily), you used incremental loads with Open_Date and Last_Updated_Timestamp. Only changed tickets were appended/updated in BigQuery.

Hybrid Approach

Use incremental loads for transactional/fact tables.

Use full loads for small reference/dimension tables.

One-liner takeaway

Incremental loads = efficient for large tables; full loads = safe for small or static reference tables. In practice, I use both depending on table size & update frequency.

👉 Do you want me to also give a sample SQL snippet for incremental load (using last_updated timestamp) so you can quote it in interviews?


## BI & Dashboards
11.	1. What KPIs would you track in an executive BI dashboard?
12.	2. How do you handle dashboard performance issues (slow refresh)?
13.	3. How do you implement row-level security in BI tools?
14.	4. Explain self-service BI. What challenges come with it?
15.	5. What are best practices for dashboard design for CXOs?
## Machine Learning / AI
16.	1. How have you used AI/ML in BI?
17.	2. How would you integrate Generative AI into BI systems?
18.	3. What is the difference between supervised and unsupervised learning?
19.	4. Explain a real project where ML improved business performance.
20.	5. How do you monitor and retrain ML models in production?
## Graph Databases & Process Mining
21.	1. When would you use a graph database instead of a relational DB?
22.	2. Explain nodes, edges, and properties in graph databases.
23.	3. What are key insights you can derive from process mining?
24.	4. What is a Directly-Follows Graph (DFG)?
25.	5. How would you design a fraud detection system using graph DBs?
## System Design & Architecture
26.	1. Design a data warehouse for an e-commerce company.
27.	2. How would you design a real-time analytics pipeline?
28.	3. What are tradeoffs between batch and stream processing?
29.	4. Explain event-driven architecture and its use cases.
30.	5. What factors influence your choice of cloud data warehouse?
Behavioral & Leadership
31.	1. Tell me about a time you led a cross-functional project.
32.	2. How do you handle conflicts with stakeholders over requirements?
33.	3. Describe a failure you experienced and how you handled it.
34.	4. How do you mentor junior team members?
35.	5. What’s your leadership style when handling large data projects?
3. Programming Questions (30–40)
36.	1. Write a Python function to reverse a string.
37.	2. Find the factorial of a number using recursion.
38.	3. Implement Fibonacci series using iteration and recursion.
39.	4. Check if a string is a palindrome.
40.	5. Find the largest element in a list.
41.	6. Remove duplicates from a list without using set().
42.	7. Find the second largest number in a list.
43.	8. Implement binary search in Python.
44.	9. Write a function to count the frequency of words in a string.
45.	10. Check if two strings are anagrams.
46.	11. Implement a stack using a list.
47.	12. Implement a queue using collections.deque.
48.	13. Find the missing number in an array of 1 to n.
49.	14. Write a program to find prime numbers up to 100.
50.	15. Check if a number is Armstrong or not.
51.	16. Find the greatest common divisor (GCD) of two numbers.
52.	17. Write code to flatten a nested list.
53.	18. Sort a list of tuples by the second element.
54.	19. Find the longest substring without repeating characters.
55.	20. Implement a linked list in Python.
56.	21. Detect a cycle in a linked list.
57.	22. Reverse a linked list.
58.	23. Find intersection of two linked lists.
59.	24. Implement depth-first search (DFS) on a graph.
60.	25. Implement breadth-first search (BFS) on a graph.
61.	26. Find shortest path in an unweighted graph using BFS.
62.	27. Implement quicksort algorithm.
63.	28. Implement merge sort algorithm.
64.	29. Solve the classic 8 balls problem (find heavier ball).
65.	30. Write SQL to find the second highest salary in a table.
66.	31. Write SQL to get employees who joined in the last 30 days.
67.	32. Explain ACID properties in databases.
68.	33. Explain indexing and how it improves query performance.
69.	34. Design a REST API in Python using Flask.
70.	35. Parse JSON data from an API response in Python.
71.	36. Explain difference between multiprocessing and multithreading.
72.	37. Use Python generators to handle large data streams.

📊 SQL Questions (15+)

Write a SQL query to find the second highest salary in an Employee table.

How do you find duplicate rows in a table?

Write a SQL query to get employees who joined in the last 30 days.

Difference between INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL JOIN – with examples.

How to calculate running total in SQL?

How do you find Nth highest salary in SQL?

Explain window functions with an example (ROW_NUMBER(), RANK(), DENSE_RANK()).

Write a query to pivot rows into columns.

Write SQL to find employees with salary > average salary in their department.

Difference between WHERE and HAVING clause.

How do you optimize slow SQL queries?

How to detect and remove orphan records (foreign key mismatch)?

Write a query to find top 3 products by sales per region.

What is the difference between DELETE, TRUNCATE, DROP?

How to implement slowly changing dimensions (SCD) in SQL/ETL?

📈 Power BI / DAX Questions (15+)

What is the difference between calculated column and measure in DAX?

Write a DAX formula for Year-to-Date (YTD) Sales.

How do you calculate % of Total Sales in DAX?

Explain ALL() vs REMOVEFILTERS() in DAX.

How do you implement row-level security (RLS) in Power BI?

Difference between DirectQuery, Import, and Composite models.

How do you handle large datasets in Power BI?

Explain context transition in DAX with an example.

What is the difference between SUM() and SUMX()?

How do you create a rolling 12-month average in DAX?

Explain STAR vs SNOWFLAKE schema for Power BI models.

How do you optimize a slow Power BI dashboard?

Write a DAX formula for Cumulative Total by Month.

What is the difference between Calculated Table vs Calculated Column?

How do you handle many-to-many relationships in Power BI?

4. System Design Questions (20–30)
73.	1. Design a URL shortening service like Bitly.
74.	2. Design a real-time chat application like WhatsApp.
75.	3. Design a scalable recommendation system for e-commerce.
76.	4. Design an online ticket booking system.
77.	5. Design a ride-hailing system like Uber.
78.	6. Design a logging and monitoring system for microservices.
79.	7. Design a scalable notification system (email/SMS/push).
80.	8. Design a distributed file storage system like Google Drive.
81.	9. Design an API rate limiter.
82.	10. Design a scalable fraud detection system for transactions.
83.	11. Design YouTube’s video streaming architecture.
84.	12. Design a food delivery app like Swiggy/Zomato.
85.	13. Design a scalable metrics dashboard system.
86.	14. Design a search engine for documents.
87.	15. Design a system to detect anomalies in IoT sensor data.
88.	16. Design a workflow automation engine like Airflow.
89.	17. Design a global e-commerce system with multi-currency support.
90.	18. Design a scalable event-driven architecture for stock trading.
91.	19. Design a caching system for a high-traffic website.
92.	20. Design a hospital management system with role-based access.
93.	21. Design a cloud-based ETL pipeline for real-time analytics.
94.	22. Design an authentication system with OAuth2 and JWT.
95.	23. Design an online examination platform.
96.	24. Design a scalable file upload service (like Google Photos).
97.	25. Design an employee resource management system.
98.	26. Design a scalable analytics platform for IoT data.
99.	27. Design a warehouse inventory management system.
100.	28. Design LinkedIn’s people you may know feature.
101.	29. Design a supply chain management system.
102.	30. Design a system for airline reservation and check-in.
