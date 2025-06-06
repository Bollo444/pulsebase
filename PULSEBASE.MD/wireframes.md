# PulseBase - Wireframes

This document outlines the wireframes for key user flows in the PulseBase platform, based on `ui_ux_flow.md` and `user_stories.md`.

## 1. User Onboarding and Authentication

### 1.1. Sign Up Page
   - **Objective:** Allow new users to create an account.
   - **Key Elements:**
     - Logo/Brand Name
     - Page Title: "Create your PulseBase Account" or similar
     - Email Input Field (with validation)
     - Password Input Field (with validation, show/hide option)
     - Confirm Password Input Field (optional, but good practice)
     - "Sign Up" Button (Primary Action)
     - Link: "Already have an account? Sign In"
   - **User Flow Reference:** `ui_ux_flow.md` - 1.1. Sign Up
   - **User Story Reference:** Implicitly supports all user stories by enabling access.

### 1.2. Sign In Page
   - **Objective:** Allow existing users to log in.
   - **Key Elements:**
     - Logo/Brand Name
     - Page Title: "Sign In to PulseBase" or similar
     - Email Input Field
     - Password Input Field (show/hide option)
     - "Sign In" Button (Primary Action)
     - Link: "Forgot your password?"
     - Link: "Don't have an account? Sign Up"
   - **User Flow Reference:** `ui_ux_flow.md` - 1.2. Sign In

### 1.3. Password Reset Request Page
   - **Objective:** Allow users to request a password reset link.
   - **Key Elements:**
     - Logo/Brand Name
     - Page Title: "Reset Your Password"
     - Email Input Field (for account identification)
     - "Send Reset Link" Button
     - Link: "Back to Sign In"
   - **User Flow Reference:** Implied by "Forgot your password?" link.

### 1.4. Password Reset Page
   - **Objective:** Allow users to set a new password using a reset link.
   - **Key Elements:**
     - Logo/Brand Name
     - Page Title: "Set Your New Password"
     - New Password Input Field
     - Confirm New Password Input Field
     - "Set New Password" Button
   - **User Flow Reference:** Implied by password reset process.

## 2. Organization and Project Management

### 2.1. Create Organization Page/Modal
   - **Objective:** Allow users to create a new organization.
   - **Key Elements:**
     - Title: "Create New Organization"
     - Organization Name Input Field
     - Organization Slug Input Field (auto-generated from name, editable, with validation for uniqueness/format)
     - "Create Organization" Button
     - Cancel/Close Button
   - **User Flow Reference:** `ui_ux_flow.md` - 2.1. Create Organization
   - **User Story Reference:** "As an organization admin, I want to create an organization..."

### 2.2. Organization Dashboard / Project List
   - **Objective:** Display projects within an organization and allow creation of new projects.
   - **Key Elements:**
     - Organization Name Display
     - "Create New Project" Button
     - List of Existing Projects (Name, Last Modified, Quick Actions like 'Open', 'Settings')
     - Navigation to Organization Settings (Members, Billing, etc.)
   - **User Flow Reference:** Implied context for project creation and management.

### 2.3. Invite User to Organization Page/Modal
   - **Objective:** Allow organization admins to invite new members.
   - **Key Elements:**
     - Title: "Invite New Member to [Organization Name]"
     - Email Input Field for Invitee
     - Role Selection Dropdown (e.g., Admin, Developer, Viewer)
     - "Send Invitation" Button
     - Cancel/Close Button
     - Display of Pending Invitations (Email, Role, Status, Resend/Cancel options)
   - **User Flow Reference:** `ui_ux_flow.md` - 2.2. Invite User to Organization
   - **User Story Reference:** "As an organization admin, I want to ... invite team members... define roles..."

### 2.4. Create Project Page/Modal
   - **Objective:** Allow users to create a new project within an organization.
   - **Key Elements:**
     - Title: "Create New Project in [Organization Name]"
     - Project Name Input Field
     - Region Selection Dropdown (if applicable, as per tech specs)
     - Database Password Field (auto-generated, with option to reveal/copy, or user-defined with strength indicator)
     - "Create Project" Button
     - Cancel/Close Button
   - **User Flow Reference:** `ui_ux_flow.md` - 2.3. Create Project
   - **User Story Reference:** "As a developer, I want to create a new PostgreSQL database for my project..."

## 3. Database Management

