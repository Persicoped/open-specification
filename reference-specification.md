# OpenSequence Reference Specification

This specification defines how references are handled within OpenSequence messages. References provide links to both internal messages and external systems, enabling traceability and rich context for workflow sequences.

## Core Components

### `reference`
- **Description**: Defines a reference to an internal or external resource
- **Purpose**: Establishes relationships and dependencies between resources
- **Example**:
    ```yaml
    reference:
      id: "PROJ-123"
      type: "jira"
      created_at: "2024-01-28T15:30:00Z"
      title: "Implement OAuth2 Authentication"
      relationship:
        type: "blocking"
        status: "active"
        weight: 3
      metadata:
        description: "Authentication implementation required before proceeding"
        last_sync: "2024-01-28T15:35:00Z"
        external_status: "In Progress"
        owner: "jane.doe"
        labels: ["security", "authentication"]
      validation:
        state: "valid"
        last_checked: "2024-01-28T15:35:00Z"
    ```

### `reference_config`
- **Description**: Configuration for allowed reference types
- **Purpose**: Defines valid reference patterns and behaviors
- **Example**:
    ```yaml
    reference_config:
      allowed_types:
        - type: "jira"
          pattern: "^[A-Z]+-\d+$"
          url_template: "https://organization.atlassian.net/browse/{ref}"
          sync:
            mode: "two_way"
            interval: "PT15M"
            fields: ["title", "status", "assignee"]
        - type: "github"
          pattern: "^GH-\d+$"
          url_template: "https://github.com/org/repo/issues/{ref}"
          sync:
            mode: "one_way"
            interval: "PT1H"
            fields: ["title", "state"]
        - type: "message"
          pattern: "^msg-[a-f0-9]+$"
          internal: true
          sync:
            mode: "realtime"
    ```

### Relationship Types
```yaml
relationship_types:
  - blocking      # This reference blocks progress
  - blocked_by    # Progress is blocked by this reference
  - related       # General relationship
  - parent        # This reference is a parent
  - child         # This reference is a child
  - duplicate     # This reference is a duplicate
```

### Validation States
```yaml
validation_states:
  - valid         # Reference exists and is accessible
  - invalid       # Reference no longer exists
  - inaccessible  # Temporary inability to validate
  - archived      # Preserved despite being invalid
```

## Synchronization Rules

### Sync Events
1. **Creation Sync**
   ```yaml
   sync_event:
     trigger: "creation"
     actions:
       - validate_reference
       - fetch_metadata
       - store_initial_state
   ```

2. **State Change Sync**
   ```yaml
   sync_event:
     trigger: "state_change"
     actions:
       - validate_reference
       - update_metadata
       - check_dependencies
   ```

3. **Periodic Sync**
   ```yaml
   sync_event:
     trigger: "periodic"
     interval: "PT1H"
     actions:
       - validate_all_references
       - update_metadata
       - report_status
   ```

### Sync Modes
1. **One-way Sync**
    - Read-only from external system
    - Local reference metadata updates only
    - No pushback to external system

2. **Two-way Sync**
    - Bidirectional updates
    - Conflict resolution required
    - Change tracking enabled

3. **Manual Sync**
    - Explicit sync requests only
    - No automatic updates
    - User-triggered synchronization

## Error Handling

### Validation Errors
```yaml
error:
  code: "invalid_reference"
  message: "Referenced JIRA ticket no longer exists"
  details:
    reference_id: "PROJ-123"
    type: "jira"
    last_valid: "2024-01-27T10:00:00Z"
  actions:
    - archive_reference
    - notify_owner
    - log_event
```

### Sync Errors
```yaml
error:
  code: "sync_failed"
  message: "Unable to sync with external system"
  details:
    system: "jira"
    status_code: 503
    retry_count: 3
  actions:
    - mark_inaccessible
    - retry_later
    - alert_admin
```

## Best Practices

1. **Reference Creation**
    - Validate references immediately upon creation
    - Ensure reference pattern matches configured type
    - Fetch initial metadata when available
    - Set appropriate relationship type

2. **Sync Management**
    - Implement exponential backoff for failed syncs
    - Cache external system responses
    - Respect rate limits
    - Log all sync activities

3. **Reference Maintenance**
    - Regular validation of all references
    - Clean up or archive invalid references
    - Update relationship types as needed
    - Maintain sync history

4. **Security Considerations**
    - Validate URL templates
    - Sanitize reference inputs
    - Respect external system permissions
    - Audit reference access

## Schema Evolution

1. **Version Control**
    - Track reference schema versions
    - Support multiple reference formats
    - Provide migration tools

2. **Backwards Compatibility**
    - Maintain support for older reference formats
    - Document deprecated fields
    - Provide upgrade paths