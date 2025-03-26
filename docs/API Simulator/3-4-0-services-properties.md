# Services properties

Use service properties in your simulation file to specify the order in which to send or receive messages.

These are the services properties you can use:

| Services property | Description | Example |
| ----------------- | ----------- | ------- |
| description | Optionally, add a description of the service. | myDescription |
| name | Specify the name of the service. | myService01 |
| steps | Specify the steps that the virtual service should execute. Depending on what type of virtual service it is, this can be done in two or more steps. For instance, the first step is incoming and the second outgoing (request response pair) or vice verse (contract). Or you can define several steps to simulate messages in a specific order (scenario). | Example|

## Example - services

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
