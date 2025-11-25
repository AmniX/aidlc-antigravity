---
description: AIDLC Adaptive Software Development Workflow (Self-Contained)
---

# AIDLC Adaptive Workflow

This workflow guides you through the AI Development Life Cycle (AIDLC). It is designed to be adaptive, meaning you should only execute the stages that are relevant to the current task.

**Agent Instructions**:
-   **Task Tracking**: You MUST use the `task_boundary` tool to track your progress. Call it at the start of each major step or phase.
-   **User Communication**: You MUST use the `notify_user` tool when you need to ask questions, request reviews, or get approval. Do NOT use standard chat messages for these critical interactions during the workflow.
-   **Audit Trail**: Log ALL user interactions, prompts, and approvals in `aidlc-docs/audit.md`.
-   **State Tracking**: Keep `aidlc-docs/aidlc-state.md` updated with current progress.

## Phase 1: Inception

### Step 1.1: Initialization and Welcome
1.  **Start Task**: Call `task_boundary` with `TaskName="AIDLC Initialization"`.
2.  **Display Welcome Message**:
    - Display the following message to the user:
      ```markdown
      # 👋 Welcome to AI-DLC (AI-Driven Development Life Cycle)! 👋

      I'll guide you through an adaptive software development workflow that intelligently tailors itself to your specific needs.

      ## The Three-Phase Lifecycle
      1. **INCEPTION PHASE**: Planning & Application Design (Workspace Detection, Requirements, Planning)
      2. **CONSTRUCTION PHASE**: Design, Implementation & Test (Detailed Design, Code Gen, Build & Test)
      3. **OPERATIONS PHASE**: Deployment & Monitoring (Placeholder)

      ## Key Principles:
      - ⚡ **Fully Adaptive**: Each stage independently evaluated based on your needs
      - 🎯 **Efficient**: Simple changes execute only essential stages
      - 📋 **Comprehensive**: Complex changes get full treatment
      - 🔍 **Transparent**: You see and approve the execution plan before work begins

      Let's begin by analyzing your workspace!
      ```

### Step 1.2: Workspace Detection (ALWAYS)
1.  **Start Task**: Call `task_boundary` with `TaskName="Workspace Detection"`.
2.  **Check for Existing Project**: Check if `aidlc-docs/aidlc-state.md` exists. If yes, resume from last state.
3.  **Scan Workspace**: Check for existing code, build files, and project structure. Use `list_dir` and `find_by_name` tools.
4.  **Determine Project Type**:
    - **Brownfield**: Existing code found.
    - **Greenfield**: No existing code.
5.  **Create/Update State**: Create `aidlc-docs/aidlc-state.md` with project type and start date.
6.  **Log Findings**: Log findings in `aidlc-docs/audit.md`.

### Step 1.3: Reverse Engineering (CONDITIONAL - Brownfield Only)
1.  **Condition**: Execute ONLY if project is **Brownfield** AND no previous reverse engineering artifacts exist.
2.  **Start Task**: Call `task_boundary` with `TaskName="Reverse Engineering"`.
3.  **Execution**:
    - **Analyze Codebase**: Scan packages, infrastructure, build system, and APIs.
    - **Generate Artifacts**: Create the following files in `aidlc-docs/inception/reverse-engineering/`:
        - `business-overview.md`: Business context and transactions.
        - `architecture.md`: System overview and diagrams.
        - `code-structure.md`: Class/module hierarchy.
        - `api-documentation.md`: REST and internal APIs.
        - `component-inventory.md`: List of packages.
        - `technology-stack.md`: Languages and frameworks.
        - `dependencies.md`: Internal and external dependencies.
    - **Review**: Use `notify_user` to ask the user to review the artifacts.

### Step 1.4: Requirements Analysis (ALWAYS)
1.  **Start Task**: Call `task_boundary` with `TaskName="Requirements Analysis"`.
2.  **Analyze Request**: Determine request clarity, type, scope, and complexity.
3.  **Determine Depth**:
    - **Minimal**: Simple requests.
    - **Standard**: Normal complexity.
    - **Comprehensive**: High risk/complexity.
