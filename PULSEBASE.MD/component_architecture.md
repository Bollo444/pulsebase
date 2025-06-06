# PulseBase - Component Architecture

This document outlines the detailed component architecture for both the frontend and backend of the PulseBase platform, derived from the technical specifications and the defined epics and tasks.

## Frontend Component Architecture

**Core Framework:** React 18+ with TypeScript
**State Management:** Redux Toolkit (global), React Query (server state), Context API (feature-specific)
**Styling:** Tailwind CSS

### High-Level Modules (as per `tech_specs_frontend.md`):

1.  **Core Module:**
    *   `AuthComponents`: Login, Registration, Password Reset, Profile Management UI.
    *   `NavigationComponents`: Main Sidebar, Top Navbar, Breadcrumbs.
    *   `LayoutComponents`: Main App Layout, Dashboard Layout, Settings Layout.
    *   `UtilityComponents`: Error Boundary, Loading Indicators, Notification System, Modal/Dialog Manager.
    *   `GlobalSearchComponent`: Input and results display for platform-wide search.
    *   `HelpSystemComponents`: Contextual help popovers, documentation links.

2.  **Database Module:**
    *   `ProjectSelector`: Dropdown/UI to switch between projects within an organization.
    *   `TableBrowser`: Component to list, filter, sort tables.
    *   `SchemaVisualizer`: React Flow based component to display table schemas and relationships.
    *   `QueryEditor`: Monaco Editor instance configured for SQL, with autocomplete features.
    *   `DataGrid/ResultsViewer`: Component to display query results, with pagination, sorting, filtering, and export options.
    *   `VectorDataVisualizer`: Specific components for visualizing vector embeddings or search results (e.g., scatter plots, nearest neighbor lists).
    *   `RLSPolicyEditor`: UI for creating, viewing, and managing Row-Level Security policies.
    *   `RealtimeIndicator`: Visual cues for real-time data updates.

3.  **Analytics Module:**
    *   `DashboardGrid`: Drag-and-drop interface for arranging widgets on a dashboard.
    *   `WidgetLibrary`: Collection of pre-built and custom widget components (ChartWidget, StatWidget, TableWidget).
    *   `ChartRenderer`: Component utilizing Recharts/D3.js to render various chart types (line, bar, pie, scatter, etc.).
    *   `SQLQueryBuilderUI`: Visual interface or enhanced text editor for constructing SQL queries for widgets.
    *   `DashboardSettings`: Controls for dashboard name, description, sharing, auto-refresh intervals.
    *   `ReportExporter`: Component to trigger and manage export of dashboard data (CSV, PDF).
    *   `MetricsDisplay`: Components for showing API usage, query performance (potentially part of a dedicated admin/monitoring dashboard).

4.  **AI Module (Vector & RAG):**
    *   `EmbeddingModelSelector`: UI to choose and configure embedding models.
    *   `VectorIndexManager`: Interface for creating, monitoring, and managing vector indexes on tables/columns.
    *   `VectorSearchInterface`: Input fields for semantic search queries, parameter adjustments (e.g., k-neighbors), and results display.
    *   `RAGConfigurator`: UI for setting up RAG pipelines - LLM selection, prompt templating, context window size.
    *   `RAGQueryInterface`: Input for querying the RAG system, display for answers and source documents.

5.  **Team (Organization) Module:**
    *   `OrganizationCreator`: Wizard/form for setting up a new organization.
    *   `UserInvitationForm`: Interface to invite new members to an organization.
    *   `RoleManager`: UI for defining custom roles and their permissions (permission matrix editor).
    *   `UserRoleAssigner`: Component to assign roles to users within an organization.
    *   `ProjectAccessManager`: UI to control user/role access to specific projects.
    *   `AuditLogViewer`: Interface to display and filter audit logs for user and system activities.

6.  **Settings Module:**
    *   `ProjectSettingsEditor`: Forms for managing project-specific configurations.
    *   `APIKeyManager`: UI for generating, listing, revoking API keys, and setting their scopes.
    *   `GeneralPreferences`: User-specific preferences (e.g., theme, notification settings).

### Cross-Cutting Concerns (Handled by Core or specific utilities):
*   **Data Fetching:** Primarily React Query hooks, abstracting API calls.
*   **Error Handling:** Global error boundaries, specific error states in components.
*   **Accessibility:** ARIA attributes, keyboard navigation, semantic HTML implemented in all components.
*   **Responsiveness:** Tailwind CSS utility classes applied for adaptive layouts.

## Backend Component Architecture

**Core Language:** Golang
**API Layer:** RESTful APIs (OpenAPI), GraphQL, WebSockets
**Database:** PostgreSQL with pgvector
**Caching:** Redis

### Service Decomposition (as per `tech_specs_backend.md`):

