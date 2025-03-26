# Resource steps properties

You can work with resources to share data at runtime between different running simulations. Use a resource step property in your simulation file to define how a resource is controlled. You can modify the data of a resource or read it.

These are the steps properties you can use:

| Resource steps property | Description |
| ----------------------- | ----------- |
| delete | Clears data in a resource. |
| insert | Pastes data into a resource. |
| read | Reads data from a resource. |
| update | Overwrites a value in a resource. |

## Example - update

This is an example of how to update the data of a resource using the where property:

- The simulation references data from the resource user that contains arbitrary values.
- The simulation contains a service called update.
- The first step of the service waits for an incoming message with a PUT method. If an incoming message meets this condition, the ID of the path is buffered using the XBuffer syntax.
- The second step updates the data in the user resource. Specifically, here we use the where property with an XPath to search for a value that matches the buffered value for the element request.
- In the same step, the message user updated successfully is written to the payload.

```yaml
schema: SimV1

connections:
  - name: http
    port: 8080

resources:
  - name: user
    type: Value

services:
  - name: update
    steps:
      - trigger:
          - property: Method
            value: PUT
        buffer:
          - type: Path
            value: "user/{xb[id]}"
      - resource:
          update:
            - ref: user
              where: '"[?(@.id == "{b[id]}")]"'
              value: "{b[request]}"
        message:
          payload: user updated successfully
```
