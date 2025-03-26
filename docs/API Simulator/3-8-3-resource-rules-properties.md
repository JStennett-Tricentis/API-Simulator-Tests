# Resource rules properties

You can work with resources to share data at runtime between different running simulations. Specify which values you want to reference by using resource rules properties in your simulation file. For example, you can read a specific value from a resource and insert paste it anywhere.

Always specify a rule within the resource property of a service step.

These are the rules properties you can use:

| Rules property | Description | Example |
| -------------- | ----------- | ------- |
| name | This property is mandatory. Define a meaningful and unique name for your resource. | read |
| one | By default, the entire content of a resource is addressed. You can use this property to restrict the values that are to be used. Unlike the where property, only one value is returned (the first value found). Depending on the type of resource, define the following syntax:- Table: Define an SQL WHERE clause.- KeyValue: Enter the key for the expected value.- Value: Enter a constrained path to the desired value. For example, an XPath or JsonPath. | [Example](#example---one) |
| ref | Specify the name of the data resource that you want to reference. This is the name that you defined in the simulation to access a data resource. | user |
| template | Specify this property, if you want to define a pattern for a requested resource. The received data is then structured according to this pattern. You can use any pattern format, such as JSON, XML, etc. Use this property within the resource steps property read. | [Example](#example---template) |
| value | Enter any value. | GET |
| where | By default, the entire content of a resource is addressed. This property allows you to restrict the values that should be used. Depending on the type of resource, define the following syntax:- Table: Define an SQL WHERE clause.- KeyValue: Enter the key for the expected value.- Value: Enter a constrained path to the desired value. For example, an XPath or JsonPath. | [Example](#example---where) |

## Example - one

This is an example of using data from a resource that is read with the one property:

- The simulation references data from an SQLite table. The resource user defines where the table is located.
- The simulation contains a service called get.
- The first step of the service waits for an incoming message with a GET method. If an incoming message meets this condition, the ID of the path is buffered using the XBuffer syntax.
- The second step reads data from the user resource. Specifically, here we use the one property with an SQL clause to read the first ID found.
- In the same step, the value found is inserted into the resource in the result column.

```yaml
schema: SimV1

connections:
  - name: http
    port: 8080

resources:
  - name: user
    file: user.sqlite

services:
  - name: get
    steps:
      - trigger:
          - property: Method
            value: GET
        buffer:
          - type: Path
            value: "user/{xb[id]}"
      - resource:
          read:
            - ref: user
              name: result
              one: "id == '{b[id]}'"
        insert:
          - value: "{b[result.name]}"
```

## Example - where

This is an example of how to update the data of a resource using the where property:

- The simulation references data of the resource user that contains arbitrary values.
- The simulation contains a service called update.
- The first step of the service waits for an incoming message with a PUT method. If an incoming message meets this condition, the ID of the path is buffered using the XBuffer syntax.
- The second step updates the data in the user resource. Specifically, here we use the where property with an XPath to search for a value that matches the buffered value for the element request.
- Within the same step, the value user updated successfully is written to the payload.

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

## Example - template

This is an example of how to structure the content of a requested resource.

- The simulation references data of the resource user that contains arbitrary values.
- The simulation contains a service called update.
- The first step of the service waits for an incoming message with a GET method. If an incoming message meets this condition, the service reads all users from the users resource into a buffer named results. The resource property defines the structure of the data in XML format.
- The second step writes the data from the results buffer to the response payload.

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
      - name: request users
          trigger:
          - property: Method
            value: GET
        resource:
          read:
            - ref: user
              name: results
              template:
                <user>
                  <id>%{results.id}</id>
                  <n>%{results.name}</n>
                </user>
      - name: return list of users
        - insert:
          - value: <users>{b[results]}</users>
```
