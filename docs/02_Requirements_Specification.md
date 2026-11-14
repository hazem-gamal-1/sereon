# Document 02: Requirements Specification

## 1. Functional Requirements

### F01 Register Organization and Sign In

- **FR-01 Register Organization:** A new user creates an organization and becomes its Organization Admin.
- **FR-02 Authenticate User:** A user signs in with email and password and receives a short-lived access credential and a renewable session credential.
- **FR-03 Manage Session:** A user can renew or end their session. Ended or expired sessions cannot be renewed.

### F02 Manage Members and Roles

- **FR-04 Invite Member:** An Organization Admin invites a person by email with an assigned role.
- **FR-05 Change Role:** An Organization Admin changes a member's role among Organization Admin, Knowledge Manager, Employee, and AI Administrator.
- **FR-06 Deactivate Member:** An Organization Admin deactivates a member, which immediately removes their access.

### F03 Manage Collections and Access

- **FR-07 Manage Collections:** Authorized users create, rename, and delete collections, each with an optional department attribute.
- **FR-08 Set Collection Access:** Authorized users grant or revoke member access to a collection.
- **FR-09 Set Document Override:** Authorized users grant or deny access to a single document, overriding its collection's access.

### F04 Upload Document

- **FR-10 Validate and Store Document:** The system accepts PDF, DOCX, TXT, Markdown, and CSV files within the size limit and stores the original.
- **FR-11 Detect Duplicate:** The system detects an identical file already present in the organization and informs the user.
- **FR-12 Process Document Asynchronously:** The system extracts, cleans, chunks, embeds, and indexes the document in the background, with a visible processing status.
- **FR-13 Configure Chunking:** The system applies a configurable chunking strategy (size, overlap, separators) while preserving page and structure metadata.
- **FR-14 Handle Failed Ingestion:** The system retries failed processing automatically and marks it as failed with a reason when retries are exhausted. A user can retry manually.

### F05 Manage Documents

- **FR-15 View Documents:** Users list documents they can access, with metadata and processing status.
- **FR-16 Upload New Version:** Authorized users replace a document with a new version while retaining history.
- **FR-17 Re-index Document:** Authorized users re-process a document, for example after a chunking change.
- **FR-18 Delete Document:** Authorized users delete a document, and its stored file and indexed content are removed.
- **FR-19 Open Original Document:** Users open the original file of any document they are permitted to access.

### F06 Search Knowledge

- **FR-20 Hybrid Search:** The system combines semantic and keyword retrieval and merges the rankings.
- **FR-21 Filter Results:** Users filter by collection, department, document type, and upload date.
- **FR-22 Enforce Access on Retrieval:** Results contain only content from the user's organization and from documents the user may access.
- **FR-23 Rerank Results:** The system reorders an initial candidate set by relevance.
- **FR-24 Transform Query:** The system can rewrite, expand, or split a query into several queries, each switchable by configuration.

### F07 Chat with Knowledge Assistant

- **FR-25 Ask Question:** Users ask natural-language questions in a conversation.
- **FR-26 Answer with Citations:** The assistant answers from retrieved content and attaches citations (document name, ID, page where available, text location) that open the source.
- **FR-27 Express Uncertainty:** When evidence is insufficient, the assistant says so instead of inventing an answer.
- **FR-28 Stream Response:** Answers are delivered progressively.
- **FR-29 Maintain Context:** Follow-up questions use conversation history, summarized when it exceeds the context budget. Memory is limited to the user and organization.
- **FR-30 Collect Feedback:** Users rate answers and add comments.
- **FR-31 Manage Conversations:** Users list, rename, and delete their conversations.

### F08 Run Agentic Task

- **FR-32 Plan Task:** The system turns a complex request into an execution plan.
- **FR-33 Execute Plan:** The system runs research and data-analysis steps using authorized sources and tools.
- **FR-34 Validate Findings:** The system checks that evidence supports conclusions and requests more research when it does not.
- **FR-35 Generate Report:** The system produces a structured report with an executive summary and references.
- **FR-36 Show Progress:** The user sees step status, elapsed time, and concise outputs, without hidden reasoning.
- **FR-37 Persist and Resume Run:** Run state is saved so that interrupted or waiting runs can resume.