1.  **Auth Service:**
    *   `UserHandler`: Manages user registration, login, profile updates.
    *   `TokenGenerator`: Creates and validates JWTs.
    *   `SessionManager`: Handles user sessions (if stateful sessions are used alongside JWTs).
    *   `OIDCIntegrator`: (Optional) Handles OIDC provider interactions.
    *   `PasswordManager`: Securely hashes and verifies passwords.

2.  **Database Service (PostgreSQL Management & Query Handling):**
    *   `DBProvisioner`: Handles creation/deletion of PostgreSQL databases for projects.
    *   `SchemaManager`: API endpoints for DDL operations (CREATE/ALTER TABLE, CREATE INDEX, etc.). Validates schema changes.
    *   `QueryExecutor`: Securely executes DML (SELECT, INSERT, UPDATE, DELETE) and DQL queries against user databases. Includes SQL injection protection.
    *   `RLSEnforcer`: Applies RLS policies to queries before execution.
    *   `RLSPolicyManager`: API for creating/updating/deleting RLS policies in the database.
    *   `RealtimeNotifier`: Integrates with PostgreSQL LISTEN/NOTIFY or triggers to push data changes via WebSockets.
    *   `ConnectionPoolManager`: Manages database connections (e.g., PgBouncer integration or internal pooling).
    *   `MigrationRunner`: Handles schema migrations for user databases.

3.  **Storage Service (File Storage & CDN Management - if applicable, less detailed in current specs):**
    *   `FileUploadHandler`: Manages uploads.
    *   `FileAccessController`: Enforces permissions on file access.
    *   `CDNIntegrator`: Interfaces with CDN for file delivery.

4.  **Vector Service (Embedding Generation & Vector Search):**
    *   `EmbeddingGenerator`: Integrates with LangChain/embedding models (OpenAI, Hugging Face) to generate vectors for text data. Can be triggered via API or automatically on data changes.
    *   `VectorIndexManager`: Manages creation and maintenance of vector indexes (HNSW, IVF) using pgvector.
    *   `VectorSearchHandler`: Executes similarity searches (cosine, dot product, euclidean) and hybrid searches. Optimizes queries for pgvector.
    *   `VectorConfigManager`: Manages configurations for different embedding models and vector spaces.

5.  **Analytics Service (Query Processing & Visualization Data):**
    *   `AnalyticsQueryProcessor`: Executes user-defined SQL queries for dashboards, optimized for analytical workloads.
    *   `DataAggregator`: Performs aggregations and transformations for chart data.
    *   `DashboardDataProvider`: Prepares and serves data specifically formatted for frontend dashboard widgets.
    *   `ReportGenerator`: Creates CSV/PDF exports from dashboard data.
    *   `MetricsCollector`: Gathers API usage, query performance, and other system metrics.

6.  **Cache Service (Intelligent Caching & Invalidation):**
    *   `CacheManager`: Interfaces with Redis to store and retrieve cached query results or other data.
    *   `InvalidationEngine`: Implements logic for cache invalidation (TTL, event-based from DB triggers/notifications).
    *   `CacheWarmer`: (Optional) Proactively populates cache for frequently accessed data.
    *   `CacheStatsCollector`: Tracks cache hit/miss rates and other performance metrics.

7.  **Organization Service (Team & Permission Management):**
    *   `OrganizationManager`: Handles creation and management of organizations.
    *   `UserManager`: Manages user profiles within organizations, invitations.
    *   `RoleManager`: Defines and manages roles and their associated permissions.
    *   `PermissionEnforcer`: Checks user permissions before allowing actions on resources.
    *   `AuditLogger`: Records significant user and system actions for security and compliance.

8.  **API Gateway (Request Routing & Rate Limiting):**
    *   `RequestRouter`: Directs incoming API requests to the appropriate backend service.
    *   `AuthMiddleware`: Verifies JWTs and user authentication for protected endpoints.
    *   `RateLimiter`: Enforces API rate limits per user/IP/API key.
    *   `RequestLogger`: Logs all incoming requests and responses.
    *   `CORSHandler`: Manages Cross-Origin Resource Sharing policies.

### Cross-Cutting Concerns (Handled by specific services or shared libraries):
*   **Database Interaction:** Common Golang libraries for PostgreSQL, potentially an ORM or query builder for structured access, raw SQL for complex queries.
*   **Error Handling:** Standardized error responses across all APIs.
*   **Logging:** Centralized logging solution (Loki + Promtail).
*   **Configuration Management:** Loading configurations for services (e.g., from env vars, config files, Vault).
*   **Security:** Input validation, output encoding, SQL injection prevention, secure API key handling, encryption at rest/transit handled within relevant services.

This component architecture will be further refined during the detailed design and implementation phases of each epic and task.