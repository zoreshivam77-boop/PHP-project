# PHP Signup Form with MariaDB/MySQL on AWS EC2

A beginner PHP project that collects signup details through an HTML form, sends them to a PHP script, stores them in a MariaDB/MySQL database, and displays a success message with the submitted information.

## Project overview

**Technologies used**
- HTML — creates the signup form.
- PHP — receives form data and handles the database operation.
- MariaDB/MySQL — stores the submitted records.
- Nginx — serves the project on the EC2 instance.
- AWS EC2 — hosts the application.

## Files

- `signup.html` — signup form with Name, Email, Website, Comment, and Gender fields.
- `submit.php` — reads the submitted values, connects to the database `FCT`, inserts a record into the `users` table, and displays the result.

## How it works

1. A visitor opens `signup.html`.
2. The visitor enters the form details and clicks **Submit**.
3. The form sends the values to `submit.php` using the HTTP `POST` method.
4. PHP reads the submitted values using `$_POST`.
5. PHP connects to the database named `FCT`.
6. The script inserts the values into the `users` table.
7. If the query succeeds, the page displays **“New record created successfully!”** and the submitted details.

## Database setup

The PHP file expects a database named `FCT` and a table named `users` with these columns:

| Column | Suggested type |
|---|---|
| `ID` | `INT` (primary key, auto-increment) |
| `name` | `VARCHAR(100)` |
| `email` | `VARCHAR(255)` |
| `website` | `VARCHAR(255)` |
| `comment` | `TEXT` |
| `gender` | `VARCHAR(20)` |

Example SQL for a new database/table:

```sql
CREATE DATABASE FCT;
USE FCT;

CREATE TABLE users (
    ID INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) NOT NULL,
    website VARCHAR(255),
    comment TEXT,
    gender VARCHAR(20) NOT NULL
);
```

If the database or table already exists, inspect its structure first rather than creating it again.

## Configuration

In `submit.php`, the database connection currently uses these settings:

```php
$servername = "localhost";
$username = "root";
$password = "root";
$dbname = "FCT";
```

These are the values in the provided project file. They must match the actual database configuration on the server. On an EC2 deployment, prefer a dedicated database user with only the permissions the application needs instead of using the database `root` account. Do not commit real passwords or secrets to a public GitHub repository.

## Deploying on AWS EC2 (overview)

1. Launch an EC2 instance and install/configure Nginx, PHP with the MySQLi extension, and MariaDB/MySQL.
2. Start the required services.
3. Place or clone the project into the Nginx web root (for example, a project subfolder under `/usr/share/nginx/html/` on Amazon Linux).
4. Create the database and table, and configure valid database credentials.
5. Make sure the EC2 security group allows inbound HTTP traffic on TCP port `80`.
6. Open the form in a browser, for example:

   `http://YOUR-EC2-PUBLIC-IP/signup.html`

   If the files are inside a subfolder, include that folder in the URL.

7. Submit test data and verify that a row is added to the `users` table.

The exact paths and commands depend on the operating system and web-server configuration.

## Testing

- Open the form and confirm all fields are visible.
- Submit valid test details.
- Confirm the success message appears.
- Query the database to confirm the row was inserted:

```sql
USE FCT;
SELECT * FROM users;
```

## Important security notes

This is a learning project, not production-ready code.

- The supplied PHP builds an SQL query by inserting form values directly into the query string. This can allow SQL injection. Use a **prepared statement** with bound parameters before using the project for real users.
- The supplied file displays PHP errors and database error details. Disable public error display in production and log errors securely instead.
- The supplied file uses `htmlspecialchars()` when displaying submitted values, which helps prevent HTML injection in that output.
- Use a dedicated database account and keep credentials outside public source code.
- Use HTTPS for a public deployment and validate incoming form data on the server.

## Current project result

The deployment screenshots show the signup form loading in the browser, a successful submission message, and the submitted record appearing in the `FCT.users` table. This confirms the basic HTML → PHP → MariaDB/MySQL flow worked in that test.

## Learning outcomes

By completing this project, you practise:

- Building an HTML form.
- Sending data with `POST`.
- Reading form values in PHP using `$_POST`.
- Connecting PHP to MariaDB/MySQL with `mysqli_connect()`.
- Inserting records with SQL.
- Displaying query results and handling basic errors.
- Deploying a web application on an AWS EC2 instance.
