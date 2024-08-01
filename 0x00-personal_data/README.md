# Personal Data Protection Project

This project includes tasks to learn how to protect a user's personal data.

## Tasks Overview

### 0. Regex-ing
File: [filtered_logger.py](filtered_logger.py)

Contains a function `filter_datum` that obfuscates log messages based on the following parameters:
- **Arguments**:
  - `fields`: List of strings representing fields to obfuscate.
  - `redaction`: String representing the replacement text for obfuscation.
  - `message`: Log message string.
  - `separator`: Character used to separate fields in the log message.
- The function uses regex to replace occurrences of specified field values.
- `filter_datum` should be less than 5 lines and use `re.sub` for substitution.

### 1. Log Formatter
File: [filtered_logger.py](filtered_logger.py)

Updates:
- Add the following class to [filtered_logger.py](filtered_logger.py):
  ```python
  import logging

  class RedactingFormatter(logging.Formatter):
      """ Redacting Formatter class
      """

      REDACTION = "***"
      FORMAT = "[HOLBERTON] %(name)s %(levelname)s %(asctime)-15s: %(message)s"
      SEPARATOR = ";"

      def __init__(self):
          super(RedactingFormatter, self).__init__(self.FORMAT)

      def format(self, record: logging.LogRecord) -> str:
          raise NotImplementedError
  ```
- Modify the class to accept a `fields` list in the constructor.
- Implement the `format` method to filter values in log records using `filter_datum`.

### 2. Create Logger
File: [filtered_logger.py](filtered_logger.py)

Steps:
- Implement `get_logger` to return a `logging.Logger` object.
- Logger should:
  - Be named `"user_data"`.
  - Log up to `logging.INFO` level.
  - Not propagate messages.
  - Have a `StreamHandler` with `RedactingFormatter`.
- Create a tuple `PII_FIELDS` with fields from [user_data.csv](user_data.csv) considered PII.

### 3. Connect to Secure Database
File: [filtered_logger.py](filtered_logger.py)

Instructions:
- Database credentials should be stored as environment variables:
  - `PERSONAL_DATA_DB_USERNAME` (default: “root”)
  - `PERSONAL_DATA_DB_PASSWORD` (default: empty string)
  - `PERSONAL_DATA_DB_HOST` (default: “localhost”)
- Database name stored in `PERSONAL_DATA_DB_NAME`.
- Implement `get_db` to return a `mysql.connector.connection.MySQLConnection` object using environment credentials.

### 4. Read and Filter Data
File: [filtered_logger.py](filtered_logger.py)

Contains a `main` function:
- Obtains database connection using `get_db`.
- Retrieves all rows from `users` table and logs each row in filtered format:
  ```log
  [HOLBERTON] user_data INFO 2019-11-19 18:37:59,596: name=***; email=***; phone=***; ssn=***; password=***; ip=e848:e856:4e0b:a056:54ad:1e98:8110:ce1b; last_login=2019-11-14T06:16:24; user_agent=Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; WOW64; Trident/5.0; KTXN);
  ```
- Fields to filter: `name`, `email`, `phone`, `ssn`, `password`.
- `main` function should run only when the module is executed.

### 5. Encrypting Passwords
File: [encrypt_password.py](encrypt_password.py)

Requirements:
- Implement `hash_password` to hash passwords using bcrypt.
- The function should return a salted, hashed password as a byte string.

### 6. Check Valid Password
File: [app.py](app.py)

Contains `is_valid` function:
- **Arguments**:
  - `hashed_password`: `bytes`
  - `password`: `str`
- Use bcrypt to validate the provided password against the hashed password.

---

Each task helps enhance your understanding of personal data protection techniques, focusing on secure logging, database access, and password management.