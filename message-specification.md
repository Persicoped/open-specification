# Message Object Specification
  
  This document provides a detailed explanation of the **message object** fields used in the openSequence project. Each field is essential for ensuring clarity, traceability, and effective communication.

## Fields

### `action`
- **Description**: Defines the specific action to be taken.
- **Purpose**: Provides clarity on the task or initiative being communicated.
- **Example**:
    ```yaml
    action: "Update the API endpoint to support pagination"
    ```

### `why`
- **Description**: Explains the reasoning or motivation behind the action.
- **Purpose**: Ensures the recipient understands the importance and context of the task.
- **Example**:
    ```yaml
    why: "To improve API performance and user experience for large datasets"
    ```

### `references`
- **Description**: A list of links, documents, or other resources relevant to the task.
- **Purpose**: Provides supporting materials to assist with execution or decision-making.
- **Example**:
    ```yaml
    references:
      - "https://docs.example.com/pagination"
      - "Internal API Spec: /docs/api/v1"
    ```

### `estimated_time`
- **Description**: Estimated time required to complete the task (in hours).
- **Purpose**: Helps with prioritization and resource planning.
- **Example**:
    ```yaml
    estimated_time: 4
    ```

### `complete_time`
- **Description**: The timestamp when the task is completed.
- **Purpose**: Tracks task progress and completion history.
- **Format**: ISO 8601 (e.g., `2025-01-21T14:30:00Z`).
- **Example**:
    ```yaml
    complete_time: "2025-01-21T18:00:00Z"
    ```

### `definition_of_done`
- **Description**: Criteria defining when a task is considered complete.
- **Purpose**: Ensures clarity and alignment on deliverables.
- **Example**:
    ```yaml
    definition_of_done:
      - "All tests pass"
      - "Reviewed and approved by team lead"
      - "Deployed to staging"
    ```

### `risk_score`
- **Description**: A numerical score (1-10) assessing the level of risk associated with the task.
- **Purpose**: Evaluates potential impact or failure likelihood.
- **Example**:
    ```yaml
    risk_score: 7
    ```

### `efficiency_score`
- **Description**: A numerical score (1-10) representing the efficiency of completing the task.
- **Purpose**: Highlights the expected value or ROI of the effort.
- **Example**:
    ```yaml
    efficiency_score: 9
    ```

### `cost_to_implement`
- **Description**: Estimated financial cost of completing the task.
- **Purpose**: Provides budget awareness and facilitates cost-benefit analysis.
- **Example**:
    ```yaml
    cost_to_implement: 500.00
    ```

### `communication_medium`
- **Description**: Preferred communication channel for discussing or managing the task.
- **Purpose**: Aligns on the best way to collaborate and reduce delays.
- **Allowed Values**:
              - `slack`
              - `text`
              - `email`
              - `phone`
- **Example**:
    ```yaml
    communication_medium: "slack"
    ```

---

This specification ensures consistent task management and transparency across the openSequence project. Each field plays a vital role in maintaining efficient workflows and clear communication.
