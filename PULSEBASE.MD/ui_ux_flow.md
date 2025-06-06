# PulseBase - UI/UX Flow Documentation

This document outlines the key user interface (UI) and user experience (UX) flows for the PulseBase platform, derived from the MVP, PRD, technical specifications, user stories, and API documentation.

## 1. User Onboarding and Authentication

### 1.1. Sign Up
   - **Trigger:** New user visits the PulseBase website and clicks "Sign Up".
   - **UI:** Registration form (Email, Password).
   - **Flow:**
     1. User enters email and password.
     2. System validates input.
     3. `POST /auth/v1/signup` is called.
     4. On success, user is redirected to project creation or dashboard.
     5. Welcome email is sent.
   - **UX Goal:** Quick and easy account creation.

### 1.2. Sign In
   - **Trigger:** Existing user visits the PulseBase website and clicks "Sign In".
   - **UI:** Login form (Email, Password).
   - **Flow:**
     1. User enters email and password.
     2. `POST /auth/v1/token?grant_type=password` is called.
     3. On success, user is redirected to their last visited project or organization dashboard.
     4. JWT tokens (access and refresh) are stored securely in the client.
   - **UX Goal:** Seamless access for returning users.

### 1.3. Sign Out
   - **Trigger:** User clicks "Sign Out" from their profile menu.
   - **UI:** Confirmation (optional).
   - **Flow:**
     1. `POST /auth/v1/signout` is called.
     2. Client-side tokens are cleared.
     3. User is redirected to the Sign In page.
   - **UX Goal:** Securely end user session.

## 2. Organization and Project Management

### 2.1. Create Organization
   - **Trigger:** User (after sign-up/sign-in) chooses to create a new organization.
   - **UI:** Form (Organization Name, Slug (auto-generated/editable)).
   - **Flow:**
     1. User enters organization details.
     2. `POST /organizations` is called.
     3. On success, user is typically prompted to create their first project within this organization or lands on the organization dashboard.
   - **UX Goal:** Simple setup for collaborative work.

### 2.2. Invite User to Organization
   - **Trigger:** Organization admin navigates to Organization Settings > Members > "Invite User".
   - **UI:** Form (User Email, Role).
   - **Flow:**
     1. Admin enters invitee's email and selects a role.
     2. `POST /organizations/{organization_id}/invitations` is called.
     3. Invitation email is sent to the user.
     4. Invitee appears in a "Pending Invitations" list.
   - **UX Goal:** Easy team expansion and role assignment.

### 2.3. Create Project
   - **Trigger:** User (within an organization context) clicks "Create New Project".
   - **UI:** Form (Project Name, Region (if applicable), Database Password (auto-generated/user-defined)).
   - **Flow:**
     1. User provides project details.
     2. System provisions database, API keys, and other project resources.
     3. User is redirected to the new project's dashboard/overview page.
   - **UX Goal:** Quick project initialization.

## 3. Database Management

### 3.1. View Tables
   - **Trigger:** User navigates to Project > Database > Table Editor.
   - **UI:** List of tables, with options to view schema, data, and relationships.
   - **Flow:**
     1. `GET /rest/v1/` (or equivalent internal call to list tables) is performed.
     2. Tables are displayed in a structured manner.
   - **UX Goal:** Clear overview of database structure.

### 3.2. Create Table
   - **Trigger:** User clicks "New Table" in the Table Editor.
   - **UI:** Interface to define table name, columns (name, type, constraints), primary keys, foreign keys, enable RLS.
   - **Flow:**
     1. User defines table structure.
     2. System generates and executes `CREATE TABLE` SQL statement.
     3. Table appears in the list.
   - **UX Goal:** Intuitive schema design.

### 3.3. Browse/Edit Table Data
   - **Trigger:** User selects a table and clicks "View Data" or similar.
   - **UI:** Spreadsheet-like interface displaying rows and columns. Options for filtering, sorting, pagination, adding new rows, editing cells, deleting rows.
   - **Flow (Read):**
     1. `GET /rest/v1/{table_name}` with appropriate query parameters is called.
   - **Flow (Create Row):**
     1. User clicks "Add Row", fills in data.
     2. `POST /rest/v1/{table_name}` is called.
   - **Flow (Update Row/Cell):**
     1. User edits a cell.
     2. `PATCH /rest/v1/{table_name}?{filter_column}=eq.{value}` is called.
   - **Flow (Delete Row):**
     1. User selects row(s) and clicks "Delete".
     2. `DELETE /rest/v1/{table_name}?{filter_column}=eq.{value}` is called.
   - **UX Goal:** Direct and easy data manipulation, similar to Supabase.

### 3.4. SQL Editor
   - **Trigger:** User navigates to Project > Database > SQL Editor.
   - **UI:** Monaco editor for writing SQL queries, results panel, query history, saved queries.
   - **Flow:**
     1. User writes and executes a SQL query.
     2. System sends query to the database (e.g., via an internal RPC or a dedicated SQL execution endpoint).
     3. Results are displayed.
   - **UX Goal:** Powerful query execution for advanced users.

## 4. AI & Vector Search

