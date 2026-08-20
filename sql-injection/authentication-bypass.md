# SQL Injection — Authentication Bypass

**Platform:** PortSwigger Web Security Academy
**Category:** SQL Injection
**Lab type:** Authentication bypass
**Purpose:** Application security research and exploitation practice

## Overview

This lab demonstrates how SQL injection can affect an application's authentication mechanism when user-controlled input is incorporated into a database query without being safely parameterized.

The objective was to analyse the authentication flow, identify the SQL injection behaviour, understand how the application's query logic could be manipulated, and demonstrate the resulting authentication bypass.

## Attack Surface

The vulnerable functionality was the application's login form, where user-controlled credentials were submitted to the server and used during authentication.

The relevant attack surface consisted of:

* Username input
* Password input
* HTTP request handling
* Backend authentication logic
* Database query execution

The security boundary of interest was the point where untrusted HTTP input was passed into the application's database query.

## Reconnaissance & Detection

I first examined the normal login behaviour and observed how the application responded to valid and invalid authentication attempts.

I then tested whether the supplied input was being handled strictly as data or whether it could influence the underlying SQL query.

Changes in the application's authentication behaviour following controlled input manipulation indicated that the supplied input was affecting the database query logic.

This provided evidence of a potential SQL injection vulnerability rather than simply an incorrect login attempt.

## Exploitation Methodology

The vulnerability resulted from the application's failure to properly separate user-controlled input from SQL syntax.

By manipulating the input supplied to the authentication mechanism, the intended query logic could be altered.

Instead of the database evaluating the credentials only as values, the application's query structure could be influenced by attacker-controlled input.

The resulting change in query behaviour allowed the authentication condition to be bypassed within the lab environment.

I validated the result by observing that the application granted authenticated access despite not providing the expected legitimate credentials.

## Technical Analysis

### Trust Boundary

The attack crosses the following trust boundary:

**User input → HTTP request → Application authentication logic → SQL query → Database**

The vulnerability exists when untrusted input is incorporated into the SQL statement without appropriate parameterization.

### Security Failure

The fundamental security failure is treating attacker-controlled input as executable SQL syntax rather than strictly as data.

This allows an attacker to manipulate the logical conditions evaluated by the database.

### Exploitation Result

The manipulated input changed the behaviour of the authentication query and caused the application to accept the authentication attempt when it should have rejected it.

This demonstrates how SQL injection can undermine authentication controls at the application layer.

## Impact

A SQL injection vulnerability in an authentication mechanism can potentially allow an attacker to:

* Bypass authentication
* Access another user's account
* Circumvent application access controls
* Potentially access functionality or information available to the compromised account

The actual impact in a production environment would depend on the privileges associated with the affected account and the application's database permissions.

## Root Cause

The underlying vulnerability is unsafe construction of SQL queries using untrusted user input.

Common examples of this insecure pattern include dynamically constructing SQL statements through string concatenation or interpolation.

The application should instead ensure that user input is treated as data and cannot modify the structure of the SQL statement.

## Remediation

The primary remediation is to use **parameterized queries / prepared statements** for database operations.

Additional defensive measures include:

* Applying least-privilege permissions to database accounts
* Validating input according to expected application behaviour
* Avoiding dynamic SQL construction where possible
* Implementing secure authentication controls
* Monitoring authentication endpoints for abnormal input patterns
* Logging and alerting on repeated SQL injection indicators

Input validation should be considered an additional security layer rather than a replacement for parameterized queries.

## Security Research Takeaways

This exercise reinforced several practical application security concepts:

1. Authentication endpoints are high-value attack surfaces.
2. Unexpected changes in application behaviour can provide evidence of injection vulnerabilities.
3. SQL injection is fundamentally a failure to separate code from data.
4. Exploitation should be followed by impact assessment rather than stopping at vulnerability identification.
5. Clear documentation of attack path, impact, root cause and remediation is essential when reporting security findings.

## Evidence

This write-up is based on a controlled, intentionally vulnerable environment provided by **PortSwigger Web Security Academy**.

No real-world systems or applications were targeted.

## Skills Demonstrated

**Application Security:** SQL Injection, Authentication Testing, Input Analysis
**Technical:** HTTP, SQL, Web Application Architecture
**Security Methodology:** Reconnaissance, Vulnerability Identification, Exploitation, Impact Assessment, Remediation
**Reporting:** Technical documentation and security finding analysis
