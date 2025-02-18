---
title: SQL Style Guides
date: 2025-02-12 11:00:00 +/-0000
categories: [notes]
tags: [sql, best_practice, learning]     # TAG names should always be lowercase
description: Consistent styling for SQL
toc: false
comments: false
pin: false
#media_subpath: /path/to/media/
---


I like style guides. A consistently styled codebase is so much easier to work with. Like most people who have had to use someone else's style guide, I don't always agree with the details, but that's not really the point.

There are plenty of style guides available, including those from [Google](https://google.github.io/styleguide/) and [The Government Digital Service (GDS)](https://gds-way.digital.cabinet-office.gov.uk/manuals/programming-languages.html)

Languages like Java are always well represented, and of course Python has it's own [PEP-8 style guide](https://peps.python.org/pep-0008/), but SQL rarely makes it onto these lists. With providers like Snowflake and dbt advocating for use of consistent styling (in part because case-sensitivity and whitespace are starting to matter more in modern SQL tools) this may change. For now, of the few publicly available SQL style guides, the most well-known  - and hotly debated - is still [Simon Holywell's](https://www.sqlstyle.guide/), which is over 10 years old, and forked over 1.4k times on GitHub.

His follow-up post [SQL style guide misconceptions](https://www.simonholywell.com/post/2016/12/sql-style-guide-misconceptions/?utm_source=sqlstyle.guide-sqlstyle.guide&utm_medium=link&utm_campaign=footer-link) addresses most of the objections - and conveys his understandable frustration at the criticism - very clearly. However, [new readers continue to be outraged](https://www.reddit.com/r/programming/comments/1gr2jpg/sql_style_guide_by_simon_holywell/).

Most IDEs will support SQL formatting and linting these days, but codebases of poorly formatted SQL can still be a challenge. Fully expecting (and indeed receiving) a similar backlash, I had a go at writing my own version for a recent project. 

It was intended as a starting point for discussion rather than a finished guide. The process of putting it together gave me the opportunity to think carefully about SQL best practices generally.

---
# SQL Style Guide
## Overview

This set of guidelines defines the expected standard for SQL code used in the current project. 

### "Who do you think you are telling me what to do?[[1]](#1)"
Style guides are, by their nature, opinionated. Frequently there is no single objectively "best" approach; you often just have to choose one and stick to it. The overall goal is to encourage consistency - because consistent code is easier to read and understand.

The following guideline are based predominantly on (and in many cases directly copied from) Simon Holywell's SQL style guide [[2]](#2) and the GitLab handbook SQL style guide [[3]](#3). Links and attributions at the end of this document.


## General Guidance
* Be consistent. Even if you are not sure of the best way to do something, do it the same way throughout your code. It will be easier to read and make changes if they are needed.
* Be explicit. Defining something explicitly will ensure that it works the way you expect and it is easier for the next person, which may be you.

## Best Practices
* Understand the difference between the following related statements and use appropriately:
    - `UNION ALL` and `UNION`
    - `LIKE` and `ILIKE`
    - `NOT` and `!` and `<>`
    - `DATE_PART()` and `DATE_TRUNC()`
    - `IS NULL` and `= NULL`
* Prefer LOWER(column) LIKE '%match%' to column ILIKE '%Match%'. This lowers the chance of stray capital letters leading to an unexpected result.
* Prefer WHERE to HAVING when either would suffice.

## Spacing and Layout
* Make judicious use of white space and indentation to make code easier to read. Do not crowd code or remove natural language spaces.
* No tabs should be used, only spaces.
* Wrap long lines of code, between 80 and 100 characters, to a new line. Having to scroll in two dimensions makes code hard to follow.
* Always include spaces:
    - before and after equals (=)
    - after commas (,)
    - surrounding apostrophes (') where not within parentheses or with a trailing comma or semicolon.
* Always include newlines/vertical space:
    - before AND or OR
    - after semicolons to separate queries for easier reading
    - after each keyword definition
    - after a comma when separating multiple columns into logical groups
    - to separate code into related sections, which helps make large chunks of code more readable.

## Comma Positioning
* Use commas after terms, not before.
  
```sql
-- Preferred
SELECT
    Column1,
    Column2,
    Column3
    ...

-- vs

-- Not Preferred
SELECT
    Column1
    ,Column2
    ,Column3
    ...
```

## Commenting
* When making single line comments use the -- syntax
* When making multi-line comments use the /* */ syntax
* Respect the character line limit when making comments. 
* Provide brief explanatory comments for anything that cannot be easily understood by simply reading the code. For example, hard-coded variables or calculations.
* Avoid vague or redundant comments. For instance, recording the date a stored procedure was modified is doubly redundant if the source code is versioned and the modified date can be extracted from the database.
* Avoid jargon that would not be obvious to people outside the immediate project (or to your future self).
* Clean up blocks of commented-out code. 


## Naming Conventions
* All field and object names should use PascalCase (having the first letter of each word capitalised and no spaces between words) to maximise consistency with existing conventions within DataPlatform.
* An ambiguous field name such as ID, name, or type should always be prefixed by what it is identifying or naming. Use an alias if needed to enforce this.
  
```sql
-- Preferred
SELECT
    Id    AS AccountID,
    Name  AS AccountName,
    Type  AS AccountType,
    ...

-- vs

-- Not Preferred
SELECT
    Id,
    Name,
    Type,
    ...
```

* Ensure the name is unique and does not exist as a reserved keyword.
* Prefer to keep the length of the name under 30 characters.
* Prefer to use the singular form of the name where possible.
* When using a word that may have alternate spellings, be consistent. Use aliases where needed.

```sql
-- Preferred
SELECT
    SpecialityID AS SpecialtyID
    SpecialityName AS SpecialtyName
FROM Specialty

-- vs

-- Not Preferred
SELECT
    SpecialityID 
    SpecialityName 
FROM Specialty
```

* Avoid abbreviations. If they must be used, make sure they are commonly understood. 
* Never give a table the same name as one of its columns and vice versa.

## Aliasing
* Prefer to use the full object name instead of an alias when the object name is shorter, e.g., less than 20 characters.
* Aliases should relate in some way to the object or expression they are aliasing. As a rule of thumb, the abbreviated name should be the first letter of each word in the object's name.
* Use the AS operator when aliasing.
* For computed data (SUM() or AVG()) use the name you would give it were it a column defined in the schema.

## Reserved Words
* Always use uppercase for the reserved keywords like SELECT and WHERE and functions such as GETDATE() and COALESCE().
* Prefer to avoid abbreviated keywords. For example use ABSOLUTE instead of ABS.
* Prefer ANSI SQL keywords over database server specific keywords where these are equivalent.

## Reference Conventions
* When joining tables and referencing columns from both tables always qualify each column in the SELECT statement with the table name / alias for easy navigation
* Prefer explicit join statements
  
```sql
-- Preferred
    SELECT *
    FROM FirstTable
    INNER JOIN SecondTable
    ...

    -- vs

    -- Not Preferred
    SELECT *
    FROM FirstTable,
        SecondTable
    ...
```

* Prefer LEFT JOIN to LEFT OUTER JOIN
* Prefer INNER JOIN to JOIN
* Prefer FULL JOIN to FULL OUTER JOIN
* Prefer not to use RIGHT JOIN at all.


## Common Table Expressions (CTEs)
* Prefer CTEs over sub-queries as CTEs make SQL more readable.

```sql
-- Preferred
WITH ImportantList AS (

    SELECT DISTINCT
        SpecificColumn
    FROM OtherTable
    WHERE SpecificColumn != 'foo'

)

SELECT
    PrimaryTable.Column1,
    PrimaryTable.Column2
FROM PrimaryTable
INNER JOIN ImportantList
    ON PrimaryTable.Column3 = ImportantList.SpecificColumn

-- vs   

-- Not Preferred
SELECT
    PrimaryTable.Column1,
    PrimaryTable.Column2
FROM PrimaryTable
WHERE PrimaryTable.Column3 IN (
    SELECT DISTINCT SpecificColumn
    FROM OtherTable
    WHERE SpecificColumn != 'foo')
```


---

* Prefer DATEDIFF to inline additions date_column + interval_column. The function is more explicit and will work for a wider variety of date parts.

---
---


## Links and references
<a id="1">[1]</a> Holywell, Simon. 'SQL Style Guide Misconceptions'. Simon Holywell, 9 December 2016. https://www.simonholywell.com/post/2016/12/sql-style-guide-misconceptions/. 

<a id="1">[2]</a>  'SQL Style Guide by Simon Holywell'. Accessed 21 June 2024. https://www.sqlstyle.guide.  
Licensed under the Creative Commons Attribution-ShareAlike 4.0 International License. 

<a id="1">[3]</a> The GitLab Handbook. 'SQL Style Guide'. Accessed 21 June 2024. https://handbook.gitlab.com/handbook/business-technology/data-team/platform/sql-style-guide/. 
Lincenced under the Creative Commons Attribution-ShareAlike 4.0 International license.

<a id="1">[4]</a> Kristianto, Wahyu. ‘Kristories/Awesome-Guidelines’. JavaScript, 21 June 2024. https://github.com/Kristories/awesome-guidelines.


