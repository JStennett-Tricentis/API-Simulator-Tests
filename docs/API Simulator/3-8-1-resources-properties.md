# Resources properties

You can work with resources to share data at runtime between different running simulations. Use resources properties in your simulation file to reference a resource, such as a data structure.

You can use the following resources propertiesto define the structure of data sources:

| Resources property | Description | Example |
| ------------------ | ----------- | ------- |
| file | Path to the referenced resource file. Supported file formats are SQlite and CSV. If you don't specify a file, the default is in-memory. | /data/users.csv |
| listEntrySeparator | By default, entries in a resource are separated by a comma. Set this property to change the separator. | , |
| listPrefix | Left square brackets start a line of a data string. Set this property to change the prefix. | [ |
| listPostfix | Right square brackets end a line of a data string. Set this property to change the postfix. | ] |
| listSeparator | \n delimits a data string line. Set this property to change the delimiter. | \n |
| name | The name of the resource. This property is mandatory. | Users |
| properties | Specify this property if the column names of a file are not defined. Set a list of properties and use column and data string delimiters. | [name, age] |
| table | If you reference a database in your resource, specify the name of the table that you want to use. You can only reference one table per resource. | Customers |
| type | Depending on the structure of the data resource, the following options are available:- KeyValue identifies each entry in the resource by a single key. To access a value in the resource, you must specify the unique key. This is the default resource type.- Value identifies each entry in the resource as a value.- Table identifies the resource as a table that consists of columns and rows. | Table |

## Example - resources

In this example, you reference a resource and specify the following:

- The name of the simulation is Service01.
- The name of the resource is user and it's a reference to an SQlite database of type table.
- The separator for list entries is | pipe.
- The inbound step trigger waits for a message with an uri property containing the value /user and a POST method. If an incoming message meets this condition, the values of the properties id, name and age are saved as buffer values.
- Note: A buffer is used here to be able to write multiple values to the resource at the same time.
- The resource step inserts the buffered values into the resource as table values.

```yaml
schema: SimV1
name: Service01

resources:
  - name: user
    type: Table
    file: ../../resources/db.sqlite
    listEntrySeparator: "|"

services:
  - name: insert
    steps:
      - trigger:
          - uri: /user
          - property: Method
            value: POST
        buffer:
          - jsonPath: id
            name: id
          - jsonPath: name
            name: name
          - jsonPath: age
            name: age
      - resource:
          insert:
            - ref: user
              value: ["{b[id]}", "{b[name]}", "{b[age]}"]
        message:
          payload: Inserted successfully
```
