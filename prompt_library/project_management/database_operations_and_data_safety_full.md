# Database Operations and Data Safety

## Default Database
- Use Supabase as the default database unless otherwise specified.

## Data Integrity and Safety
- Data integrity is the highest priority—users must never lose their data.
- Destructive operations, such as `DROP` or `DELETE`, are forbidden if they could result in data loss (e.g., dropping columns, changing column types, renaming tables).
- Transaction control statements (e.g., `BEGIN`) are forbidden unless explicitly allowed.

## Migration File Guidelines
- Each migration file must:
  - Start with a markdown summary block describing the changes, new tables/columns, modifications, security changes, and important notes.
  - Use clear headings and numbered sections for readability.
  - Include all necessary operations (table creation, updates, RLS, policies).
  - Use `IF EXISTS` or `IF NOT EXISTS` to prevent errors.
  - Always enable Row Level Security (RLS) for new tables and add appropriate policies.
  - Set default values for columns where appropriate, but avoid masking potential problems.
- Never update existing migration files—always create new ones for any changes.
- Name migration files descriptively, without number prefixes.
- Never skip RLS setup for any table—security is non-negotiable.

## Client and Authentication Setup
- Use `@supabase/supabase-js` for client setup, with a singleton instance and environment variables from `.env`.
- Use TypeScript-generated types from the schema.
- Always use email/password sign-up for authentication.
- Never use magic links, social providers, or custom authentication tables.
- Email confirmation is always disabled unless explicitly stated.

## Best Practices
- One migration per logical change.
- Use descriptive policy names and add indexes for frequently queried columns.
- Keep RLS policies simple and focused.
- Use foreign key constraints.
- Generate types from the schema and use strong typing throughout the application.

