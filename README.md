# Communication_LTD Web Security Project

This README provides a step-by-step guide for implementing a secure web application for a fictional communication company called Communication_LTD. The project demonstrates common web security vulnerabilities and their mitigations.

## Project Overview

The project requires building a web-based information system for Communication_LTD, a company that sells internet browsing packages. The system will store information about customers, browsing packages, and market sectors.

## System Requirements

1. MySQL Database
2. JavaScript-based Web Application (with Node.js and Express)

## Implementation Guide

### Step 1: Set Up Your Development Environment

1. **Install Required Software:**
   - MySQL Server (for database)
   - Node.js and npm (for JavaScript backend)
   - Visual Studio Code (recommended code editor)

2. **Initialize Your Project:**
   - Create a new folder for your project
   - Set up a basic Node.js/Express application
   - Install necessary packages (express, mysql2, bcrypt, etc.)

3. **Create a Basic Folder Structure:**
   - `/config` - Configuration files
   - `/public` - Static files (CSS, client-side JS)
   - `/views` - HTML templates
   - `/routes` - Route handlers
   - `/models` - Database interaction

### Step 2: Set Up MySQL Database

1. **Create Database:**
   - Create a new database named "communication_ltd"

2. **Create Tables:**
   - Users table (id, username, email, password_hash, salt, login_attempts, is_locked)
   - Customers table (id, name, email, sector_id, package_id)
   - Packages table (id, name, description, price)
   - Sectors table (id, name, description)
   - PasswordResets table (id, user_id, reset_token, expiry_date)
   - PasswordHistory table (id, user_id, password_hash, created_at)

3. **Add Example Data:**
   - Add a few sample packages and sectors

### Step 3: Password Configuration

1. **Create a configuration file** in your project to store password requirements:
   - Minimum length (10 characters)
   - Required character types (uppercase, lowercase, numbers, special characters)
   - History count (3 previous passwords)
   - Dictionary words to block
   - Maximum login attempts (3)

### Step 4: Implement Part A - Secure Development Principles

#### 4.1 User Registration

1. **Create a registration form page** with:
   - Username field
   - Email field
   - Password field
   - Submit button

2. **Implement registration logic:**
   - Validate the password against requirements from config file
   - Generate a random salt for the user
   - Create an HMAC hash of the password using the salt
   - Store username, email, password hash, and salt in the database
   - Store the password hash in password history table
   - Redirect to login page

#### 4.2 Password Change

1. **Create a password change form page** with:
   - Current password field
   - New password field
   - Submit button

2. **Implement password change logic:**
   - Verify current password matches stored hash
   - Validate new password against requirements
   - Check if new password matches any in password history
   - Create new salt and hash for the new password
   - Update password in database
   - Add new password to password history table
   - Display success message

#### 4.3 Login System

1. **Create a login form page** with:
   - Username field
   - Password field
   - Submit button
   - Links to registration and forgot password

2. **Implement login logic:**
   - Check if username exists in database
   - Check if account is locked
   - Hash the entered password and compare with stored hash
   - If match fails, increment login attempts
   - If login attempts exceed maximum, lock the account
   - If match succeeds, create a session for the user
   - Reset login attempts on successful login
   - Redirect to dashboard

#### 4.4 Customer Management System

1. **Create a customer form page** with:
   - Name field
   - Email field
   - Dropdown for sector
   - Dropdown for package
   - Submit button

2. **Implement customer addition logic:**
   - Save customer details to database
   - Display the newly added customer information on confirmation page
   - Include navigation back to dashboard

#### 4.5 Forgot Password System

1. **Create a forgot password form page** with:
   - Username or email field
   - Submit button

2. **Implement token generation:**
   - Generate a random value
   - Hash it with SHA-1
   - Store the token in database with expiration time
   - Display message saying token was sent (in a real app, you would email it)

3. **Create a reset password page** with:
   - Token field
   - New password field
   - Submit button

4. **Implement password reset logic:**
   - Verify token exists and hasn't expired
   - Follow the same validation as password change
   - Update the user's password
   - Redirect to login page

### Step 5: Implement Part B - Demonstrating and Fixing Vulnerabilities

#### 5.1 Vulnerable Version

1. **Create a version with Stored XSS vulnerability:**
   - In the customer management system, directly insert user input into HTML without sanitization
   - Example attack: Adding a customer with name containing `<script>alert('XSS')</script>`

2. **Create a version with SQL Injection vulnerability:**
   - In registration, login, and customer forms, construct SQL queries by concatenating strings
   - Example attack: Using `' OR '1'='1` in the username field of login form

#### 5.2 Secure Version

1. **Fix XSS vulnerability:**
   - Sanitize user input by encoding special characters
   - Use built-in template engine features for secure rendering

2. **Fix SQL Injection vulnerability:**
   - Replace string concatenation with parameterized queries
   - Use placeholder values in all database queries

### Step 6: Testing Your Application

1. **Test User Registration:**
   - Try creating a user with various passwords to test complexity rules
   - Verify that passwords are properly hashed in database

2. **Test Login System:**
   - Verify that successful login creates a session
   - Verify that failed login attempts are tracked
   - Verify that account locks after maximum attempts

3. **Test Customer Management:**
   - Add customers and verify they appear in database
   - Verify that customer information is displayed correctly

4. **Test Vulnerability Demonstrations:**
   - Test XSS attack in vulnerable version (should work)
   - Test XSS attack in secure version (should fail)
   - Test SQL injection in vulnerable version (should work)
   - Test SQL injection in secure version (should fail)

## Tips for Implementation

1. **For the Database:**
   - Create all tables before starting development
   - Test queries in MySQL Workbench before implementing in code

2. **For Security Features:**
   - Keep the salt unique for each user
   - Use crypto libraries for HMAC and SHA-1 functions
   - Never store raw passwords anywhere

3. **For Express.js Setup:**
   - Use express-session for session management
   - Use body-parser to handle form data
   - Use a template engine like EJS or Handlebars

4. **For Vulnerability Demonstrations:**
   - Keep vulnerable and secure code in separate branches or folders
   - Document each vulnerability and how to exploit it

## Resources

- [MySQL Documentation](https://dev.mysql.com/doc/)
- [Node.js Documentation](https://nodejs.org/en/docs/)
- [Express.js Documentation](https://expressjs.com/)
- [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)
- [OWASP SQL Injection Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
