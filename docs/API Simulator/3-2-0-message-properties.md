# Message properties

Use message properties in your simulation file to define outgoing messages. A message should contain all information that your service requires.

These are the message properties that you can use:

| Message property | Description | Example |
| ---------------- | ----------- | ------- |
| basedOn | Reference a message by using the name of this message. | Message 1 |
| delay | Wait time in milliseconds before a response is sent. | 500 |
| encoding | Sets the character encoding. | UTF-8 |
| headers | Collection of additional information that can be sent with a request or received with a response message. | date: 14.12.2021 |
| messagingProperties | Collection of addition information that can be sent with a request or received with a response message. This property is only supported for messaging technologies, for example IBM MQ. | my property: 10 |
| method | Method for sending messages. Possible values are: GET, POST, PUT, DELETE, or PATCH. | PUT |
| name | Name of the message. | Message 2 |
| payload | The actual content of the message. This is the data that is transported during a communication between two services. | Text |
| payloadBase64 | You can use this property as an alternative to payload if the payload contains special characters that YAML doesn't support. The value must be BASE64 encoded. | aGVsbG8gd29ybGQh |
| payloadFile | You can use this property as an alternative to payload if you want to send the payload in a separate file. The value must be a file path, either absolute or relative to the simulation file. | ..\simulator\payload.txt |
| statusCode | The HTTP status code of a response message. Specify the number, or specify number and description. | 401 Unauthorized |

## Example - message

This is an example of how a virtual service could look like. It specifies the following:

- The name of the virtual service is Service01 and it is a request response pair.
- The inbound step trigger waits for a message with a path property containing any value. If an incoming message meets this condition, API simulation sends a response.
- The outbound property message sends the value Hello world! to a service with the connection port 54545.

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
    message:
      payload: Hello world!

connections:

- port: 54545
```
