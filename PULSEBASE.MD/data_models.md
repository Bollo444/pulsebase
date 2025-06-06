# PulseBase - Detailed Data Models

This document outlines the detailed data models for PulseBase, derived from the technical specifications, product requirements, and defined epics/tasks.

## Core Entities

### 1. `organizations`
   - Purpose: Represents an organization or a top-level account.
   - Fields:
     - `id` (UUID, Primary Key, Default: gen_random_uuid())
     - `name` (TEXT, Not Null)
     - `owner_user_id` (UUID, Foreign Key to `users.id`)
     - `created_at` (TIMESTAMPTZ, Default: now())
     - `updated_at` (TIMESTAMPTZ, Default: now())

### 2. `users`
   - Purpose: Stores user account information.
   - Fields:
     - `id` (UUID, Primary Key, Default: gen_random_uuid())
     - `email` (TEXT, Unique, Not Null)
     - `hashed_password` (TEXT, Not Null)
     - `full_name` (TEXT)
     - `is_active` (BOOLEAN, Default: true)
     - `last_login_at` (TIMESTAMPTZ)
     - `created_at` (TIMESTAMPTZ, Default: now())
     - `updated_at` (TIMESTAMPTZ, Default: now())

### 3. `organization_members`
   - Purpose: Links users to organizations and defines their primary role within that organization.
   - Fields:
     - `organization_id` (UUID, Foreign Key to `organizations.id`, Part of Composite PK)
     - `user_id` (UUID, Foreign Key to `users.id`, Part of Composite PK)
     - `role` (TEXT, Not Null, e.g., 'admin', 'member')
     - `joined_at` (TIMESTAMPTZ, Default: now())
     - Primary Key: (`organization_id`, `user_id`)

### 4. `projects`
   - Purpose: Represents a development project within an organization.
   - Fields:
     - `id` (UUID, Primary Key, Default: gen_random_uuid())
     - `organization_id` (UUID, Foreign Key to `organizations.id`, Not Null)
     - `name` (TEXT, Not Null)
     - `description` (TEXT)
     - `db_connection_string` (TEXT)  -- Potentially encrypted or managed by a separate service
     - `created_at` (TIMESTAMPTZ, Default: now())
     - `updated_at` (TIMESTAMPTZ, Default: now())

### 5. `project_members` (Teams & Permissions - Granular)
   - Purpose: Manages user access to specific projects and their roles within those projects.
   - Fields:
     - `project_id` (UUID, Foreign Key to `projects.id`, Part of Composite PK)
     - `user_id` (UUID, Foreign Key to `users.id`, Part of Composite PK)
     - `role_id` (UUID, Foreign Key to `roles.id`)
     - `assigned_at` (TIMESTAMPTZ, Default: now())
     - Primary Key: (`project_id`, `user_id`)

### 6. `roles`
   - Purpose: Defines a set of permissions.
   - Fields:
     - `id` (UUID, Primary Key, Default: gen_random_uuid())
     - `project_id` (UUID, Foreign Key to `projects.id`, Nullable - for global roles vs project-specific roles)
     - `organization_id` (UUID, Foreign Key to `organizations.id`, Nullable - for org-level roles)
     - `name` (TEXT, Not Null, Unique within its scope - project/org/global)
     - `description` (TEXT)
     - `permissions` (JSONB) -- Stores an array of permission strings (e.g., `["table:create", "table:read:my_table"]`)
     - `created_at` (TIMESTAMPTZ, Default: now())
     - `updated_at` (TIMESTAMPTZ, Default: now())

## Database Specific Entities (Managed by PulseBase for User Projects)

These tables would reside within the user's provisioned PostgreSQL database, but PulseBase needs to be aware of their structure for API generation and management.

### 7. `pulsebase_schemas` (Metadata within user's DB)
   - Purpose: Tracks schemas created by the user within their project database.
   - Fields:
     - `id` (UUID, Primary Key)
     - `project_id` (UUID) -- Reference back to PulseBase project for context
     - `schema_name` (TEXT, Not Null, Unique within project)
     - `created_at` (TIMESTAMPTZ, Default: now())

### 8. `pulsebase_tables` (Metadata within user's DB)
   - Purpose: Tracks tables created by the user.
   - Fields:
     - `id` (UUID, Primary Key)
     - `schema_id` (UUID, Foreign Key to `pulsebase_schemas.id`)
     - `table_name` (TEXT, Not Null, Unique within schema)
     - `rls_enabled` (BOOLEAN, Default: false)
     - `created_at` (TIMESTAMPTZ, Default: now())

