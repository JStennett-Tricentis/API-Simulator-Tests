# About API simulation

API simulation allows you to simulate interactions and the behavior of services instead of depending on these services. For instance, whenever services are still under development, offline, or broken, service virtualization enables you to create virtual or simulated versions of the services you need to work with. This includes third-party services such as credit checks, authentication services, APIs, or other external services with which your test application needs to interact.

## What's a simulation?

Simulations are a set of virtual services and connections that you can use instead of real services that are unavailable in your test environment. A virtual service needs connections to communicate with a service. This means that data is sent to or received from the destination or location that you've specified as the connection in your simulation, such as an URL, port, etc.

You can communicate with services using a broad set of message formats and protocols such as HTTP, HTTPS, IBM MQ, Kafka, RabbitMQ, files, and more.

In a virtual service you define messages that you want to send or receive from other services. These messages can be either incoming or outgoing. Depending on the purpose of the message, you determine whether to start with an incoming or outgoing message.

- If you want to send a response to specific incoming messages, you can define a rule. API simulation applies this rule to all incoming messages. If all properties match, a message is sent as a response (trigger).

- You can send a request and check all incoming response messages for specific properties to ensure that a service is behaving as expected (API call).

- You can define a virtual service that receives messages from multiple services or responds to multiple services in a specific order.

## How API simulation works

API simulation is tailored to the needs of both testers and developers. As a first start, you create the simulations of the services you need to work with directly in Tosca Cloud or any Integrated Development Environment (IDE) of your choice as a YAML file. If you already use Tosca OSV, you can also migrate your simulations (scenarios) to API simulation.

In Tosca Cloud, you can view all your simulations and see to which Simulator agent they are deployed.

Deploy simulations from Tosca Cloud to any Simulator agent on any machine and run them there.

The Simulator agent hosts simulations that you can run in your local environment on any machine. These simulations send messages, receive requests, or respond like real services.
