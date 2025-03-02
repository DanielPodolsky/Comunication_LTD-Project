# Communication_LTD Web Security Project

This README provides a step-by-step guide for implementing a secure web application for a fictional communication company called Communication_LTD. The project demonstrates common web security vulnerabilities and their mitigations.

## Project Overview

The project requires building a web-based information system for Communication_LTD, a company that sells internet browsing packages. The system will store information about customers, browsing packages, and market sectors.

## System Requirements

1. Relational Database (SQL Express or MySQL)
2. Web Application (using a framework of your choice - Python Django, Java, C#, etc.)

## Implementation Guide

### Step 1: Set Up Your Development Environment

1. **Install Required Software:**
   - Choose a code editor (Visual Studio Code, PyCharm, etc.)
   - Install a database server (MySQL or SQL Express)
   - Install your chosen web framework

2. **Create Project Structure:**
   - Set up a basic web application with your chosen framework
   - Configure database connection
   - Create a basic folder structure for your project

### Step 2: Database Design

1. **Create the following tables:**
   - Users (id, username, email, password_hash, salt)
   - Customers (id, name, email, sector, package_id)
   - Packages (id, name, description, price)
   - Sectors (id, name, description)
   - PasswordResets (id, user_id, reset_token, expiry_date)
   - PasswordHistory (id, user_id, password_hash, created_at)

### Step 3: Configuration File for Password Management

Create a configuration file (e.g., `password_config.json` or similar based on your framework) that includes:

```json
{
  "min_length": 10,
  "require_uppercase": true,
  "require_lowercase": true,
  "require_digits": true,
  "require_special_chars": true,
  "history_count": 3,
  "dictionary_words": ["password", "admin", "user", "login"],
  "max_login_attempts": 3
}
```

### Step 4: Implement Part A - Secure Development Principles

#### 4.1 User Registration Page
1. Create a form with fields for username, email, and password
2. Implement password complexity validation using the configuration file
3. Implement HMAC+Salt for password storage
4. Store the user information in the database

#### 4.2 Password Change Page
1. Create a form with fields for current password and new password
2. Validate current password
3. Enforce password complexity requirements
4. Check against password history
5. Update the password in the database

#### 4.3 Login Page
1. Create a login form with username and password fields
2. Implement authentication logic
3. Track login attempts
4. Lock account after exceeding maximum attempts
5. Display appropriate success/error messages

#### 4.4 System Page
1. Create a form to add new customers
2. Display the newly added customer on the screen
3. Implement navigation to other system features

#### 4.5 Forgot Password Page
1. Create a form to request password reset
2. Generate a random token using SHA-1
3. Send the token to the user's email (simulate this for project purposes)
4. Create a form to enter the token and set a new password

### Step 5: Implement Part B - XSS and SQLi Vulnerabilities

#### 5.1 Create a Vulnerable Version

1. **Stored XSS Vulnerability:**
   - Modify the customer addition form to be vulnerable to XSS
   - Allow direct insertion of user input into HTML without sanitization

2. **SQL Injection Vulnerabilities:**
   - Modify the registration, login, and customer addition functions to use string concatenation for SQL queries
   - Example: `"SELECT * FROM users WHERE username='" + username + "' AND password='" + password + "'"`

#### 5.2 Create a Secure Version

1. **XSS Protection:**
   - Implement proper encoding of special characters in user input
   - Use framework-specific methods for rendering data safely

2. **SQL Injection Protection:**
   - Implement parameterized queries
   - Example: `"SELECT * FROM users WHERE username=? AND password=?"`
   - Or implement stored procedures for database operations

### Step 6: Testing

1. **Test Functionality:**
   - Verify all pages work as expected
   - Test password requirements
   - Test login attempts limitation

2. **Test Vulnerabilities:**
   - Test XSS by inserting `<script>alert('XSS')</script>` in the customer name
   - Test SQL injection with inputs like `' OR '1'='1` in login fields

3. **Test Security Measures:**
   - Verify XSS protection works in the secure version
   - Verify SQL injection protection works in the secure version

## Project Submission

Prepare two versions of your project:
1. A vulnerable version demonstrating the security issues
2. A secure version with all protections implemented

Document the following for each version:
- How to set up and run the application
- How to demonstrate the vulnerabilities (in the vulnerable version)
- How the security measures work (in the secure version)

## Recommendations for Implementation

1. **For Beginners:**
   - Consider using a framework with built-in security features
   - Python Django or PHP Laravel make many security tasks easier
   - Use ORM (Object-Relational Mapping) to reduce SQL injection risks

2. **Database:**
   - Start with a simple schema and evolve as needed
   - Use migrations to track database changes

3. **Security:**
   - For HMAC+Salt, use your framework's crypto libraries
   - For SHA-1 (password reset token), use your framework's hashing functions

4. **Frontend:**
   - Start with simple HTML forms before adding complex styling
   - Focus on functionality first, then improve the user interface

## Resources

- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [OWASP Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