### 9. `pulsebase_columns` (Metadata within user's DB)
   - Purpose: Tracks columns within user tables.
   - Fields:
     - `id` (UUID, Primary Key)
     - `table_id` (UUID, Foreign Key to `pulsebase_tables.id`)
     - `column_name` (TEXT, Not Null)
     - `data_type` (TEXT, Not Null, e.g., 'TEXT', 'INTEGER', 'VECTOR(384)')
     - `is_nullable` (BOOLEAN, Default: true)
     - `is_unique` (BOOLEAN, Default: false)
     - `default_value` (TEXT)
     - `is_indexed` (BOOLEAN, Default: false)
     - `is_vector_column` (BOOLEAN, Default: false)
     - `vector_dimensions` (INTEGER, Nullable)
     - `created_at` (TIMESTAMPTZ, Default: now())

### 10. `pulsebase_rls_policies` (Metadata within user's DB)
    - Purpose: Stores Row-Level Security policies defined by the user.
    - Fields:
        - `id` (UUID, Primary Key)
        - `table_id` (UUID, Foreign Key to `pulsebase_tables.id`)
        - `policy_name` (TEXT, Not Null, Unique within table)
        - `command` (TEXT, Not Null, e.g., 'SELECT', 'INSERT', 'UPDATE', 'DELETE', 'ALL')
        - `role_names` (TEXT[], Nullable, e.g., `ARRAY['authenticated', 'custom_role_name']`)
        - `using_expression` (TEXT, Nullable) -- SQL expression for SELECT, UPDATE, DELETE
        - `check_expression` (TEXT, Nullable) -- SQL expression for INSERT, UPDATE
        - `created_at` (TIMESTAMPTZ, Default: now())

## AI & Vector Specific Entities

### 11. `vector_embeddings` (Could be part of user's table or a dedicated table)
    - This is more of a concept. Vector data will be stored in user-defined tables using a vector column type (e.g., `vector(dimensions)`).
    - The `pulsebase_columns` table will mark columns as vector columns and store their dimensions.

### 12. `embedding_models`
    - Purpose: Stores information about available embedding models.
    - Fields:
        - `id` (UUID, Primary Key)
        - `name` (TEXT, Not Null, Unique, e.g., 'text-embedding-ada-002', 'all-MiniLM-L6-v2')
        - `provider` (TEXT, e.g., 'OpenAI', 'HuggingFace')
        - `dimensions` (INTEGER, Not Null)
        - `api_endpoint` (TEXT, Nullable)
        - `is_active` (BOOLEAN, Default: true)
        - `created_at` (TIMESTAMPTZ, Default: now())

### 13. `project_embedding_configs`
    - Purpose: Configures which embedding models are used by a project and for which tables/columns (if auto-embedding).
    - Fields:
        - `id` (UUID, Primary Key)
        - `project_id` (UUID, Foreign Key to `projects.id`)
        - `table_id` (UUID, Foreign Key to `pulsebase_tables.id`, Nullable - for project-wide default)
        - `source_column_id` (UUID, Foreign Key to `pulsebase_columns.id`, Nullable)
        - `target_vector_column_id` (UUID, Foreign Key to `pulsebase_columns.id`, Nullable)
        - `embedding_model_id` (UUID, Foreign Key to `embedding_models.id`)
        - `auto_embed_on_insert` (BOOLEAN, Default: false)
        - `auto_embed_on_update` (BOOLEAN, Default: false)
        - `created_at` (TIMESTAMPTZ, Default: now())

### 14. `rag_configurations`
    - Purpose: Stores RAG pipeline configurations for a project.
    - Fields:
        - `id` (UUID, Primary Key)
        - `project_id` (UUID, Foreign Key to `projects.id`, Unique)
        - `llm_model_id` (TEXT, Not Null, e.g., 'gpt-3.5-turbo') -- Could link to another table if we manage LLMs
        - `context_window_size` (INTEGER, Default: 4096)
        - `system_prompt` (TEXT)
        - `vector_search_top_k` (INTEGER, Default: 5)
        - `created_at` (TIMESTAMPTZ, Default: now())
        - `updated_at` (TIMESTAMPTZ, Default: now())

## Analytics & Caching Entities

