# 🔍 SQL Server – Top Logical Reads Query Analyzer (Memory-Intensive Queries)

![SQL Server](https://img.shields.io/badge/SQL%20Server-Performance%20Tuning-red?logo=microsoftsqlserver)
![Category](https://img.shields.io/badge/Category-Query%20Analysis-blue)
![Language](https://img.shields.io/badge/Language-TSQL-purple)
![Purpose](https://img.shields.io/badge/Purpose-Top%20Logical%20Reads%20Detection-green)
![Author](https://img.shields.io/badge/Author-Mahdi%20Lorvand-orange)

This powerful SQL script helps DBAs identify the **top memory-consuming queries** on the entire SQL Server instance by analyzing *total logical reads*.  
It is one of the most essential scripts for performance troubleshooting and query tuning.

---

## 🌐 Languages  
- 🇮🇷 [فارسی](#-نسخه-فارسی)  
- 🇬🇧 [English](#-english-version)  
- 🇸🇦 [العربية](#-الإصدار-العربي)

---

## 🇮🇷 نسخه فارسی

### 🧠 معرفی  
این اسکریپت یکی از مهم‌ترین ابزارهای Performance Tuning در SQL Server است.  
با اجرای این کوئری می‌توانید **پُرمصرف‌ترین کوئری‌ها از نظر Logical Read** را در کل Instance شناسایی کنید.  

این موضوع در موارد زیر بسیار کاربردی است:  
- عیب‌یابی کندی دیتابیس  
- بررسی Memory Pressure  
- یافتن Missing Index ها  
- تحلیل Query Plan  

---

### 🚀 ویژگی‌ها  
✅ شناسایی Top 50 کوئری با بیشترین Logical Reads  
✅ نمایش Query Text و Database مربوطه  
✅ میانگین و حداقل/حداکثر Logical Reads  
✅ نمایش Worker Time و Elapsed Time  
✅ تشخیص Missing Index در Query Plan  
✅ ایده‌آل برای Performance Tuning سطح حرفه‌ای  

---

### 🧾 اسکریپت  
```sql
SELECT TOP(50) 
    DB_NAME(t.[dbid]) AS [Database Name],
    REPLACE(REPLACE(LEFT(t.[text], 255), CHAR(10),''), CHAR(13),'') AS [Short Query Text], 
    qs.total_logical_reads AS [Total Logical Reads],
    qs.min_logical_reads AS [Min Logical Reads],
    qs.total_logical_reads/qs.execution_count AS [Avg Logical Reads],
    qs.max_logical_reads AS [Max Logical Reads],   
    qs.min_worker_time AS [Min Worker Time],
    qs.total_worker_time/qs.execution_count AS [Avg Worker Time], 
    qs.max_worker_time AS [Max Worker Time], 
    qs.min_elapsed_time AS [Min Elapsed Time], 
    qs.total_elapsed_time/qs.execution_count AS [Avg Elapsed Time], 
    qs.max_elapsed_time AS [Max Elapsed Time],
    qs.execution_count AS [Execution Count], 
    CASE WHEN CONVERT(nvarchar(max), qp.query_plan) COLLATE Latin1_General_BIN2 
         LIKE N'%<MissingIndexes>%' THEN 1 ELSE 0 END AS [Has Missing Index],
    qs.creation_time AS [Creation Time]
FROM sys.dm_exec_query_stats AS qs WITH (NOLOCK)
CROSS APPLY sys.dm_exec_sql_text(plan_handle) AS t 
CROSS APPLY sys.dm_exec_query_plan(plan_handle) AS qp 
ORDER BY qs.total_logical_reads DESC OPTION (RECOMPILE);
🧑‍💻 نویسنده
مهدی لوروند (Mahdi Lorvand)
📧 mehdilorvand92@gmail.com
🔗 لینکدین: https://www.linkedin.com/in/mehdi-lorvand-08aa151a4/

🇬🇧 English Version
🧠 Overview
This script identifies the Top 50 queries with the highest logical reads across the SQL Server instance — an essential insight for diagnosing memory-intensive workload issues.

Logical reads directly impact:

Buffer pool usage

Query performance

CPU and memory pressure

Missing index detection

🚀 Features
✅ Detects the most memory-heavy queries
✅ Shows query text, DB name, execution count
✅ Calculates Min/Max/Avg Logical Reads
✅ Displays Worker Time & Elapsed Time stats
✅ Detects Missing Indexes inside the query plan
✅ Excellent for performance tuning and workload analysis

🧾 Script
(See above — identical)

🧑‍💻 Author
Mahdi Lorvand
📧 mehdilorvand92@gmail.com
🔗 LinkedIn: https://www.linkedin.com/in/mehdi-lorvand-08aa151a4/

🇸🇦 الإصدار العربي
🧠 المقدّمة
هذا السكربت يحدّد أعلى 50 استعلاماً يستهلك الذاكرة عبر قياس أعلى Logical Reads في خادم SQL Server.
أداة مهمّة جداً لتحسين الأداء وتتبع مشاكل الذاكرة.

🚀 المميزات
✅ تحديد الاستعلامات ذات أعلى Logical Reads
✅ عرض النص المختصر للاستعلام
✅ حساب الحد الأدنى/الأقصى/المتوسط للـ Logical Reads
✅ قراءة Worker/Elapsed Time
✅ اكتشاف Missing Index داخل الـ Query Plan
✅ مثالي لعمليات Performance Tuning المتقدمة

🧾 السكربت
(See above)

🧑‍💻 المؤلف
مهدي لورفند (Mahdi Lorvand)
📧 mehdilorvand92@gmail.com
🔗 LinkedIn: https://www.linkedin.com/in/mehdi-lorvand-08aa151a4/

