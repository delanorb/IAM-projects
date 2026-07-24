Enterprise Identity and Access Management (IAM) User Provisioning Workflow

Project Overview

In this hands-on project, I simulated common enterprise Identity and Access Management (IAM) workflows, including employee onboarding, access provisioning, group assignment, Multi-Factor Authentication (MFA) configuration, password policy enforcement, internal transfers, and employee offboarding.

The environment used a simulated identity directory rather than a production Active Directory environment or commercial IAM platform. The purpose of this project was to demonstrate the underlying identity lifecycle and access management processes commonly used in enterprise environments.

This project focused on the employee onboarding and account provisioning process. The objective was to create a new user account, assign appropriate group memberships based on the user’s role, enforce MFA, require a password change at first login, and verify that the organization’s password policy applied to the account.

⸻

1. Creating the User Account

The first step in the onboarding process was to create a new user account for the employee, Jullian Morgan.

I located the user management section of the simulated identity directory and created a new account using the employee’s identifying information. Creating the account represents the initial stage of the identity lifecycle, where a digital identity is established for a new employee.

During this process, I configured the account with the appropriate user information and verified that the account was created successfully.

[Insert Screenshot: User account creation screen]

Creating the account establishes the user’s identity within the organization’s directory and provides the foundation for assigning access and security controls.

⸻

2. Assigning Group Memberships

After creating the account, I assigned Jullian Morgan to the appropriate security and organizational groups.

For this scenario, the account was assigned to:

* IT Staff
* IT Helpdesk

Group-based access management is commonly used in enterprise IAM environments to simplify access provisioning. Rather than assigning individual permissions to every user, administrators can assign users to groups that already have the appropriate permissions associated with them.

In this scenario, assigning the user to the IT Staff and IT Helpdesk groups represents the user’s job responsibilities and provides access based on their organizational role.

[Insert Screenshot: Group membership assignment]

This approach follows the principle of role-based access control (RBAC). Access is granted based on the user’s role and responsibilities rather than assigning permissions individually. This can improve consistency, simplify administration, and reduce the risk of excessive or inappropriate access.

⸻

3. Configuring Multi-Factor Authentication (MFA)

After the user was created and assigned to the appropriate groups, I configured the account to require Multi-Factor Authentication (MFA).

MFA adds an additional layer of security by requiring users to provide more than just a password when authenticating. In a real-world enterprise environment, this could involve an authentication application, hardware security key, text message, or another approved authentication method.

Requiring MFA helps protect accounts in the event that a user’s password is compromised. An attacker who obtains the password would still need to satisfy the additional authentication requirement.

[Insert Screenshot: MFA requirement configuration]

For this scenario, MFA was enabled as a required security control for the account. This demonstrates how administrators can enforce stronger authentication requirements for users and reduce the risk associated with compromised credentials.

⸻

4. Requiring a Password Change at First Login

As part of the account setup process, I configured the account to require the user to change their password at the next login.

This is a common enterprise onboarding practice. An administrator may initially create an account using a temporary password or temporary credential. Requiring the user to change the password ensures that the administrator does not retain knowledge of the user’s permanent password.

[Insert Screenshot: Require password change at next login]

This process also allows the user to establish their own credentials while maintaining control over their account security.

⸻

5. Verifying Password Policy Enforcement

The next step was to verify that the organization’s password policy was applied to the account.

The password policy was reviewed to confirm that the required security settings were enforced. These requirements may include:

* Minimum password length
* Password complexity requirements
* Uppercase characters
* Lowercase characters
* Numbers
* Special characters
* Password expiration requirements
* Password history requirements
* Account lockout settings

[Insert Screenshot: Password policy configuration]

Verifying the password policy is important because password requirements are a fundamental part of identity security. A strong password policy helps reduce the risk of weak or easily guessed credentials.

However, password complexity alone does not guarantee a secure account. A longer, unique password combined with MFA provides significantly stronger protection than relying solely on complexity requirements.

⸻

6. Summary of the Onboarding Workflow

The completed onboarding workflow followed these steps:

1. Created the user account for Jullian Morgan.
2. Assigned the user to the appropriate IT-related groups.
3. Enabled MFA requirements for the account.
4. Required the user to change their password during the next login.
5. Verified that the organization’s password policy applied to the account.

This workflow demonstrates how IAM administrators can manage the initial stages of the identity lifecycle while applying security controls during the account creation process.

The combination of centralized account management, group-based access control, MFA, and password policy enforcement helps organizations establish a more consistent and secure onboarding process.
