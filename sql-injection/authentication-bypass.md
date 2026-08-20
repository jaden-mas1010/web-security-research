# SQL Injection — Authentication Bypass

## Objective

Demonstrate how an SQL injection vulnerability in an authentication mechanism can allow an attacker to bypass intended login controls.

## Vulnerability

The application's login functionality constructs a database query using user-controlled input without properly separating data from SQL syntax.

## Exploitation

I tested the authentication functionality by manipulating the application's input and observing how the resulting database query behaviour affected authentication.

The injection altered the intended query logic, allowing the authentication condition to be bypassed.

## Impact

An attacker could potentially access an account without knowing the legitimate credentials, depending on the privileges associated with the affected account.

## Root Cause

The underlying issue is unsafe handling of user-controlled input when constructing database queries.

## Remediation

Use parameterized queries/prepared statements rather than concatenating user input into SQL queries. Input validation should be treated as an additional defensive layer rather than the primary SQL injection control.

## Evidence

PortSwigger Web Security Academy lab completed as part of application security research practice.
