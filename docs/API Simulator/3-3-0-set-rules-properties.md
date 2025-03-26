# Step rules properties

Define rules in your simulation file to automate the selection of messages to send or receive. For instance, you can identify a service by comparing values.

A rule is always used combined with a step and can be either true or false. You can use rules for the following steps: trigger, buffer, verify, insert or save.

These are the rules properties you can use:

| Rules property | Description | Example |
| -------------- | ----------- | ------- |
| count | Use this rule together with the jsonPath or xPath properties. Whenever there are multiple elements within a message with a specific path, API simulation verifies that the number of elements in a message corresponds to the defined value. | 5 |
| dataType | Specifies the data type of the value property. Use the data types Boolean, Date, Numeric, Password, or String. | String |
| exists | Indicates whether the element you want to verify exists or not. Set the value to True if it exists or to False if it doesn't. | True |
| file | Specifies the path, file name, and file extension where the payload or other message property values are saved. | /path/to/file.txt |
| index | If an element occurs more than once, define an index to determine which element to use. Let's say there are 2 headers with the same name and you want to use the second one. The index should be 2. | 2 |
| jsonPath | Use this property to find an element in a message that is in JSON format. Enter the path to the element. Define the value of the element via the property value. | x.store.book[0].title |
| key | Use this property to find specific message elements. Enter the name of the element. Define the type of the message element with the type property. If you want to insert the key, you also need to add and define the value property. | myHeader |
| name | Defines the name of the rule property. | myBuffer |
| operator | Use this property to find specific property value ranges, such as 'greater than' or 'less than'. Possible operators are Equal, NotEqual, Less, LessOrEqual, Greater, GreaterOrEqual, or In. | Greater |
| property | References a connection or message property to specify a rule. For instance Endpoint, Path, or Queue. | Endpoint |
| range | Specify this property to use a specific part of a value. We use the C# range operator specification. You define a range by 2 index operands. Each of the two operands can be omitted. For the calculations, counting starts at 0. The left index is inclusive and the right index is exclusive:- `<Index>`..`<Index>` defines the range from a specific character to a specific character.- ..`<Index>` defines the range from the first character to a specific character.- `<Index>`.. defines the range from a specific character to the last character.Use the hat (^) operator in addition to the index to take the index from the end. For instance, ..^4 takes all characters up to the last 4 characters or ^4.. starts after the last 4 characters and ends with the last character. | Example text..3 gives Exa3..^3 gives mple t^3.. gives ext |
| type | Specifies which part of a message to steer. Possible types are Header, MessagingProperty, Query, Path, or Multipart.For all types except Path, add the key property to find these specific message elements. | Header |
| uri | Defines the URI, or a part of it, that you want to use as the value for a rule. | /wait/for/me |
| value | Defines the value of a rule property. This can be any text or a Boolean value. | myValue |
| xPath | Use this property to find an element in a message that is in XPath format. Enter the path to the element. Define the value of the element via the property value. | /bookstore/book[price>35]/price |

## Example

This is an example of how to insert values:

- The inbound step property trigger waits for a message with a path property containing any value. If an incoming message meets this condition, API simulation sends a response.
- The outbound step property insert writes the value Hello world! to the payload of a service.

```yaml
schema: SimV1
name: default

services:
- name: Service01
  steps:
  - direction: In
    trigger:
    - uri: '*'
  - direction: Out
    insert:
    - value: 'Hello world!'
```
