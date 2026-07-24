# Joiner-Mover-Leaver (JML) Lifecycle Lab

## Objective

This lab demonstrates the identity lifecycle of an employee through three
stages:

1. Joiner - onboarding a new employee
2. Mover - changing an employee's department or role
3. Leaver - removing access when an employee leaves the organization

## Scenario

A simulated employee named Jane Doe joins the organization as a Financial
Analyst.

### Initial Identity

- Employee: Jane Doe
- Employee ID: 1001
- Department: Finance
- Job Title: Financial Analyst
- Status: Active

## Joiner Process

The following actions are performed:

1. Create the user identity
2. Assign the Finance department
3. Assign the Financial Analyst role
4. Add the appropriate group memberships
5. Provision required access
6. Record the activity in an audit log

## Mover Process

Jane Doe transfers from Finance to IT.

The following actions are performed:

1. Update department
2. Remove Finance access
3. Remove outdated group memberships
4. Assign the new IT role
5. Provision new IT access
6. Verify that unnecessary access was removed

## Leaver Process

Jane Doe leaves the organization.

The following actions are performed:

1. Disable the account
2. Remove group memberships
3. Revoke access
4. Preserve audit records
5. Confirm that the identity can no longer access resources

## Security Principles Demonstrated

- Least privilege
- Identity lifecycle management
- Role-Based Access Control (RBAC)
- Access revocation
- Account deprovisioning
- Auditability