### 15. `dashboards`
    - Purpose: Stores dashboard configurations.
    - Fields:
        - `id` (UUID, Primary Key)
        - `project_id` (UUID, Foreign Key to `projects.id`)
        - `name` (TEXT, Not Null)
        - `layout_config` (JSONB) -- Stores widget positions and sizes
        - `created_at` (TIMESTAMPTZ, Default: now())
        - `updated_at` (TIMESTAMPTZ, Default: now())

### 16. `dashboard_widgets`
    - Purpose: Stores individual widget configurations within a dashboard.
    - Fields:
        - `id` (UUID, Primary Key)
        - `dashboard_id` (UUID, Foreign Key to `dashboards.id`)
        - `widget_type` (TEXT, Not Null, e.g., 'line_chart', 'bar_chart', 'kpi')
        - `title` (TEXT)
        - `query` (TEXT, Not Null) -- SQL query to fetch data
        - `visualization_options` (JSONB) -- Chart-specific options
        - `refresh_interval_seconds` (INTEGER, Nullable)
        - `created_at` (TIMESTAMPTZ, Default: now())
        - `updated_at` (TIMESTAMPTZ, Default: now())

### 17. `cache_configurations`
    - Purpose: Stores caching rules for a project or specific queries/tables.
    - Fields:
        - `id` (UUID, Primary Key)
        - `project_id` (UUID, Foreign Key to `projects.id`)
        - `target_entity` (TEXT, Nullable, e.g., 'table_name', 'query_hash')
        - `ttl_seconds` (INTEGER, Not Null)
        - `invalidation_rules` (JSONB) -- e.g., event-based triggers
        - `is_active` (BOOLEAN, Default: true)
        - `created_at` (TIMESTAMPTZ, Default: now())

## System & Operational Entities

### 18. `api_keys`
    - Purpose: Stores API keys for project access.
    - Fields:
        - `id` (UUID, Primary Key)
        - `project_id` (UUID, Foreign Key to `projects.id`)
        - `user_id` (UUID, Foreign Key to `users.id`, Nullable - for service keys)
        - `key_hash` (TEXT, Not Null, Unique) -- Store hash, not the key itself
        - `key_prefix` (TEXT, Not Null, Unique) -- First few chars of the key for identification
        - `scopes` (TEXT[]) -- Array of permission scopes
        - `expires_at` (TIMESTAMPTZ, Nullable)
        - `last_used_at` (TIMESTAMPTZ, Nullable)
        - `created_at` (TIMESTAMPTZ, Default: now())

### 19. `audit_logs`
    - Purpose: Records significant actions performed within the system.
    - Fields:
        - `id` (BIGSERIAL, Primary Key)
        - `organization_id` (UUID, Foreign Key to `organizations.id`, Nullable)
        - `project_id` (UUID, Foreign Key to `projects.id`, Nullable)
        - `user_id` (UUID, Foreign Key to `users.id`, Nullable)
        - `action` (TEXT, Not Null, e.g., 'user.login', 'project.create', 'table.update')
        - `target_resource_id` (TEXT, Nullable)
        - `payload` (JSONB) -- Details of the action
        - `ip_address` (INET)
        - `user_agent` (TEXT)
        - `created_at` (TIMESTAMPTZ, Default: now())

### 20. `realtime_subscriptions`
    - Purpose: Manages active real-time subscriptions.
    - Fields:
        - `id` (UUID, Primary Key)
        - `project_id` (UUID, Foreign Key to `projects.id`)
        - `user_id` (UUID, Foreign Key to `users.id`)
        - `connection_id` (TEXT, Not Null) -- WebSocket connection ID or similar
        - `entity_type` (TEXT, Not Null, e.g., 'table', 'query')
        - `entity_identifier` (TEXT, Not Null) -- e.g., table name, query hash
        - `filters` (JSONB) -- Specific filters for the subscription
        - `created_at` (TIMESTAMPTZ, Default: now())

## Notes:
- All `UUID` primary keys should ideally use `gen_random_uuid()` as default.
- `TIMESTAMPTZ` is used for all timestamps to ensure timezone awareness.
- `JSONB` is preferred over `JSON` for performance and indexing capabilities in PostgreSQL.
- Relationships and constraints (e.g., `ON DELETE CASCADE`) need to be defined during actual schema creation.
- This is an initial model and will evolve as development progresses.
- Indexing strategies for various fields (especially foreign keys and frequently queried columns) will be crucial for performance.