4.  **Clarify**: If requirements are unclear, create `aidlc-docs/inception/requirements/requirement-verification-questions.md` and use `notify_user` to ask the user.
5.  **Document**: Create `aidlc-docs/inception/requirements/requirements.md` with functional and non-functional requirements.
6.  **Update State**: Mark Requirements Analysis as complete in `aidlc-docs/aidlc-state.md`.

### Step 1.5: User Stories (CONDITIONAL)
1.  **Condition**: Execute if:
    - New user-facing features.
    - Changes affecting user workflows.
    - Multiple user personas.
    - Complex business requirements.
2.  **Start Task**: Call `task_boundary` with `TaskName="User Stories"`.
3.  **Execution**:
    - **Plan**: Create `aidlc-docs/inception/plans/story-generation-plan.md`. Ask clarifying questions about personas and story granularity using `notify_user`.
    - **Generate**: Create `aidlc-docs/inception/user-stories/stories.md` and `personas.md`.
    - **Review**: Use `notify_user` to get user approval.

### Step 1.6: Workflow Planning (ALWAYS)
1.  **Start Task**: Call `task_boundary` with `TaskName="Workflow Planning"`.
2.  **Analyze Context**: Load requirements, stories, and reverse engineering artifacts.
3.  **Determine Phases**: Decide which of the following stages to execute:
    - Application Design
    - Units Generation
    - Functional Design (per unit)
    - NFR Requirements (per unit)
    - NFR Design (per unit)
    - Infrastructure Design (per unit)
4.  **Create Plan**: Create `aidlc-docs/inception/plans/execution-plan.md` listing all stages to Execute or Skip with rationale.
5.  **Visualize**: Create a Mermaid diagram of the workflow.
6.  **Review**: Use `notify_user` to present the plan to the user for approval.

### Step 1.7: Application Design (CONDITIONAL)
1.  **Condition**: Execute if new components, services, or complex interactions are needed.
2.  **Start Task**: Call `task_boundary` with `TaskName="Application Design"`.
3.  **Execution**:
    - **Plan**: Create `aidlc-docs/inception/plans/application-design-plan.md`.
    - **Generate**: Create artifacts in `aidlc-docs/inception/application-design/`:
        - `components.md`: Component definitions.
        - `services.md`: Service layer design.
        - `component-dependency.md`: Interactions.
    - **Review**: Use `notify_user` to get user approval.

### Step 1.8: Units Generation (CONDITIONAL)
1.  **Condition**: Execute if system needs decomposition into multiple units of work (e.g., microservices or modules).
2.  **Start Task**: Call `task_boundary` with `TaskName="Units Generation"`.
3.  **Execution**:
    - **Plan**: Create `aidlc-docs/inception/plans/unit-of-work-plan.md`.
    - **Generate**: Create artifacts in `aidlc-docs/inception/application-design/`:
        - `unit-of-work.md`: Unit definitions.
        - `unit-of-work-story-map.md`: Mapping stories to units.
    - **Review**: Use `notify_user` to get user approval.

## Phase 2: Construction

**Loop**: Perform the following steps for **EACH** unit defined in the plan.

### Step 2.1: Functional Design (CONDITIONAL)
1.  **Condition**: Execute if complex business logic or new domain models are needed.
2.  **Start Task**: Call `task_boundary` with `TaskName="Functional Design - {unit}"`.
3.  **Execution**:
    - **Plan**: Create `aidlc-docs/construction/plans/{unit}-functional-design-plan.md`.
    - **Generate**: Create artifacts in `aidlc-docs/construction/{unit}/functional-design/`:
        - `business-logic-model.md`
        - `domain-entities.md`
        - `business-rules.md`
    - **Review**: Use `notify_user` to get user approval.

### Step 2.2: NFR Requirements (CONDITIONAL)
1.  **Condition**: Execute if performance, security, or scalability requirements exist.
2.  **Start Task**: Call `task_boundary` with `TaskName="NFR Requirements - {unit}"`.
3.  **Execution**:
    - **Plan**: Create `aidlc-docs/construction/plans/{unit}-nfr-requirements-plan.md`.
    - **Generate**: Create artifacts in `aidlc-docs/construction/{unit}/nfr-requirements/`.
    - **Review**: Use `notify_user` to get user approval.

