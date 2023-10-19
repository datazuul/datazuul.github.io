---
layout: post
title: SQL-Cheatsheet
author: Ralf Eichinger
toc: true
---

Collection of useful SQL (Structured Query Language) commands to query relational databases.
The SQL examples are base on PostgreSql 15 database (https://www.postgresql.org/docs/15/sql.html).

# SELECT: Selecting data

## Search in a JSONB column

* Task: "Count records containing an empty JSON property at beginning of JSON in a JSONB column 'label'."

    Solution:

```sql
select count(*) from digitalobjects where label::jsonb::text like '{""%';
```

# INSERT: Inserting data


# UPDATE: Updating data

## Setting JSON into a JSONB column

* Task: Update a JSONB column 'content' with JSON given as String.

  Solution:

```sql
update articles set content = '{"de": {"type": "doc", "content": [{"type": "paragraph", "content": [{"text": "Als im Jahre 1790 der Pariser Akademie der Wissenschaften ... von Eisenmassen irdischen Ursprungs zu unterscheiden.", "type": "text"}]}]}}'::text::jsonb where uuid = '0666ad69-5013-42b6-bd72-39161d13ef54';
```

## Search and Replace in a column

### Search and Replace in a VARCHAR column

* Task: "Search for 'http://localhost:23232/' anywhere in the String of a VARCHAR column 'http_base_url' and replace it with 'https://api.alexandria.de/iiif/'."

  Solution:

```sql
UPDATE fileresources_image SET http_base_url = REPLACE(http_base_url, 'http://localhost:23232/', 'https://api.alexandria.de/iiif/');
```

### Search and Replace in a JSONB column

* Task: "Search for 'http://localhost:23232/' anywhere in a JSON of a JSONB column 'content' and replace it with 'https://api.alexandria.de/iiif/'."

    Solution:

```sql
update articles set content = (REPLACE(content::jsonb::text, 'http://localhost:23232/', 'https://api.alexandria.de/iiif/'))::jsonb where content::jsonb::text like '%localhost:23232%';
```
