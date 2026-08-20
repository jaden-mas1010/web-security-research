# SQL Injection — Retrieval of Hidden Data

## Objective

Demonstrate how SQL injection in an application's filtering functionality can be used to alter the intended database query and retrieve data that should not be exposed.

## Vulnerability

The application incorporates user-controlled input into a database query without safely parameterizing the input.

## Exploitation

I analysed the application's filtering behaviour and manipulated the relevant input to modify the query logic.

This caused the application to return data outside the intended filtering conditions, demonstrating that the database query could be influenced by attacker-controlled input.

## Impact

An attacker could potentially access information that the application intended to keep hidden or restricted.

The impact depends on the privileges of the database account and the data accessible through the vulnerable query.

## Root Cause

The application does not safely separate SQL commands from user-supplied data.

## Remediation

Use parameterized queries/prepared statements and apply appropriate database privileges. Avoid constructing SQL statements through direct concatenation of user input.

## Evidence

PortSwigger Web Security Academy lab completed as part of application security research practice.