### 4.1. Generate Embeddings (Implicit/Explicit)
   - **Implicit (e.g., on table creation/configuration):**
     - **Trigger:** User configures a text column in a table for automatic embedding generation.
     - **UI:** Checkbox or setting in table schema editor: "Enable automatic embeddings for this column", model selection.
     - **Flow:** Backend process (e.g., database trigger + function) calls `POST /rest/v1/rpc/generate_embeddings` for new/updated text data in that column.
   - **Explicit (e.g., via API or a dedicated UI tool):**
     - **Trigger:** Developer calls the API or uses a UI tool to embed specific text.
     - **UI (Tool):** Input field for text, model selection, button "Generate Embedding".
     - **Flow:** `POST /rest/v1/rpc/generate_embeddings` is called.
   - **UX Goal:** Seamless integration of embedding generation into data workflows.

### 4.2. Perform Vector Search
   - **Trigger:** User/application needs to find similar items based on text or existing vector.
   - **UI (for testing/dev):** A dedicated Vector Search interface in the dashboard.
     - Input for query text or vector.
     - Selection of table and vector column.
     - Filters, limit, similarity threshold inputs.
     - Results display (matched items + scores).
   - **Flow (API):** `POST /rest/v1/rpc/vector_search` is called with appropriate parameters.
   - **UX Goal:** Easy to test and utilize vector search capabilities.

### 4.3. Configure RAG
   - **Trigger:** User navigates to Project > AI > RAG Configuration.
   - **UI:** Form to create/edit RAG configurations.
     - Name, description.
     - Selection of document collections (tables/views).
     - Default LLM model selection.
     - System prompt editor.
     - Context size, etc.
   - **Flow:**
     1. User fills in configuration details.
     2. `POST /projects/{project_id}/rag/configure` is called.
   - **UX Goal:** Simple setup for Retrieval Augmented Generation pipelines.

### 4.4. Query RAG
   - **Trigger:** User/application sends a query to a configured RAG setup.
   - **UI (for testing/dev):** A chat-like interface or query input in the RAG section of the dashboard.
     - Input for natural language query.
     - Selection of RAG configuration.
     - Display of answer and sources.
   - **Flow (API):** `POST /projects/{project_id}/rag/query` is called.
   - **UX Goal:** Interactive testing and demonstration of RAG capabilities.

## 5. Analytics & Visualization

### 5.1. Create Dashboard
   - **Trigger:** User navigates to Project > Analytics and clicks "New Dashboard".
   - **UI:** Form for dashboard name and description.
   - **Flow:**
     1. User enters details.
     2. `POST /projects/{project_id}/dashboards` is called (initially, might just create the dashboard shell).
     3. User is taken to an empty dashboard canvas or a chart addition wizard.
   - **UX Goal:** Quick start for data visualization.

### 5.2. Add Chart/Visualization to Dashboard
   - **Trigger:** User clicks "Add Chart" or similar on a dashboard.
   - **UI:** Wizard/modal:
     1. Select chart type (line, bar, pie, etc.).
     2. Define data source (SQL query editor or table/column selector for simple charts).
     3. Configure chart options (axes, labels, colors, refresh interval).
   - **Flow:**
     1. User configures the chart.
     2. Chart configuration (including query) is saved as part of the dashboard's definition (likely an update to the dashboard via `POST /projects/{project_id}/dashboards` or a dedicated chart creation endpoint linked to the dashboard).
     3. Chart appears on the dashboard, fetching data via `GET /projects/{project_id}/dashboards/{dashboard_id}/data` (or individual chart data endpoint).
   - **UX Goal:** Flexible and intuitive chart creation.

### 5.3. View Dashboard
   - **Trigger:** User selects a dashboard from the list in Project > Analytics.
   - **UI:** Dashboard displaying all its configured charts with data.
   - **Flow:**
     1. `GET /projects/{project_id}/dashboards/{dashboard_id}/data` is called to fetch data for all charts.
     2. Charts are rendered.
   - **UX Goal:** Clear and insightful data presentation.

## 6. Cache Management

### 6.1. View Cache Stats
   - **Trigger:** User navigates to Project > Settings > Cache Management.
   - **UI:** Display of cache statistics (hit rate, miss rate, total entries, memory usage).
   - **Flow:** `GET /projects/{project_id}/cache/stats` is called.
   - **UX Goal:** Provide visibility into cache performance.

### 6.2. Invalidate Cache
   - **Trigger:** User needs to clear specific cache entries.
   - **UI:** Form to specify invalidation criteria (table, keys, pattern).
   - **Flow:**
     1. User specifies criteria and submits.
     2. `POST /projects/{project_id}/cache/invalidate` is called.
   - **UX Goal:** Allow manual control over cache freshness when needed.

## 7. Developer Experience

### 7.1. Access API Documentation
   - **Trigger:** User navigates to Project > API Docs or a general "Documentation" section.
   - **UI:** Interactive API documentation (e.g., Swagger UI/Redoc) generated from OpenAPI spec. Shows endpoints, request/response schemas, and allows test calls.
   - **Flow:** Static content serving or dynamically generated page.
   - **UX Goal:** Easy discovery and understanding of API capabilities.

### 7.2. Manage API Keys
   - **Trigger:** User navigates to Project > Settings > API Keys.
   - **UI:** List of API keys (public, secret), options to create new keys, revoke existing keys, copy keys.
   - **Flow:** Internal API calls to manage API key lifecycle.
   - **UX Goal:** Secure and straightforward API key management.

This UI/UX flow documentation provides a high-level overview. Detailed wireframes, mockups, and interactive prototypes would be developed in subsequent design phases.
