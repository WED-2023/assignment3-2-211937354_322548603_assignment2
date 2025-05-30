# 🗃️ SQL Scripts – Initialization & Schema Documentation

This directory contains all SQL-related logic for the project, including:
- Table creation script (`schema.sql`)
- Test data insertion script (`initial_data.sql`)
- Connection configuration (`db_connection.js`)
- Database explanation and modeling notes

---

## ⚙️ Files Overview

| File                    | Description                                  |
|-------------------------|----------------------------------------------|
| `schema.sql`            | Creates all 11 tables (with `USE recipes_db`) |
| `initial_data.sql`      | Inserts sample users, recipes, ingredients    |
| `db_connection.js`      | MySQL connection pool using dotenv            |
| `README.md`             | You are here – full schema documentation      |
---

## 📌 Setup Instructions

1. Ensure your `.env` file includes the following values:

```
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=123456
DB_NAME=recipes_db
```

2. Open `schema.sql` and run it:
    - The script contains a `USE recipes_db;` statement
    - If the database does **not** yet exist, uncomment the first line:
      ```sql
      CREATE DATABASE IF NOT EXISTS recipes_db;
      ```

3. Execute `initial_data.sql` to populate example rows for testing.

4. Make sure your SQL dialect is set to **MySQL** in WebStorm or your IDE.

---

## 📋 SQL Syntax & Compatibility

- Syntax must be set to `MySQL` (not `Generic SQL`) for WebStorm/JetBrains IDEs.
- All tables use proper keys, constraints, and relationships.
- Use a proper MySQL interpreter (e.g., WebStorm, DataGrip, CLI).

---

## 📘 FK Constraints – Explanation

**FK** stands for **Foreign Key** – a constraint used to link child tables to parent tables using consistent reference values (e.g., `user_id`, `recipe_id`).

---

## 🧱 Tables Overview

Each table below includes:

- **Purpose**
- **Primary Key**
- **Relation to other tables**
- **Whether it’s mandatory or bonus**
- **Justification in the model**

---

### 1. `users` – ✅ Required

- **Purpose**: Stores registered user info: username, name, country, email, password
- **PK**: `user_id`
- **Used In**: All other tables as foreign key
- **Why**: Central identity table

---

### 2. `user_recipes` – ✅ Required

- **Purpose**: Recipes created by users
- **PK**: `recipe_id`
- **FK**: `user_id → users(user_id)`
- **Why**: Lets each user manage their own custom content

---

### 3. `user_recipe_ingredients` – ✅ Required

- **Purpose**: Ingredients belonging to user recipes
- **PK**: `ingredient_id`
- **FK**: `recipe_id → user_recipes(recipe_id)`
- **Why separate**: Many-to-one relationship; avoid clutter in main table

---

### 4. `family_recipes` – ✅ Required

- **Purpose**: Family-style recipes with story, owner name, timing
- **PK**: `recipe_id`
- **FK**: `user_id → users(user_id)`
- **Why separate**: Different fields (owner, heritage text)

---

### 5. `family_recipe_ingredients` – ✅ Required

- **Purpose**: Ingredients for family recipes
- **PK**: `ingredient_id`
- **FK**: `recipe_id → family_recipes(recipe_id)`
- **Why separate**: Keeps family structure consistent and normalized

---

### 6. `user_favorites` – ✅ Required

- **Purpose**: Links users to external Spoonacular recipes they marked as favorites
- **PK**: `favorite_id`
- **FK**: `user_id`
- **Why**: Needed for favorites tab + API integration

---

### 7. `recipe_views` – ✅ Required

- **Purpose**: Tracks user views of Spoonacular recipes
- **PK**: `view_id`
- **FK**: `user_id`
- **Why**: Used for “recently viewed” section

---

### 8. `search_history` – ✅ Required

- **Purpose**: Logs search inputs + filters
- **PK**: `search_id`
- **FK**: `user_id`
- **Why**: Used for “previous searches” feature

---

### 9. `meal_plans` – ✅ Bonus

- **Purpose**: Weekly plan (mix of all recipe types)
- **PK**: `plan_id`
- **FK**: one of `spoonacular_recipe_id`, `user_recipe_id`, or `family_recipe_id`
- **Why**: Advanced personalization

---

### 10. `recipe_preparation_progress` – ✅ Bonus

- **Purpose**: Track step-by-step live cooking progress
- **PK**: `progress_id`
- **FKs**: `user_id` and one of the 3 recipe sources
- **Why**: Real-time cooking flow

---

### 11. `recipe_preparation_steps` – ✅ Bonus

- **Purpose**: Holds structured steps for user/family recipes
- **PK**: `step_id`
- **FK**: Either `user_recipe_id` or `family_recipe_id`
- **Why**: Normalize recipe steps into a dedicated, queryable table

---

## 📌 Summary

The database was carefully normalized and designed to:
- Avoid redundancy
- Enable expansion (bonus features)
- Remain scalable for future extensions

Each FK and schema element reflects a functional or UI requirement in the project.