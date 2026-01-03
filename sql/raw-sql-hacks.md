# Raw SQL Hacks for Fun and Profit

### 1. NOT EXISTS Instead of LEFT JOIN with IS NULL

Engines optimize NOT EXISTS better, avoids result bloat and stops early on match.

```sql
select id
from users u
where not exists (
select 1
from orders o
where o.user_id = u.id
)
```

Optimal anti-join pattern backed by performance evals. [[dba.stackexchange](https://dba.stackexchange.com/questions/121034/best-practice-between-using-left-join-or-not-exists?utm_source=chatgpt.com) [thoughtspot](https://www.thoughtspot.com/data-trends/data-modeling/optimizing-sql-queries?utm_source=chatgpt.com) [erikdarling](https://erikdarling.com/why-not-exists-makes-more-sense-than-left-joins/?utm_source=chatgpt.com)]

### 2. CASE WHEN to Avoid Joining Lookup Tables

Inline static mappings cleaner, faster, no extra join.

```sql
select case status
when 'a' then 'active'
when 'i' then 'inactive'
else 'unknown'
end as status_label
from accounts
```

Used everywhere in high-stakes dashboards.

### 3. DISTINCT ON with ORDER BY for First-Row-Per-Group

Works in Postgres/DuckDB. Simpler and faster than ROW_NUMBER windows.

```sql
select distinct on (user_id) *
from events
order by user_id, created_at desc
```

"SELECT DISTINCT ON … keeps only the first row…" - that's canonical [[johnnunemaker.com](https://www.johnnunemaker.com/postgres-distinct-on/?utm_source=chatgpt.com)].

### 4. FULL OUTER JOIN Emulation Using UNION ALL

For engines without FULL JOIN.

```sql
select _ from a left join b on a.id = b.id
union all
select _ from a right join b on a.id = b.id
where a.id is null
```

Pure SQL workaround, widely proven in cross-engine tools.

### 5. Push Filters Inside CTEs Manually

CTEs may get materialized - put your WHERE inside.

```sql
with active*users as (
select *
from users
where is*active = true
)
select *
from active_users
where signup_source = 'organic'
```

Filter pushdown is anti-optimizer horror if ignored ([dba.stackexchange.com](https://dba.stackexchange.com/questions/121034/best-practice-between-using-left-join-or-not-exists?utm_source=chatgpt.com), [thoughtspot.com](https://www.thoughtspot.com/data-trends/data-modeling/optimizing-sql-queries?utm_source=chatgpt.com), [erikdarling.com](https://erikdarling.com/why-not-exists-makes-more-sense-than-left-joins/?utm_source=chatgpt.com)).

### 6. Bucketing Timestamps with Integer Math

Way faster than date functions.

```sql
select floor(epoch_ts / 3600) \* 3600 as hour_bucket
from logs
```

A straight-up speed hack for time-series without DB-specific functions.

### 7. Boolean XOR via AND/NOT

SQL lacks XOR. Build it clean:

```sql
where (is_new and not is_premium)
or (not is_new and is_premium)
```

Read-once, sum-twice truth. Minimal, tight, reliable.

### 8. Positional GROUP BY for Fast Edits

GROUP BY 1 saves you from rewriting column names during ad hoc runs.

```sql
select region, count(\*)
from users
group by 1
```

Dev life hack for quick iterations.

### 9. Dedup Before Join to Prevent Row Explosions

Force-deduping to prevent insane joins.

```sql
select *
from users u
join (
select distinct user_id from orders
) o on u.id = o.user_id
```

Prevents data multiplication without windowing logic.

### 10. Avoid SELECT \* in Joins

Tight projection avoids spilling temp files.

```sql
select u.id, u.name, o.id as order_id
from users u
join orders o on u.id = o.user_id
```

Clean queries, predictable performance (bennadel.com, timescale.com, wikipedia, risingwave.com, medium.com).

### 11. WHERE 1=0 for Dry-Run Stubs

Create structure without pulling rows.

```sql
select * from payments where 1=0
```

Scaffolding queries in one line.

### 12. COALESCE for Cleaner Boolean Logic

Use COALESCE to avoid null traps though be aware of perf implications.

where coalesce(is_active, false) = true

It's just CASE under the hood ([stackoverflow.com](https://stackoverflow.com/questions/23386750/does-use-of-coalesce-slows-down-the-query-performance?utm_source=chatgpt.com)).

### 13. Fake Number Sequences Using UNIONs

Simulate generate_series cheaply.

```sql
select a.n + b.n \* 10 as seq
from (select 0 as n union all select 1 union all select 2) a,
     (select 0 as n union all select 1 union all select 2) b
```

Clever, portable test data hack.

### 14. Emulate LEAD/LAG Without Window Functions

If window functions aren't an option:

```sql
select id,
(select min(id)
from events e2
where e2.id > e1.id
) as next_id
from events e1
```

Slow on big data, but works everywhere.

### 15. NULL-Safe Equality Without IS NOT DISTINCT FROM

Portable approach:

```sql
where (email = input_email
or (email is null and input_email is null))
```

Leverages three-valued logic ([dba.stackexchange.com](https://dba.stackexchange.com/questions/4274/performance-difference-for-coalesce-versus-isnull?utm_source=chatgpt.com), [stackoverflow.com](https://stackoverflow.com/questions/66893968/select-distinct-is-slower-than-expected-on-my-table-in-postgresql?utm_source=chatgpt.com), [wikipedia](https://en.wikipedia.org/wiki/Null_%28SQL%29?utm_source=chatgpt.com)).

### 16. INTERSECT and EXCEPT for Set Logic

Better expresses intent than joins in some cases.

```sql
select user_id from churned_users
intersect
select user_id from active_users
```

Backed by common set-logic patterns ([sqlperformance.com](https://sqlperformance.com/2012/12/t-sql-queries/left-anti-semi-join?utm_source=chatgpt.com), [sqlshack.com](https://www.sqlshack.com/t-sql-commands-performance-comparison-not-vs-not-exists-vs-left-join-vs-except/?utm_source=chatgpt.com)).

### 17. Fallback Row with UNION and NOT EXISTS

Guarantee at least one row:

```sql
select \* from products
union all
select 'no data', null, null
where not exists (select 1 from products)
```

Safe guard for dashboards expecting rows.

### 18. Width Bucketing with FLOOR

Reduce group explosion cleanly:

```sql
select floor(age / 10) _ 10 as age_bucket, count(_)
from users
group by 1
```

Quick and effective continuous-to-bucket conversion.
