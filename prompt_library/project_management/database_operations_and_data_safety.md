# Database Operations and Data Safety

## Database Instructions
- Use managed cloud databases (such as Supabase) by default, unless specified otherwise by the user or project requirements.

### Critical Data Preservation and Safety Requirements
- **Data integrity is the highest priority.** Users must never lose their data.
- **Forbidden:** Any destructive operations like `DROP` or `DELETE` that could result in data loss (e.g., dropping columns, changing column types, renaming tables, etc.)
- **Forbidden:** Any transaction control statements (e.g., explicit transaction management) such as `BEGIN`, `COMMIT`, or `ROLLBACK`.

### Migration File Rules
- Each migration file must follow these rules:
  - Always start with a markdown summary block (in a multi-line comment) that:
    - Includes a short, descriptive title summarizing the changes (e.g., "Schema update for blog features")
    - Explains in plain English what changes the migration makes
    - Lists all new tables and their columns with descriptions

### Client Setup
- Use the official client library for the database (e.g., `@supabase/supabase-js` for Supabase)
- Create a singleton client instance
- Use environment variables from the project's `.env` file
- Use TypeScript generated types from the schema when possible