### F09 Use Business Tools via MCP

- **FR-38 Expose Tools:** Document, business-data, reporting, and workflow tools are available through MCP.
- **FR-39 Authorize and Audit Tool Calls:** Every tool call is authenticated, authorized in application code, validated, time-limited, and audit-logged.
- **FR-40 Save and Export Reports:** Generated reports can be saved and exported.

### F10 Approve Sensitive Action

- **FR-41 Request Approval:** Sensitive tool actions pause and create an approval request.
- **FR-42 Decide Request:** An authorized approver approves or rejects it, and execution resumes accordingly.
- **FR-43 Cancel Request:** The requester can cancel a pending request. Unanswered requests expire.
- **FR-44 View Approval History:** Users see request status and decisions.

### F11 Monitor AI and Platform Health

- **FR-45 Trace AI Execution:** LLM calls, retrieval steps, tool calls, and graph runs are traced with tokens, latency, and errors.
- **FR-46 Collect Operational Metrics:** API, worker, ingestion, and vector database metrics are collected.
- **FR-47 Display Dashboards:** Dashboards show health, performance, volume, and error rates.
- **FR-48 View Audit Log:** Authorized administrators search the organization's audit records.

### F12 Evaluate Retrieval and Answer Quality

- **FR-49 Manage Evaluation Dataset:** AI Administrators maintain questions, expected documents or chunks, and ground-truth answers.
- **FR-50 Run Evaluation:** The system computes Recall@K, Precision@K, MRR, NDCG, latency, and answer metrics (correctness, faithfulness, relevance, citation correctness).
- **FR-51 Compare Configurations:** Results of different chunking, embedding, retrieval, and reranking configurations can be compared.
- **FR-52 Review Human Feedback:** User feedback is reviewed alongside automated results.

## 2. System Use Cases

### UC-01 Register Organization and Sign In

- **Primary Actor:** Prospective Admin / Employee
- **Goal:** Create an organization or access an existing account.
- **Main Flow:**
  1. The user submits the organization name, email, and password.
  2. The system validates the input and creates the organization, user, and Admin membership.
  3. Later, the user signs in.
  4. The system verifies the credentials and issues access and session credentials.
  5. The system renews the session on request or revokes it on sign-out.
- **Expected Outcome:** The user has an authenticated, tenant-scoped session.

### UC-02 Manage Members and Roles

- **Primary Actor:** Organization Admin
- **Goal:** Control who belongs to the organization and what they may do.
- **Main Flow:**
  1. The admin invites an email with a role.
  2. The system sends an invitation, and the invitee accepts and sets a password.
  3. The admin changes a role or deactivates a member.
  4. The system applies the change and records it in the audit log.
- **Expected Outcome:** Membership and roles match the admin's intent, and deactivated users lose access immediately.

### UC-03 Manage Collections and Access

- **Primary Actor:** Knowledge Manager
- **Goal:** Organize documents and control who can see them.
- **Main Flow:**
  1. The actor creates a collection with a department.
  2. The actor grants members access to the collection.
  3. Optionally, the actor sets a document-level override.
  4. The system saves the rules and audit-logs them.
- **Expected Outcome:** Access rules are enforced for all later searches and chats.

### UC-04 Upload Document

- **Primary Actor:** Knowledge Manager
- **Goal:** Make a document searchable.
- **Main Flow:**
  1. The actor selects a collection and uploads a file.
  2. The system validates the type and size, checks for duplicates, stores the original, and records the document as queued.
  3. The system responds immediately.
  4. A background worker extracts, cleans, chunks, embeds, and indexes the content.
  5. The status moves to ready.
  6. On failure, the worker retries and then marks the document failed with a reason.
- **Expected Outcome:** The document is indexed and ready, or clearly failed with a retry option.

