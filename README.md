# BloodLink

BloodLink is a portfolio/demo Flask application that helps connect blood
requesters with nearby volunteer donors. It is not a medical platform,
blood compatibility validator, or emergency service.

## Requirements

- Python 3.13
- MySQL local server
- Git

## Local setup

1. Create and activate a virtual environment.
2. Copy `.env.example` to `.env`.
3. Set `SECRET_KEY` and local MySQL values in `.env`.
4. Create the `bloodlink_db` database in MySQL.
5. Install dependencies:
   `python -m pip install -r requirements.txt`
6. Initialize migrations:
   `flask db init`
7. Start the application:
   `flask --app wsgi run --debug`

## Tests

Run:

`python -m pytest`

The foundation test suite uses an isolated in-memory SQLite database.
Production and local development use MySQL.

## Environment

Never commit `.env`. Use strong unique values for `SECRET_KEY` and your MySQL
password. Demo data must use fake personal information only.