### 3.1. Table Editor - Main View
   - **Objective:** Display list of tables and allow navigation to table-specific actions.
   - **Key Elements:**
     - Project Context Display (e.g., "Project: [Project Name] / Database")
     - "New Table" Button
     - Search/Filter Bar for tables
     - List of Tables:
       - Table Name
       - Number of Rows (optional)
       - RLS Enabled (indicator)
       - Quick Actions per table (View Data, Edit Schema, Settings, Delete)
     - Navigation to SQL Editor, Functions, Extensions, etc.
   - **User Flow Reference:** `ui_ux_flow.md` - 3.1. View Tables
   - **User Story Reference:** "As a developer, I want to define tables and schemas easily..."

### 3.2. Create/Edit Table Schema Page/Modal
   - **Objective:** Allow users to define or modify the structure of a table.
   - **Key Elements:**
     - Title: "Create New Table" or "Edit Schema: [Table Name]"
     - Table Name Input Field (disabled if editing)
     - "Enable Row Level Security (RLS)" Checkbox
     - Column Definition Section (Grid or Repeated Blocks):
       - Column Name Input
       - Data Type Selector (Dropdown: text, int, bool, timestamp, uuid, jsonb, vector, etc.)
       - Default Value Input (optional)
       - Is Nullable Checkbox
       - Is Unique Checkbox
       - Is Primary Key Checkbox (allow multiple for composite PKs)
       - Foreign Key Configuration (optional: target table, target column)
       - For Vector Type: Dimensions input, distance metric selector (cosine, L2, etc.)
       - Add/Remove Column Buttons
     - "Save Table" / "Create Table" Button
     - Cancel/Close Button
   - **User Flow Reference:** `ui_ux_flow.md` - 3.2. Create Table
   - **User Story Reference:** "As a developer, I want to define tables and schemas easily...", "As a developer, I want to set up row-level security policies...", "As a developer, I want to be able to create indexes on vector columns..." (implicitly, vector column definition)

### 3.3. Table Data Browser (Grid View)
   - **Objective:** Allow users to view, add, edit, and delete data within a table.
   - **Key Elements:**
     - Table Name Display
     - "Add Row" Button
     - Filter Controls (e.g., "Column = Value", add multiple filters)
     - Sort Controls (per column)
     - Pagination Controls (if many rows)
     - Data Grid:
       - Column Headers (clickable for sorting)
       - Rows of data (cells editable in-place or via a modal)
       - Checkbox per row for selection (for bulk delete)
     - "Delete Selected Rows" Button (appears if rows are selected)
     - Export Data Button (optional)
   - **User Flow Reference:** `ui_ux_flow.md` - 3.3. Browse/Edit Table Data
   - **User Story Reference:** "As a developer, I want to perform CRUD ... operations on my data..."

### 3.4. SQL Editor Page
   - **Objective:** Allow users to write and execute custom SQL queries.
   - **Key Elements:**
     - Project Context Display
     - SQL Input Area (Monaco Editor or similar, with syntax highlighting)
     - "Run Query" Button (or Ctrl+Enter)
     - "Save Query" Button (with name input)
     - Results Panel (displays query output in a table, or error messages)
     - Query History List (clickable to reload past queries)
     - Saved Queries List (clickable to load saved queries)
     - Schema Browser (optional, tree view of tables/columns for reference)
   - **User Flow Reference:** `ui_ux_flow.md` - 3.4. SQL Editor
   - **User Story Reference:** Supports advanced data interaction beyond basic CRUD.

## 4. AI & Vector Search

### 4.1. Table Schema - Embedding Configuration (Integrated)
   - **Objective:** Allow users to configure automatic embedding generation for a text column.
   - **Key Elements (within Create/Edit Table Schema - Column Definition):**
     - When Data Type is 'text' or similar:
       - Checkbox: "Enable automatic embeddings for this column"
       - If checked:
         - Embedding Model Selector (Dropdown of available models, e.g., 'text-embedding-ada-002')
         - Associated Vector Column Name (auto-suggested or user-defined, e.g., `[column_name]_embedding`)
         - Vector Column Dimensions (auto-filled based on selected model, read-only)
   - **User Flow Reference:** `ui_ux_flow.md` - 4.1. Generate Embeddings (Implicit)
   - **User Story Reference:** "As a developer, I want to generate vector embeddings for text data automatically..."

