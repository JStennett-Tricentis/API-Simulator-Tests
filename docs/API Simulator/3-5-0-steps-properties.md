# Steps properties

Use steps properties in your simulation file to define the process of a service. This specifies the simulation that you want to perform.

These are the steps properties you can use:

| Steps property | Description | Example |
| -------------- | ----------- | ------- |
| buffer | Use buffer in messages to save values. This property includes a collection of rules that are applied to create buffers. | Example|
| connection | Specify a connection for sending or receiving messages. To do this, use the connection properties. | Example |
| counter | Define how many times the specified message should be sent or received before proceeding to the next step. | 5 |
| description | Description of what the step does. | contract verification |
| direction | Specify whether the step is for sending or receiving messages. To send messages, enter the value Out. To receive messages, enter the value In.If you leave the value blank, the direction is derived logically (implicit). It's assumed that the first step is In, the next step Out, and so on. | Out |
| from | Reference to a connection by using the name of the connection to receive messages.If you want to use this, you need to set the direction property to In. | myConnection |
| insert | Collection of rules to insert values. | Example |
| message | Specify the message to send. To do this, use the message properties. | Example |
| name | Specify the name of the step. | step 1 |
| order | This property defines whether messages should be received in a certain order or not.Enter STRICT to use the specified order or ANY to accept any order. The default value is STRICT. | STRICT |
| resource | In your simulation file, you can reference a resource, such as a data structure. To define what happens to this resource, specify a rule in conjunction with a resource steps property. | Example |
| save | Collection of rules to store message payloads or other properties of a message in a specific file. Use the file property to set the file name and file extension. | Example |
| to | Reference a connection by using the name of this connection to send messages.If you want to use this, you need to set the direction property to Out. | myConnection |
| trigger | Collection of rules to start a services. If all rules apply, API simulation starts the service. | Example |
| verify | Collection of rules for message verification. If the verification fails, API simulation logs a warning. | Example |

## Examples

### Example - insert

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

### Example - save

This example shows how to save message payloads.

- The inbound step contains two properties:
  - The trigger property to wait for a message containing any value as the value of the Path property.
  - The save property to save the incoming message that meets this condition to a file named request.dat.
- Once the incoming message has been saved, the outbound step property insert writes the value Request saved successfully to the payload of a service.

```yaml
schema: SimV1
name: default

services:
- name: Service01
  steps:
  - direction: In
    trigger:
    - uri: '*'
    save:
    - file: request.dat
  - direction: Out
    insert:
    - value: Request saved successfully
```

### Example - trigger

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

### Example - verify

This is an example of how to verify message rules:

- The inbound step contains two properties:
  - The trigger property to wait for a message containing any value as the value of the Path property.
  - The verify property to verify if the value of the path property is /test/content.
- The outbound step property insert writes the value ok to the payload of the service.

```yaml
schema: SimV1
name: default

services:
- name: Service01
  steps:
  - direction: In
    trigger:
    - uri: '*'
    verify:
    - uri: /test/content
  - direction: Out
    message:
      payload: ok
```
