# TrunkSubmittal Project Plan

## 1. System Overview

TrunkSubmittal is a B2B web application designed to assist construction designers in reviewing contractor submittals against authoritative base documents (e.g., specifications, drawings). The system leverages AI to identify concerns, suggest responses, and classify issues using RAG (Retrieval-Augmented Generation) analysis. Designers retain full authority over decisions, and all AI outputs are explainable, auditable, and source-cited.

---

## 2. Milestone Breakdown (Phase 0 → Phase 5)

### **Phase 0: Foundation**
- Authentication & Role Management
- Core Data Models
- Project & User Management

### **Phase 1: Designer Portal**
- Base Document Upload & Management
- Document Versioning & Metadata
- Active Baseline Concept

### **Phase 2: Document Ingestion Pipeline**
- Text Extraction, Chunking, and Metadata Enrichment
- Embedding Generation & Vector DB Integration
- Failure Handling & Retry Logic

### **Phase 3: Contractor Proposal Submission**
- Proposal Upload & Validation
- Proposal Lifecycle Management
- Read-only Rules Post-Submission

### **Phase 4: RAG-Based Comparison Engine**
- Proposal Normalization
- Relevant Spec Retrieval
- Concern Generation & Severity Classification
- Explainability & Confidence Scoring

### **Phase 5: Human-in-the-Loop Review**
- Designer Review Dashboard
- Concern Presentation & Decision Options
- AI-Assisted Comment Generation
- Bulk Actions with Safeguards

### **Phase 6: Contractor Feedback Loop**
- Feedback Presentation to Contractors
- Resubmission Rules & Revision Tracking

### **Phase 7: Audit Trail & Legal Safety**
- Logging of All Actions & Decisions
- Auditability for Legal Protection

---

## 3. Detailed Goals Per Phase

### **Phase 0: Foundation**
- **Authentication & Roles**: Implement Designer and Contractor roles with role-based access control (RBAC).
- **Core Data Models**: Define entities like `User`, `Project`, `BaseDocument`, `Proposal`, etc.
- **Project Management**: Allow Designers to create and manage projects.

### **Phase 1: Designer Portal**
- **Base Document Upload**: Support PDF/DOCX uploads with metadata input.
- **Versioning**: Track document versions and enforce the "active baseline" concept.
- **Preview & Verification**: Provide a UI for Designers to verify extracted content.

### **Phase 2: Document Ingestion Pipeline**
- **Text Extraction**: Use `pdfplumber`/`docx` for parsing.
- **Chunking**: Split documents into logical sections (e.g., headings, paragraphs).
- **Embedding**: Generate embeddings using GPT-class models and store in a vector DB.
- **Failure Handling**: Log and retry failed extractions.

### **Phase 3: Contractor Proposal Submission**
- **Proposal Upload**: Allow Contractors to upload proposals with metadata.
- **Validation**: Ensure file format and metadata completeness.
- **Lifecycle Management**: Track proposal status (e.g., Draft, Submitted, Reviewed).

### **Phase 4: RAG-Based Comparison Engine**
- **Spec Retrieval**: Query vector DB for relevant base document sections.
- **Comparison Logic**: Use LLMs to compare proposals against specs.
- **Concern Generation**: Identify mismatches and classify severity (RAG).
- **Explainability**: Provide source-cited justifications for all concerns.

### **Phase 5: Human-in-the-Loop Review**
- **Review Dashboard**: Display concerns with decision options (Approve/Reject/Comment).
- **AI-Assisted Comments**: Suggest responses for Designer review.
- **Bulk Actions**: Enable batch processing with safeguards.

### **Phase 6: Contractor Feedback Loop**
- **Feedback Presentation**: Show Contractors structured feedback.
- **Resubmission Rules**: Allow revisions while tracking changes.

### **Phase 7: Audit Trail & Legal Safety**
- **Logging**: Record all actions, decisions, and AI outputs.
- **Auditability**: Ensure logs are immutable and accessible for legal review.

---

## 4. Data Models Per Phase

### **Core Models**
1. **User**: `id`, `role`, `email`, `password_hash`
2. **Project**: `id`, `name`, `owner_id`, `created_at`
3. **BaseDocument**: `id`, `project_id`, `version`, `status`, `metadata`
4. **DocumentVersion**: `id`, `base_document_id`, `content`, `created_at`
5. **Proposal**: `id`, `project_id`, `contractor_id`, `status`, `metadata`
6. **ProposalDocument**: `id`, `proposal_id`, `content`, `created_at`
7. **Chunk**: `id`, `document_id`, `content`, `embedding`
8. **Concern**: `id`, `proposal_id`, `chunk_id`, `severity`, `explanation`
9. **DecisionLog**: `id`, `concern_id`, `decision`, `comment`, `timestamp`

---

## 5. API Surface (High-Level)

### **Authentication**
- `POST /auth/login`
- `POST /auth/register`

### **Project Management**
- `GET /projects`
- `POST /projects`
- `GET /projects/{id}`

### **Document Management**
- `POST /projects/{id}/base-documents`
- `GET /projects/{id}/base-documents`

### **Proposal Management**
- `POST /projects/{id}/proposals`
- `GET /projects/{id}/proposals`

### **RAG Analysis**
- `POST /proposals/{id}/analyze`

### **Review & Feedback**
- `GET /proposals/{id}/concerns`
- `POST /concerns/{id}/decision`

---

## 6. RAG Flow Diagram (Textual)

1. **Input**: Contractor uploads proposal.
2. **Normalize**: Extract and chunk proposal content.
3. **Retrieve**: Query vector DB for relevant base document chunks.
4. **Compare**: Use LLM to compare proposal chunks with specs.
5. **Generate Concerns**: Identify mismatches and classify severity (RAG).
6. **Explain**: Provide source-cited justifications.
7. **Output**: Present concerns to Designer for review.

---

## 7. Failure & Edge Case Matrix

| **Failure Mode**                | **Mitigation**                                      |
|----------------------------------|----------------------------------------------------|
| Text extraction fails            | Retry logic, manual upload option                  |
| Low confidence in comparison     | Flag for manual review                             |
| Missing metadata                 | Enforce validation at upload                       |
| Ambiguous AI output              | Require Designer override                         |
| Proposal resubmission abuse      | Limit number of resubmissions                     |

---

## 8. Build Order & Dependencies

1. **Phase 0**: Foundation (Authentication, Core Models)
2. **Phase 1**: Designer Portal (Base Document Management)
3. **Phase 2**: Document Ingestion Pipeline
4. **Phase 3**: Contractor Proposal Submission
5. **Phase 4**: RAG-Based Comparison Engine
6. **Phase 5**: Human-in-the-Loop Review
7. **Phase 6**: Contractor Feedback Loop
8. **Phase 7**: Audit Trail

---

## 9. What We Build First (Day 1–3)

1. **Authentication & Roles**: Basic login, registration, and RBAC.
2. **Core Models**: Define `User`, `Project`, and `BaseDocument`.
3. **Project Management**: Allow Designers to create projects.

---

## 10. What We Explicitly Postpone

- **Analytics & Reporting**: Not part of MVP.
- **Billing & Admin Features**: Out of scope.
- **Advanced AI Features**: Focus on explainability, not automation.
- **Non-Core Personas**: Ignore GC and other roles.