### 4.2. Vector Search Test Page/Tool
   - **Objective:** Provide a UI for developers to test vector search functionality.
   - **Key Elements:**
     - Title: "Vector Search Playground" or similar
     - Query Input Type Selector (Radio buttons: "Query by Text", "Query by Vector")
       - If "Query by Text": Text Area for input query.
       - If "Query by Vector": Text Area for pasting a vector (e.g., `[0.1, 0.2, ...]`).
     - Embedding Model Selector (if Query by Text, to generate query vector)
     - Target Table Selector (Dropdown of tables with vector columns)
     - Target Vector Column Selector (Dropdown of vector columns in selected table)
     - Number of Results (Limit) Input (e.g., default 10)
     - Minimum Similarity Threshold Input (optional, e.g., 0.7)
     - Additional Filters Section (optional, to add `WHERE` clauses on metadata columns of the target table)
     - "Search" Button
     - Results Display Area:
       - List of matching items, showing selected columns from the table.
       - Similarity Score for each item.
       - Original vector (optional, for inspection).
   - **User Flow Reference:** `ui_ux_flow.md` - 4.2. Perform Vector Search (UI for testing)
   - **User Story Reference:** "As a developer, I want to perform similarity searches on my vector data via an API..." (UI supports testing this)

### 4.3. RAG Configuration Page
   - **Objective:** Allow users to set up and manage RAG pipelines.
   - **Key Elements:**
     - Title: "RAG Configurations for [Project Name]"
     - "Create New RAG Configuration" Button
     - List of Existing RAG Configurations:
       - Name, Description, Last Modified, Actions (Edit, Delete, Test)
     - **Create/Edit RAG Configuration Form/Modal:**
       - Configuration Name Input
       - Description Text Area
       - Document Source(s) Section:
         - Selector for Table(s)/View(s) containing text and vector data.
         - Text Column Selector (for context snippets)
         - Vector Column Selector (for similarity search)
       - LLM Model Selector (Dropdown of available LLMs, e.g., GPT-3.5, GPT-4)
       - System Prompt Text Area (editable, with placeholders for context)
       - Context Snippet Length (e.g., number of characters/tokens)
       - Number of Matching Documents to Retrieve (for context)
       - "Save Configuration" Button
   - **User Flow Reference:** `ui_ux_flow.md` - 4.3. Configure RAG
   - **User Story Reference:** "As a developer, I want to configure the RAG pipeline..."

### 4.4. RAG Query Test Page/Tool
   - **Objective:** Provide a UI for developers to test RAG queries.
   - **Key Elements:**
     - Title: "RAG Query Playground" or similar
     - RAG Configuration Selector (Dropdown of existing RAG configurations)
     - Natural Language Query Input Text Area
     - "Ask" / "Query" Button
     - Results Display Area:
       - Generated Answer Text Area
       - Sources Used Section (List of document snippets/IDs that contributed to the answer, with similarity scores if applicable)
       - Raw LLM response (optional, for debugging)
   - **User Flow Reference:** `ui_ux_flow.md` - 4.4. Query RAG (UI for testing)
   - **User Story Reference:** "As a developer, I want an API endpoint to send queries to the RAG system..." (UI supports testing this)

## 5. Analytics & Visualization

### 5.1. Analytics - Dashboard List Page
   - **Objective:** Display existing dashboards and allow creation of new ones.
   - **Key Elements:**
     - Project Context Display (e.g., "Project: [Project Name] / Analytics")
     - "New Dashboard" Button
     - Search/Filter Bar for dashboards
     - List of Dashboards:
       - Dashboard Name
       - Description (optional)
       - Last Modified
       - Quick Actions (View, Edit, Settings, Delete, Share/Export options)
   - **User Flow Reference:** `ui_ux_flow.md` - 5.1. Create Dashboard (entry point)
   - **User Story Reference:** "As a developer, I want to create custom dashboards..."

### 5.2. Dashboard View/Edit Page
   - **Objective:** Display a dashboard with its charts and allow editing (adding/configuring charts).
   - **Key Elements (View Mode):**
     - Dashboard Title
     - Grid/Canvas displaying configured charts/visualizations.
     - Refresh Data Button (manual)
     - Time Range Selector (if applicable to multiple charts)
     - Export Dashboard Button (PDF, etc.)
     - "Edit Dashboard" Button (to switch to Edit Mode)
   - **Key Elements (Edit Mode - in addition to View Mode):**
     - "Add Chart/Widget" Button
     - Chart/Widget Placeholders on a draggable/resizable grid.
     - Options to configure individual charts (edit query, appearance, type).
     - Options to rearrange/resize charts.
     - "Save Dashboard" Button
     - "Cancel" / "Exit Edit Mode" Button
   - **User Flow Reference:** `ui_ux_flow.md` - 5.2. Add Chart/Visualization to Dashboard, 5.3. View Dashboard
   - **User Story Reference:** "As a developer, I want to create custom dashboards...", "As a developer, I want dashboards to refresh data automatically..."

