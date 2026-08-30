# SQLite as a document database

SQLite's JSON functions and generated columns make it possible to store a JSON document, expose the fields you query, and index them without maintaining duplicate data in application code.

```sql
create table events (
  body text not null,
  id text generated always as (json_extract(body, '$.id')) virtual not null,
  kind text generated always as (json_extract(body, '$.kind')) virtual
);
```

Insert through `json()` so SQLite validates and canonicalises the document:

```sql
insert into events (body)
values (json('{"id":"evt_42","kind":"invoice.paid"}'));
```

`json_extract()` also validates the JSON whenever SQLite computes a generated column. Invalid documents fail at insert time, and `not null` or `check` constraints can require fields that every document must contain.

```sql
insert into events (body) values ('');
-- Runtime error: malformed JSON

insert into events (body) values ('{}');
-- Runtime error: NOT NULL constraint failed: events.id
```

Use `virtual` columns when the extracted value does not need to be stored separately. They can still be indexed:

```sql
create index events_id on events (id);

explain query plan
select * from events where id = 'evt_42';
```

The schema can start with only a JSON body and grow as useful fields emerge. Add another virtual column and index it later; this works well for payloads such as webhooks where the complete event should be retained before every field is known.

```sql
alter table events add column customer_id text
  generated always as (json_extract(body, '$.customer.id')) virtual;

create index events_customer_id on events (customer_id);
```

`stored` generated columns cache their values, but cannot be added with `alter table`. Prefer `virtual` for flexible JSON-backed schemas and add indexes only for fields used in lookups.

[source](https://dgl.cx/2020/06/sqlite-json-support)