### Step 2.3: NFR Design (CONDITIONAL)
1.  **Condition**: Execute if NFR patterns need to be designed.
2.  **Start Task**: Call `task_boundary` with `TaskName="NFR Design - {unit}"`.
3.  **Execution**:
    - **Plan**: Create `aidlc-docs/construction/plans/{unit}-nfr-design-plan.md`.
    - **Generate**: Create artifacts in `aidlc-docs/construction/{unit}/nfr-design/`.
    - **Review**: Use `notify_user` to get user approval.

### Step 2.4: Infrastructure Design (CONDITIONAL)
1.  **Condition**: Execute if infrastructure changes are needed.
2.  **Start Task**: Call `task_boundary` with `TaskName="Infrastructure Design - {unit}"`.
3.  **Execution**:
    - **Plan**: Create `aidlc-docs/construction/plans/{unit}-infrastructure-design-plan.md`.
    - **Generate**: Create artifacts in `aidlc-docs/construction/{unit}/infrastructure-design/`.
    - **Review**: Use `notify_user` to get user approval.

### Step 2.5: Code Generation (ALWAYS)
1.  **Start Task**: Call `task_boundary` with `TaskName="Code Generation - {unit}"`.
2.  **Execution**:
    - **Plan**: Create `aidlc-docs/construction/plans/{unit}-code-generation-plan.md` with explicit, numbered steps.
    - **Approve**: Use `notify_user` to get user approval for the plan.
    - **Generate**: Execute the plan step-by-step.
        - Mark steps as complete `[x]` as you go.
        - Generate code, tests, and config files.
    - **Review**: Use `notify_user` to ask user to review generated code.

### Step 2.6: Build and Test (ALWAYS)
1.  **Start Task**: Call `task_boundary` with `TaskName="Build and Test"`.
2.  **Execution**:
    - **Instructions**: Create build and test instructions in `aidlc-docs/construction/build-and-test/`.
    - **Execute**: Run builds and tests (Unit, Integration, Performance).
    - **Summary**: Create `aidlc-docs/construction/build-and-test/build-and-test-summary.md`.
    - **Review**: Use `notify_user` to present results to user.

## Phase 3: Operations (Placeholder)

### Step 3.1: Operations
1.  **Start Task**: Call `task_boundary` with `TaskName="Operations"`.
2.  **Status**: Placeholder for future deployment workflows.
3.  **Action**: Log completion of Construction phase and readiness for deployment.

### Step 3.2: Project Finalization (ALWAYS)
1.  **Start Task**: Call `task_boundary` with `TaskName="Project Finalization"`.
2.  **Check Template**: Check if `README.md` contains "AIDLC for Antigravity".
3.  **Execution**:
    -   **Archive**: Move the existing `README.md` to `aidlc-docs/AIDLC_TEMPLATE_README.md`.
    -   **Generate**: Create a new `README.md` tailored to the created project.
        -   **Context**: Use `requirements.md`, design specs, and the actual code to write a comprehensive README.
        -   **Content**:
            -   **Title & Description**: Clear overview of the project.
            -   **Features**: List key features implemented.
            -   **Tech Stack**: Languages and frameworks used.
            -   **Installation & Usage**: Steps from `build-and-test-summary.md`.
            -   **Project Structure**: Brief overview of key files.
            -   **License**: Maintain existing license.
    -   **Review**: Use `notify_user` to ask user to review the new README.

## General Rules
-   **Audit Trail**: Log ALL user interactions, prompts, and approvals in `aidlc-docs/audit.md`.
-   **State Tracking**: Keep `aidlc-docs/aidlc-state.md` updated with current progress.
-   **User Approval**: Always wait for explicit user approval before moving between major stages or generating code.
-   **Checkboxes**: Use checkboxes `[ ]` -> `[x]` to track progress in plan files.
-   **Question Format**: When asking questions in plan files, DO NOT use checkboxes for options. Use the following format:
    ```
    Q: [Question Text]
    Context: [Optional: Why this is being asked / Impact of choice]
    Options:
    A) [Option A]
    B) [Option B]
    ...
    Answer: [User Input] (Default: [Option])
    ```
    - **Multi-Select**: If multiple answers are allowed, specify "(Select all that apply)".
    - **Validation**: You MUST validate the user's answer. If they provide an invalid option, ask again.
