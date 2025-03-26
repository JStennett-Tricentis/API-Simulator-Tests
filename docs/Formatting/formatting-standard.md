# ServiceFusion API Simulator Formatting Standards

This document defines the standard formatting for all ServiceFusion API Simulator YAML files.

## YAML Structure

All simulation files should follow this structure:

```yaml
schema: SimV1
name: serviceName

# Optional resources section
resources:
  - name: resourceName
    type: resourceType
    file: path/to/resource.file

# Templates section (if needed)
templates:
  connections:
    - name: templateName
      type: ConnectionType
      # other template properties

connections:
  - name: connectionName
    port: portNumber
    # other connection properties

services:
  - name: serviceName
    description: Brief description of service purpose.
    steps:
      - direction: In
        name: receiveRequestStepName
        trigger:
          - type: Path
            value: /api/endpoint
        # other step properties

      - direction: Out
        name: sendResponseStepName
        message:
          payload: |
            {"key": "value"}
        # other step properties
```

## Formatting Rules

1. **Headers and Structure**
   - Always include `schema: SimV1` at the top
   - Always provide a descriptive `name` for the simulator
   - Use the sections in the order: resources, templates, connections, services

2. **Naming Conventions**
   - Use camelCase for all names (services, steps, connections, resources)
   - Connection names should indicate purpose and type (e.g., workspacesApi)
   - Step names should clearly indicate their purpose (e.g., receiveWorkspacesRequest, sendWorkspacesResponse)

3. **Steps Format**
   - Always include a `name` for each step
   - Always include `direction: In` or `direction: Out` for each step
   - **Service Files**
     - Do NOT include `from` or `to` in service definitions
     - Do NOT include `type: Method` in trigger definitions - use Path patterns instead
   - **Test Files**
     - ALWAYS include `from` for incoming steps, `to` for outgoing steps
     - You CAN use `type: Method` in insert definitions
   - Group related steps together (request/response pairs)

4. **Indentation**
   - Use 2-space indentation consistently
   - Align similar properties vertically for readability

5. **Comments**
   - Add comments for complex triggers or business logic
   - Include comments for any non-obvious data manipulation

6. **Headers and Content Types**
   - In general, include only necessary headers:
     - `Server` for response headers
     - `Authorization` for authentication
     - `Accept` for content negotiation
   - All request/response payloads should be JSON formatted
   - Avoid unnecessary headers like request-id, traceparent, etc.

7. **Service Integration**
   - When services communicate with each other:
     - Use buffer variables to pass data between steps
     - Include proper error handling
     - Document dependencies between services

## Examples

### Service File Example (without -test suffix)

```yaml
schema: SimV1
name: toscaCloudWorkspaces

connections:
  - name: workspacesApi
    port: 20001

services:
  - name: workspacesService
    description: Simulates the Tosca Cloud Workspaces API that returns available workspaces.
    steps:
      - direction: In
        name: receiveWorkspacesRequest
        trigger:
          - type: Path
            value: /_core/api/organization/v1/spaces

      - direction: Out
        name: sendWorkspacesResponse
        message:
          headers:
            - key: Server
              value: Kestrel
          payload: |
            [
              {
                "id": "default",
                "name": "Default",
                "path": "Default",
                "description": "Default workspace of the tenant."
              },
              {
                "id": "95e9d173-8cac-482a-a62f-c5a6483daca6",
                "name": "Samples",
                "path": "Samples"
              }
            ]
```

### Test File Example (with -test suffix)

```yaml
schema: SimV1
name: toscaCloudWorkspacesTest

connections:
  - name: workspacesApi
    endpoint: http://localhost:20001
    listen: false

services:
  - name: testWorkspacesApiBasic
    description: Basic test for the Tosca Cloud Workspaces API.
    steps:
      - direction: Out
        name: sendWorkspacesRequest
        to: workspacesApi
        insert:
          - type: Path
            value: /_core/api/organization/v1/spaces

      - direction: In
        name: verifyWorkspacesResponse
        from: workspacesApi
        verify:
          - jsonPath: $[0].id
            value: default
          - jsonPath: $[0].name
            value: Default
          - jsonPath: $[1].id
            value: 95e9d173-8cac-482a-a62f-c5a6483daca6
```

### Service with Buffer Example

```yaml
schema: SimV1
name: authorizationService

connections:
  - name: authApi
    port: 21001

services:
  - name: tokenService
    description: Handles token generation and verification.
    steps:
      - direction: In
        name: receiveTokenRequest
        trigger:
          - type: Path
            value: /api/v1/token
        buffer:
          - name: userId
            jsonPath: $.userId
          - name: sessionId
            value: "{RANDOMTEXT[10]}"

      - direction: Out
        name: sendTokenResponse
        message:
          payload: |
            {
              "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9",
              "sessionId": "{B[sessionId]}",
              "userId": "{B[userId]}"
            }
```
