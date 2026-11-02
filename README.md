# Expense Tracker (Step 1: CRUD backend)

A simple Spring Boot app to create, read, update and delete expenses, stored in PostgreSQL.
Later we will add a frontend form (step 2) and AI features (step 3).

## Requirements

- Java 17 or newer
- Maven 3.9+ (or open the folder in IntelliJ / Eclipse and run the main class)
- PostgreSQL running on your Mac (default port 5432)

## 1. Create the database

Open a terminal and run:

```bash
psql -U postgres -c "CREATE DATABASE expensedb;"
```

Notes for Mac:

- If your install uses your Mac username instead of `postgres` (common with Postgres.app and Homebrew), run `psql -d postgres -c "CREATE DATABASE expensedb;"` instead.
- You only need to create the database. The `expense` table is created automatically when the app starts.

## 2. Set your database login

Open `src/main/resources/application.properties` and set the username and password you use for Postgres, or set them as environment variables:

```bash
export DB_USER=postgres
export DB_PASSWORD=your_password
```

If your Postgres has no password (some Homebrew setups), set `DB_PASSWORD=` to empty and use your Mac username as `DB_USER`.

## 3. Run

```bash
mvn spring-boot:run
```

The app starts at http://localhost:8080

## Project structure

```
src/main/java/com/example/expense/
  ExpenseTrackerApplication.java   -> starts the app
  Expense.java                     -> Entity (the data model / table)
  ExpenseRepository.java           -> database access (save, findAll, ...)
  ExpenseController.java           -> the 4 CRUD endpoints
src/main/resources/
  application.properties           -> PostgreSQL settings
```

## Try the endpoints (Postman or curl)

**Create**
```bash
curl -X POST http://localhost:8080/expenses \
  -H "Content-Type: application/json" \
  -d '{"description":"Pizza with friends","amount":450,"category":"FOOD","date":"2026-10-20"}'
```

**Read all**
```bash
curl http://localhost:8080/expenses
```

**Read one**
```bash
curl http://localhost:8080/expenses/1
```

**Update**
```bash
curl -X PUT http://localhost:8080/expenses/1 \
  -H "Content-Type: application/json" \
  -d '{"description":"Pizza and drinks","amount":600,"category":"FOOD","date":"2026-10-20"}'
```

**Delete**
```bash
curl -X DELETE http://localhost:8080/expenses/1
```

## Check the data in Postgres

```bash
psql -U postgres -d expensedb -c "SELECT * FROM expense;"
```

Unlike H2, this data stays after you stop the app.

## What to notice

- Validation: send an empty description or a negative amount and you get a 400 error.
- Update/Delete on an id that does not exist returns 404.

## Next steps

1. Frontend form (HTML + JavaScript) that calls these endpoints
2. AI: type "Paid 450 for pizza yesterday" and let the model fill the fields