### 5.3. Add/Edit Chart/Visualization Modal/Wizard
   - **Objective:** Guide the user through creating or configuring a chart.
   - **Key Elements:**
     - **Step 1: Select Chart Type**
       - Visual selector for chart types (Line, Bar, Pie, Scatter, Table, Number Display, etc.)
     - **Step 2: Define Data Source**
       - Option 1: SQL Query Editor (Monaco Editor, with schema browser for reference)
       - Option 2: Simple Table/Column Selector (for basic charts - select table, then columns for axes/values, aggregation function)
     - **Step 3: Configure Chart Options** (Varies by chart type)
       - Title Input
       - X-Axis Column Selector / Label
       - Y-Axis Column Selector(s) / Label(s)
       - Group By Column Selector (for series)
       - Colors, Legends, Labels configuration
       - Refresh Interval (optional)
     - **Preview Area:** Shows a live preview of the chart as it's configured.
     - "Save Chart" / "Add to Dashboard" Button
     - Cancel/Close Button
   - **User Flow Reference:** `ui_ux_flow.md` - 5.2. Add Chart/Visualization to Dashboard
   - **User Story Reference:** "As a developer, I want to write SQL queries to define the data for my visualizations...", "As a developer, I want to create custom dashboards with various chart types..."

## 6. Cache Management

### 6.1. Cache Management Page
   - **Objective:** Provide tools to view cache statistics and manage cache invalidation.
   - **Key Elements:**
     - Project Context Display (e.g., "Project: [Project Name] / Settings / Cache")
     - **Cache Statistics Section:**
       - Hit Rate (Percentage)
       - Miss Rate (Percentage)
       - Total Cached Entries (Number)
       - Estimated Memory Usage (e.g., MB/GB)
       - Refresh Stats Button
     - **Cache Invalidation Section:**
       - Title: "Invalidate Cache Entries"
       - Input Field for Cache Key Pattern (e.g., `table:public.users:id=123`, `table:public.products:*`)
       - Helper text or examples for key patterns.
       - "Invalidate by Pattern" Button
       - Option: "Invalidate All Cache for this Project" Button (with confirmation dialog)
   - **User Flow Reference:** `ui_ux_flow.md` - 6.1. View Cache Stats, 6.2. Invalidate Cache
   - **User Story Reference:** "As a developer, I want to see cache performance statistics...", "As a developer, I want to be able to configure cache invalidation rules..."

## 7. Developer Experience

### 7.1. API Documentation Page (Integrated)
   - **Objective:** Provide interactive API documentation for the project.
   - **Key Elements (typically using Swagger UI or Redoc):**
     - Project Context Display
     - Navigation Pane (listing API endpoints grouped by tags/resources, e.g., Auth, Tables, RAG)
     - Main Content Area:
       - For each endpoint:
         - HTTP Method and Path (e.g., `GET /rest/v1/{table_name}`)
         - Description
         - Parameters (Path, Query, Header, Body) with types, descriptions, required status.
         - Request Body Schema/Example
         - Response Schemas/Examples for different status codes (200, 400, 401, etc.)
         - "Try it out" functionality to make live API calls (requires auth token input).
     - Download OpenAPI Specification Button
   - **User Flow Reference:** `ui_ux_flow.md` - 7.1. Access API Documentation
   - **User Story Reference:** "As a developer, I want comprehensive API documentation with examples..."

### 7.2. API Keys Management Page
   - **Objective:** Allow users to manage API keys for their project.
   - **Key Elements:**
     - Project Context Display (e.g., "Project: [Project Name] / Settings / API Keys")
     - "Create New API Key" Button
     - List of Existing API Keys:
       - Key Name/Label (user-defined)
       - Key Prefix (e.g., `pb_pub_...`, `pb_sec_...`)
       - Key Type (Public/Anon, Secret/Service Role)
       - Created Date
       - Last Used Date (optional)
       - Actions: Copy Key (reveals full key temporarily for secret keys), Revoke Key (with confirmation)
     - **Create API Key Modal:**
       - Key Name Input
       - Key Type Selector (Public, Secret)
       - (Optional) Scope/Permissions configuration if granular key permissions are supported.
       - "Create Key" Button
       - Display of newly generated key (with a strong warning to copy it now as it won't be shown again for secret keys).
   - **User Flow Reference:** `ui_ux_flow.md` - 7.2. Manage API Keys
   - **User Story Reference:** Implicitly supports all API-related user stories by providing access credentials.

This concludes the initial set of wireframe descriptions for PulseBase.