### UC-05 Manage Documents

- **Primary Actor:** Knowledge Manager
- **Goal:** Keep the document library accurate.
- **Main Flow:**
  1. The actor lists accessible documents and statuses.
  2. The actor opens an original, uploads a new version, re-indexes, or deletes.
  3. The system re-processes or removes the stored file, chunks, and vectors as requested.
- **Expected Outcome:** The library, storage, and index stay consistent.

### UC-06 Search Knowledge

- **Primary Actor:** Employee
- **Goal:** Find relevant passages across permitted documents.
- **Main Flow:**
  1. The employee enters a query and optional filters.
  2. The system resolves the collections the user may access.
  3. The system optionally transforms the query.
  4. The system runs semantic and keyword retrieval restricted to the organization and permitted collections, then merges and reranks the results.
  5. The system verifies document-level access and returns the passages with their sources.
- **Expected Outcome:** The employee sees ranked, permitted, source-linked results.

### UC-07 Chat with Knowledge Assistant

- **Primary Actor:** Employee
- **Goal:** Get a trustworthy, cited answer.
- **Main Flow:**
  1. The employee asks a question in a conversation.
  2. The system loads conversation context.
  3. The system decides whether internal knowledge is needed and retrieves it as in UC-06.
  4. The system judges whether the evidence suffices.
  5. If it does, the system streams a grounded answer with citations. If not, it says the evidence is insufficient.
  6. The employee opens a citation or rates the answer.
- **Expected Outcome:** The employee receives a verifiable answer or an honest statement of uncertainty.

### UC-08 Run Agentic Task

- **Primary Actor:** Employee
- **Goal:** Complete a multi-step analysis and obtain a report.
- **Main Flow:**
  1. The employee submits a complex request.
  2. The supervisor creates a plan.
  3. Research and data-analysis steps run.
  4. Validation checks the findings and may trigger more research.
  5. A report is generated.
  6. The UI shows step progress throughout, and state is checkpointed after each step.
- **Expected Outcome:** The employee receives a referenced report, and the run is resumable if interrupted.

### UC-09 Use Business Tools via MCP

- **Primary Actor:** Agent Workflow
- **Goal:** Read business data or perform an operation safely on behalf of a user.
- **Main Flow:**
  1. The workflow calls an MCP tool with the user's identity and organization.
  2. The tool server authenticates, validates input, and authorizes the call.
  3. The tool executes within a timeout.
  4. The result is returned and the call is audit-logged.
- **Expected Outcome:** Authorized calls succeed. Unauthorized or invalid calls fail safely, and all calls are audited.

### UC-10 Approve Sensitive Action

- **Primary Actor:** Approver
- **Goal:** Control sensitive actions.
- **Main Flow:**
  1. A workflow reaches a sensitive tool and pauses.
  2. The system creates an approval request and notifies approvers.
  3. The approver reviews the proposed action.
  4. The approver approves or rejects, or the requester cancels, or the request expires.
  5. The workflow resumes and executes or skips the action accordingly.
- **Expected Outcome:** No sensitive action runs without explicit approval, and the history is recorded.

### UC-11 Monitor AI and Platform Health

- **Primary Actor:** AI Administrator
- **Goal:** Understand reliability, cost, and behavior.
- **Main Flow:**
  1. The services emit traces and metrics.
  2. The administrator opens dashboards.
  3. The administrator inspects a trace or the audit log.
- **Expected Outcome:** The administrator can locate failures, latency, and cost drivers.

### UC-12 Evaluate Retrieval and Answer Quality

- **Primary Actor:** AI Administrator
- **Goal:** Measure and compare system quality.
- **Main Flow:**
  1. The administrator maintains an evaluation dataset.
  2. The administrator starts a run with a chosen configuration.
  3. The runner executes retrieval and generation over the dataset and computes metrics.
  4. The administrator compares runs and reviews user feedback.
- **Expected Outcome:** Configuration choices are backed by measured results.
