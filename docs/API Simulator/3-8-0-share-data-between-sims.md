# TODO: Find the rest

## Reference to a resource with access data

To use data from a data source, you must reference the resource with the appropriate properties to access it.

Define your simulation as follows:

- Enter the resource property in the appropriate step to indicate that a data source is being accessed.
- Specify whether to read, or modify data from the data source. Valid properties are: delete, insert, read, and update.
- Optionally, you can specify one of the resource rule properties to further specify the data to use.
- Alternatively, you can use the FROM expression to access data from a resource.

### Example

Here's an example of accessing data from a data source:

- The name of the simulation is read customer and it contains a service called read.
- The first step of the service reads data from the resource called user. To access this data later, we give it the name list.
- The second step inserts the values from the read step by using the buffer syntax.

```yaml
schema: SimV1
name: read customer

services:
  - name: read
    steps:
    - resource:
        read:
        - ref: user
          name: list
    - insert:
      - value: '{B[list]}'
```
