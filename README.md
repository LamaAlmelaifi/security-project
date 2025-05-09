# security-project

# Building a Secure Web Application - Detection and Mitigation of Security Vulnerabilities

In this project, we built a simple web application and secured it against common vulnerabilities, applying concepts like encryption, access control, and SQL injection. By the end, we demonstrated secure coding practices and mitigated security threats effectively.

---

## Team Members' Names/Student ID
Lama Almelaifi/443200447
Remas Alharbi/443200506
Shahad Al-Hussain/443200850
Fay Alfarraj/443201111

---

## Table of Contents

- [Features](#features)
- [Web Application Setup](#Web-Application-Setup)
- [SQL Injection](#SQL-Injection)
- [Weak Password Storage](#Weak-Password-Storage)
- [Cross-Site Scripting](#Cross-Site-Scripting)
- [Access Control](#Access-Control)
- [Encryption](#Encryption)
- [Challenges and Solutions](#Challenges-and-Solutions)

---

## Features

This web application includes the following features:

- **User Registration**: Users can create an account by providing a username, password, and selecting a role (either "user" or "admin").
- **User Authentication**: Secure login system allowing users to log in with their credentials.
- **Role-based Access Control**: Admin pages are restricted to users with the admin role only. Regular users are redirected if they try to access protected admin areas.
- **Dashboard**: Users are directed to a dashboard page after login, which may include a form (e.g., for submitting comments).
- **Admin Panel**: Admin users can view a list of all registered users.
- **Session Management**: User sessions are securely managed using cookies, with protection against JavaScript access and insecure transmission.


## Web Application Setup

We developed the application using the **Flask framework** for Python due to its simplicity, flexibility, and widespread use in educational and real-world scenarios. Flask allowed us to quickly set up routes, handle form inputs, manage sessions, and integrate database operations.

The application is divided into two versions:

- **Vulnerable Version**: This version contains intentional security flaws to demonstrate various attack vectors.
- **Secure Version**: This version includes all security improvements, applying best practices and protections.

---
## SQL Injection

*Vulnerable website* :
    user input was directly embedded in SQL queries without proper sanitization or parameterization. Specifically, in the login functionality.

*Secure website* :
    To mitigate this vulnerability, we switched to parameterized queries (prepared statements) where user input is treated as data, not code.  making it impossible to inject arbitrary SQL commands.

*Testing* :
        1- Go to the login page (/login).
        2- In the username field, enter: ' OR 1=1 --.
        3- Enter anything in the password field.
        
  Expected results:

  - Vulnerable: The login will be successful even though the credentials are incorrect because the SQL query is vulnerable to injection. This will give attackers unauthorized access.

  - Secure: The login attempt should fail with an error message like "Invalid username or password." This is because the query now uses parameterized statements, preventing SQL injection.

## Weak Password Storage

*Vulnerable Website* :
    The passwords were stored using MD5, which is a fast, unsalted hashing algorithm. MD5 is vulnerable to rainbow table and brute-force attacks, which allows attackers to easily crack passwords.

*Secure Website* :
    To mitigate this, we used bcrypt, a slow, salted hashing algorithm. bcrypt automatically adds a unique salt to each password, making it much more difficult to crack via brute-force or precomputed attacks.

*Testing*:
    1- Go to the registration page (/register).
    2- Register a new account with a simple password.
    3- Use a password cracking tool like hashcat or John the Ripper to attempt to crack the hashed password stored in the database.

  Expected Results:

  - Vulnerable: The MD5 hash can be cracked quickly, allowing attackers to reveal the original password.

  - Secure: bcrypt hashes should take a significant amount of time to crack, making password cracking infeasible.


## Cross-Site Scripting

*Vulnerable Website*:
    User input was directly reflected on the page (e.g., in comments or other user-generated content) without sanitization, allowing attackers to inject malicious JavaScript into the page.

*Secure Website*:
    To mitigate this, user input is properly sanitized and escaped before being displayed. This ensures that any HTML or JavaScript code entered by the user is rendered as plain text rather than executable code.

*Testing*:
    1- Go to the dashboard page (/dashboard).
    2- In the comment form, enter the following:
        <script>alert('XSS Attack!')</script>
    3- Submit the comment.

  Expected Results:

  - Vulnerable: An alert box should pop up, indicating that JavaScript is executed in the browser, demonstrating the XSS vulnerability.

  - Secure: The comment should display as plain text (i.e., the script tags should be shown as text and not executed), with no alert box appearing.

## Access Control

*Vulnerable Website*:
    The application did not properly restrict access to certain pages based on user roles. For example, regular users could access admin pages without any role validation.

*Secure Website*:
    We implemented proper role-based access control (RBAC) by checking the user's role before granting access to sensitive pages, such as the admin panel.

*Testing*:
    1- Log in as a regular user.
    2- Try to access the admin page (/admin) using the URL since the button is invisible by default for users.

  Expected Results:

  - Vulnerable: You will be able to access the admin page even though you are not an administrator.

  - Secure: You should be redirected to the dashboard or see a message indicating insufficient permissions. Only users with the admin role will have access to the admin page.

## Encryption

*Vulnerable Website*:
    The session cookie was not properly secured. It may have been set without the HttpOnly or Secure flags, meaning the cookie could be accessed by JavaScript or transmitted over unencrypted HTTP.

*Secure Website*:
    To mitigate this, we set the session cookie with the HttpOnly and Secure flags. The HttpOnly flag ensures the cookie is inaccessible to JavaScript, and the Secure flag ensures the cookie is only transmitted over HTTPS.

  For testing purposes, we created a self-signed certificate to enable HTTPS locally in the secure version of the application.

*Testing*:
    1- Open the browser's developer tools.
    2- Go to the "Application" tab and inspect the cookies.
    3- Look for the session cookie.

  Expected Results:

  - Vulnerable: The session cookie is visible to JavaScript and may be transmitted over unencrypted HTTP.

  - Secure: The session cookie is marked as HttpOnly and Secure, and it will only be transmitted over HTTPS, making it secure against interception and XSS attacks.


---

## Challenges and Solutions

During this project, we faced several technical challenges while building and securing the web application:

  1. Initial Setup & Testing Environment: Setting up a secure local environment was one of the hardest parts. Testing features like HTTPS required creating our own self-signed SSL certificates, which was new to us. Configuring Flask to use these certificates correctly and dealing with browser warnings added complexity.

  2. Implementing Secure Authentication: Switching from MD5 to bcrypt introduced issues with password encoding and verification. We had to carefully adapt our registration and login logic to handle bcrypt’s hashing format.

  3. Role-Based Access Control: Ensuring that only admins could access certain routes was tricky. We needed to enforce role checks both in the backend and in the user interface, which required testing different user scenarios.

  4. Simulating Attacks: Testing vulnerabilities like SQL injection and XSS required research and experimentation to craft the right payloads. It was challenging to confirm that the attacks worked on the vulnerable version and were properly blocked in the secure version.

Overall, the project taught us a lot about both building functional web applications and protecting them from real-world threats.
