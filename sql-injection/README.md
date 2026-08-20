# SQL Injection Research

Hands-on SQL injection research conducted in controlled, intentionally vulnerable environments using PortSwigger Web Security Academy.

The exercises cover the progression from identifying SQL injection behaviour to authentication bypass, database fingerprinting, UNION-based enumeration, and data retrieval.

## Research Areas

### Authentication & Query Manipulation

* SQL injection in a `WHERE` clause
* Authentication bypass through SQL query manipulation
* Retrieval of data hidden by application filtering

### Database Fingerprinting

* Identifying the underlying database technology
* Querying database type and version
* Understanding database-specific SQL behaviour

### UNION-Based SQL Injection

* Determining the number of columns returned by a query
* Identifying columns capable of returning text
* Using UNION-based injection to retrieve data from additional tables
* Retrieving multiple values through a single column

### Database Enumeration

* Identifying database contents
* Enumerating tables and relevant schema information
* Understanding how database structure can be inferred through application responses

## Completed Research

| Technique               | Lab                                | Level        | Status   |
| ----------------------- | ---------------------------------- | ------------ | -------- |
| WHERE-clause injection  | Retrieval of hidden data           | Apprentice   | ✅ Solved |
| Authentication bypass   | Login bypass                       | Apprentice   | ✅ Solved |
| Database fingerprinting | Oracle database version            | Practitioner | ✅ Solved |
| Database fingerprinting | MySQL / Microsoft database version | Practitioner | ✅ Solved |
| Database enumeration    | Non-Oracle databases               | Practitioner | ✅ Solved |
| UNION enumeration       | Determine number of columns        | Practitioner | ✅ Solved |
| UNION enumeration       | Find column containing text        | Practitioner | ✅ Solved |
| UNION data extraction   | Retrieve data from other tables    | Practitioner | ✅ Solved |

## Methodology

The research follows a structured vulnerability-analysis workflow:

```text
Reconnaissance
      ↓
Identify Input
      ↓
Test Query Behaviour
      ↓
Confirm SQL Injection
      ↓
Fingerprint Database
      ↓
Enumerate Query Structure
      ↓
Retrieve / Validate Data
      ↓
Assess Security Impact
      ↓
Document Remediation
```

## Key Technical Concepts

### 1. Input Analysis

Identify application parameters that influence database queries and compare application behaviour under controlled input changes.

### 2. Query Manipulation

Understand how attacker-controlled input can alter the intended SQL query logic.

### 3. Database Fingerprinting

Use observable application behaviour to determine characteristics of the underlying database technology.

### 4. UNION-Based Enumeration

Determine the structure of the original query before attempting to combine its results with attacker-controlled queries.

This includes identifying:

* Number of columns
* Compatible data types
* Columns capable of displaying text
* Additional database tables and data

### 5. Data Retrieval

Validate whether SQL injection can expose information outside the application's intended result set.

## Security Impact

Depending on the vulnerable application's architecture and database privileges, SQL injection can potentially result in:

* Authentication bypass
* Unauthorized data access
* Sensitive information disclosure
* Database enumeration
* Modification or deletion of data
* Further compromise of connected application functionality

The actual impact depends on the privileges of the database account and the application's security controls.

## Remediation

The primary defence against SQL injection is to use **parameterized queries / prepared statements**.

Additional controls include:

* Least-privilege database accounts
* Server-side input validation
* Avoiding dynamically constructed SQL
* Appropriate application access controls
* Security monitoring and logging
* Regular application security testing

Input validation should provide defence in depth rather than replace parameterized queries.

## Detailed Research

* [Authentication Bypass](./authentication-bypass.md)
* [Hidden Data Retrieval](./hidden-data-retrieval.md)
* [UNION-Based SQL Injection](./union-based-injection.md)

## Environment

All exercises were performed against intentionally vulnerable applications provided by PortSwigger Web Security Academy.

No production systems or unauthorized targets were tested.

## Skills Demonstrated

**Application Security:** SQL Injection, Authentication Testing, Data Exposure

**Database Security:** Query Manipulation, Database Fingerprinting, Enumeration, Data Retrieval

**Security Research:** Reconnaissance, Vulnerability Validation, Exploitation Analysis, Impact Assessment, Remediation

**Technical:** HTTP, SQL, Web Application Architecture
