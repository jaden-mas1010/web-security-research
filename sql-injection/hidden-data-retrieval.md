# SQL Injection — Retrieval of Hidden Data

**Platform:** PortSwigger Web Security Academy
**Category:** SQL Injection
**Lab type:** Retrieving hidden application data
**Purpose:** Application security research and exploitation practice

## Overview

This lab demonstrates how SQL injection can allow an attacker to manipulate an application's database query and retrieve information that the application did not intend to expose.

The objective was to analyse a filtering function, identify whether user-controlled input influenced the underlying SQL query, and validate whether the query could be manipulated to return hidden data.

## Attack Surface

The vulnerable functionality was an application filtering mechanism that accepted user-controlled input through an HTTP request.

The relevant attack surface consisted of:

* Product/category filtering functionality
* User-controlled request parameters
* Backend query construction
* Database query execution
* Data returned by the application

The key trust boundary was the transition from attacker-controlled HTTP input to the backend database query.

## Reconnaissance & Detection

I first examined the application's normal filtering behaviour and established what information was returned for legitimate input.

I then modified the relevant request parameters and compared the application's responses.

The changes in returned content indicated that the supplied input was influencing the underlying database query rather than being handled purely as application data.

This provided an indication that the filtering functionality could be vulnerable to SQL injection.

## Exploitation Methodology

The vulnerability occurred because user-controlled input was incorporated into the database query without sufficient separation between SQL syntax and application data.

I manipulated the filtering input to alter the logical conditions evaluated by the database.

The modified query behaviour caused the application to return records that were outside the application's intended filtering criteria.

The result demonstrated that an attacker could influence the database query and access information that was not normally exposed through the application's legitimate filtering behaviour.

## Technical Analysis

### Trust Boundary

The relevant data flow was:

**User input → HTTP request → Application filter → SQL query → Database → HTTP response**

The security failure occurred when untrusted input crossed into the SQL query layer without appropriate parameterization.

### Security Failure

The application allowed user-controlled input to influence the structure or logical conditions of the SQL statement.

This meant that the database could be instructed to evaluate conditions beyond those intended by the application's filtering functionality.

### Exploitation Result

After manipulating the input, the application's response contained information that should have remained outside the intended result set.

This confirmed that the filtering mechanism could be influenced through SQL injection.

## Impact

Successful SQL injection can potentially allow attackers to:

* Retrieve unauthorized application data
* Bypass intended filtering conditions
* Access records outside their authorized scope
* Discover sensitive information stored in the database
* Potentially escalate the attack depending on database privileges and application architecture

The severity of a real-world vulnerability would depend on the type and sensitivity of the exposed information.

## Root Cause

The root cause is unsafe handling of user-controlled input when constructing or executing SQL queries.

When applications dynamically construct queries using untrusted input, attackers may be able to manipulate the intended query logic.

## Remediation

The primary remediation is to use **parameterized queries / prepared statements** so that user input is always treated as data.

Additional controls should include:

* Least-privilege database accounts
* Appropriate access controls
* Server-side input validation
* Avoiding dynamic SQL construction
* Monitoring suspicious database query behaviour
* Security testing of application input points
* Logging anomalous requests and responses

Parameterization should remain the primary defence against SQL injection.

## Security Research Takeaways

This exercise reinforced several practical application security concepts:

1. Filtering functionality can represent a significant SQL injection attack surface.
2. Response differences can provide useful evidence during vulnerability identification.
3. SQL injection can affect both authentication and data confidentiality.
4. Vulnerability validation should establish actual security impact rather than simply identifying suspicious input.
5. Understanding the application's data flow makes it easier to identify where untrusted input crosses into security-sensitive operations.

## Evidence

This write-up is based on a controlled, intentionally vulnerable environment provided by **PortSwigger Web Security Academy**.

No real-world systems or applications were targeted.

## Skills Demonstrated

**Application Security:** SQL Injection, Input Analysis, Data Exposure
**Technical:** HTTP, SQL, Web Application Architecture
**Security Methodology:** Reconnaissance, Vulnerability Identification, Exploitation, Impact Assessment, Remediation
**Reporting:** Technical vulnerability documentation and security analysis
