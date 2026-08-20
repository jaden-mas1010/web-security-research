# UNION-Based SQL Injection — Enumeration & Data Retrieval

**Platform:** PortSwigger Web Security Academy
**Category:** SQL Injection — UNION attacks
**Lab level:** Practitioner
**Research type:** Controlled application security testing

## Overview

This research examines **UNION-based SQL injection**, focusing on how an attacker can determine the structure of an application's database query and use that information to retrieve data from additional database tables.

The research covered three stages:

1. Determining the number of columns returned by the original query
2. Identifying which columns can return text
3. Using the identified structure to retrieve data from another table

The exercises were performed against intentionally vulnerable applications in PortSwigger Web Security Academy.

## Attack Surface

The attack surface was an application parameter used to filter or retrieve database-backed content.

The relevant data flow was:

```text
User-controlled input
        ↓
HTTP request
        ↓
Application query
        ↓
Database
        ↓
Application response
```

The key security issue was the application's failure to safely separate attacker-controlled input from the SQL query being executed.

---

# 1. Determining the Number of Columns

## Objective

The first stage was to determine how many columns were being returned by the application's original SQL query.

This information is important because a successful `UNION SELECT` requires the injected query to return a compatible number of columns with the original query.

## Methodology

I analysed the application's response while systematically testing different query structures.

The objective was to identify a condition where the application accepted the modified query without producing a database error.

The response behaviour provided evidence about the structure of the underlying query.

## Technical Concept

A UNION operation requires the combined queries to have compatible result structures.

Conceptually:

```text
Original query:
SELECT A, B, C FROM products

UNION query:
SELECT X, Y, Z FROM users
```

Both queries must return the same number of columns.

Determining this structure therefore provides an important prerequisite for further UNION-based exploitation.

## Result

The correct column count was identified, allowing the research to progress to the next stage.

---

# 2. Identifying a Column Capable of Returning Text

## Objective

After determining the number of columns, the next objective was to identify which returned column could safely display textual data.

## Methodology

I tested the identified column positions with controlled text values and observed how the application handled the resulting responses.

This allowed the response behaviour to be compared across different column positions.

## Why This Matters

Not every column necessarily accepts or displays arbitrary text.

For example, a database query may conceptually return:

```text
Integer | Text | Decimal
```

Injecting text into an incompatible position may produce an error or fail to produce useful output.

Identifying a text-compatible position makes it possible to use the application's response as a channel for retrieving database information.

## Result

A column capable of displaying text was identified.

This provided the required output location for the subsequent data-retrieval stage.

---

# 3. Retrieving Data from Another Table

## Objective

The final stage was to demonstrate that the UNION injection could be used to retrieve information from a different database table.

## Methodology

With the query structure and text-compatible column established, I used the UNION-based technique to combine the application's original query with a controlled query targeting another table.

The application's response was then analysed to determine whether information from the additional table was exposed.

## Security Significance

This demonstrates the progression from:

```text
SQL Injection
      ↓
Query Structure Discovery
      ↓
Column Enumeration
      ↓
Output Identification
      ↓
Additional Table Access
      ↓
Data Exposure
```

The vulnerability therefore represents more than simple query manipulation. Where database permissions allow it, an attacker may be able to access information outside the application's intended data scope.

---

# Technical Analysis

## Trust Boundary

The relevant trust boundary is:

**HTTP input → Application → SQL query → Database → Application response**

The vulnerability occurs when attacker-controlled input is incorporated into the SQL query without appropriate parameterization.

## Exploitation Dependencies

Successful UNION-based SQL injection depends on several factors:

* The application must be vulnerable to SQL injection.
* The injected query must be syntactically compatible.
* The UNION query must return the appropriate number of columns.
* Compatible data types must be used.
* The application must expose some portion of the database response.
* The database account must have sufficient permissions to access the targeted information.

## Information Discovery

The research demonstrates that exploitation can involve progressively discovering information about the database rather than immediately attempting to retrieve sensitive data.

The process can involve:

1. Identifying injectable input
2. Determining query structure
3. Identifying compatible output columns
4. Identifying database structures
5. Retrieving accessible information

This makes database enumeration an important part of application security testing.

# Impact

Depending on the application's database privileges and accessible information, UNION-based SQL injection can potentially result in:

* Unauthorized data disclosure
* Exposure of user information
* Exposure of application credentials or secrets
* Database structure discovery
* Access to information belonging to other application functions
* Further exploitation of the application or connected systems

The severity of a real-world vulnerability would depend on the sensitivity of the accessible data and the privileges of the database account.

# Root Cause

The underlying issue is unsafe construction of SQL queries using untrusted user input.

Applications become vulnerable when user input can influence SQL syntax or query structure rather than being treated strictly as data.

# Remediation

The primary remediation is to use **parameterized queries / prepared statements**.

Additional defensive controls include:

* Apply least-privilege database permissions
* Avoid dynamic SQL construction where possible
* Validate input according to expected application behaviour
* Restrict database account permissions
* Monitor suspicious application requests
* Log anomalous database-related activity
* Perform regular application security testing

Parameterized queries should remain the primary control because they prevent user input from being interpreted as SQL syntax.

# Research Takeaways

This research demonstrated the practical progression of UNION-based SQL injection:

**Identify → Enumerate → Validate → Retrieve → Assess Impact**

The key lesson is that successful SQL injection exploitation often depends on understanding the application's underlying query structure rather than simply identifying an injectable parameter.

The exercises also reinforced the importance of validating actual security impact and documenting the conditions required for exploitation.

# Related Research

* [Authentication Bypass](./authentication-bypass.md)
* [Hidden Data Retrieval](./hidden-data-retrieval.md)

# Evidence & Environment

These exercises were performed in intentionally vulnerable environments provided by **PortSwigger Web Security Academy**.

No production systems or unauthorized targets were tested.

The write-up documents the security concepts and methodology demonstrated during the controlled exercises rather than reproducing the lab solutions or payloads.

# Skills Demonstrated

**Application Security**

* SQL Injection
* UNION-based SQL injection
* Query manipulation
* Data exposure analysis

**Database Security**

* Query structure analysis
* Column enumeration
* Database information retrieval
* Access-control considerations

**Security Research**

* Vulnerability validation
* Exploitation methodology
* Impact assessment
* Remediation analysis

**Technical**

* HTTP
* SQL
* Web application architecture
* Database-backed application analysis
