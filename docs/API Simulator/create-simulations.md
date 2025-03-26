# Create simulations

Simulations are a set of virtual services and connections that you can use instead of real services that are unavailable in your test environment. A virtual service needs connections to communicate with a service. This means that data is sent to or received from the destination or location that you've specified as the connection in your simulation, such as an URL, port, etc.

You can create simulations in the following ways:

- In the simulation registry, where you manually enter all the required message elements of your simulation.

- Capture the traffic of 2 services that are communicating with each other and save this traffic as a simulation in your simulation registry.

- From an API test to save it in your simulation registry.

- In a text editor or an Integrated Development Environment (IDE) of your choice, where you manually enter all required message elements.

## Create a simulation file

Your simulation file needs the following elements:

Use these top-level elements of a simulation file to define the behavior of a simulation:

| Main element | Description | Example |
| ------------ | ----------- | ------- |
| connections | Connections that are required for communication with a service. | [Example](#example---connection) |
| includes | Links to other simulation files. You can reuse simulations in multiple other simulations, so you don't have to redo your work. The file path can be absolute or relative to the current schema location. | C:\my_simulations\test.yaml |
| name | Name of the simulation. | My simulation |
| resources | Resources that you want to share between different services. | [Example](#example---resources) |
| schema | Version name of the schema applied to the simulation YAML file. This property is mandatory and the value must be SimV1. | SimV1 |
| services | Virtual services that you want to simulate. | [Example](#example---services) |
| templates | Messages and connections that can be referenced in the virtual services via the property basedOn. | myMessage, myConnection |

- The simulation file must include one or more connections to communicate with a service. Use connection properties to define them.

- Use message properties to define the content of your outgoing messages.

- Use resources properties in your simulation file to reference a resource, such as a data structure.

- Use service properties to specify the order in which you want to send or receive messages.

- Use steps properties to define the process of a service.

- Define rules properties to select which messages you want to send or receive.

- Find out which values you can use to define your properties and how to specify them.

The schema file is a YAML file and follows YAML patterns. For information on how to work with YAML files, see the YAML documentation.

---

## Connection properties

A connection is a virtual service endpoint. You need connection properties in your simulation file to establish or mimic a connection to a service. These properties can include timeouts, encryption, or authentication options. For example, if the original service requires SSL encryption, the virtual service may also need to simulate a secure connection in order to behave the same way.

These are common connection properties that you can use:

| Connection property | Description | Example |
| ------------------- | ----------- | ------- |
| basedOn | Reference a connection by using the name of this connection. | Connection 1 |
| capture | Set this property to true to temporarily store all sent or received messages. The default value is false. | true |
| clientCertificateFile | Define this property if you need SSL authentication for a connection. Enter the path to the client certificate file. | /path/to/clientcert.pem |
| clientCertificatePassword | Define this property if the client certificate file requires a password. | test123 |
| endpoint | Specify the endpoint to which you want to send messages. This can be a URI, a port, or an IP address. | `https://www.send-messages.here.com/` |
| forward | Define a connection to which you want to forward all incoming messages. Always specify forward in combination with the properties to, mode and, if needed, simulateOn:- to: Specify the name of the connection. You must define the referenced connection in the same simulation file or another simulation file in the same workspace folder.- mode: Use this property to define what type of connection it is. The following options are available:  - passThrough: The message is forwarded to the real service.  - simulate first: The message is sent to a simulation. If the simulation doesn't work, it's forwarded to the real service.  - forward first: The message is sent to the real service first. If the service isn't available, it's forwarded to the simulation. In addition, you need the property simulateOn to specify when the simulation should be triggered.  - learning: The message is sent to the specified connection and API simulation saves the interaction with the real service as a simulation. This creates a new simulation file with the updated behavior of the service. When API simulation creates the simulation file, it detects possible trigger elements and saves them as trigger properties.- simulateOn: You need this property only in combination with the property forward first. This lets API simulation know when to trigger the simulation.  - timeout: Specify how long API simulation should wait for the service to respond before the fallback service starts. Enter the time in milliseconds.  - statusCode: You can enter multiple status codes, separated by comma. | forward: to: myConnection2 mode: passThrough |
| keepProcessed | It specifies whether processed messages should be kept or deleted. Set the value to true if processed messages should be kept. The default value is false. | true |
| listen | By default, the value of this property is set to true. This means that the connection sends and receives messages. To specify that the connection should only send messages, set the value to false. | true |
| name | The name of the connection. | myConnection |
| password | Specify this property if authentication is required. | test123 |
| port | The port used to listen for incoming messages. | 8080 |
| queue | You need this property if you use a message broker queue for reading or writing messages. Specify the name of the queue you want to use. | myQueue |
| selector | Use this property in conjunction with message broker queues. The selector allows you to filter messages received from the queue. Only those messages that contain the specified value pass through. The syntax and support of the property selector depends on the message broker that you use. | JMSPriority = 9 |
| serverCertificateFile | If you want the connection to be encrypted, specify the path to the certificate. | /etc/certs/server.pfx |
| serverCertificatePassword | Specify this property if the connection requires encryption and a password for the provided server certificate file. | test123 |
| serverCertificateThumbprint | Specify the thumbprint of the certificate if the connection must be encrypted. When opening a connection, API simulation uses the thumbprint to search for the certificate in the certificate stores of the operating system. | a909502dd... |
| topic | You need this property if you use a message broker topic for reading or writing messages. Specify the name of the topic that you want to use. | myTopic |
| type | Specify this property to use a connection technology other than the default HTTP. For instance, to send or receive messages via message broker or to connect to a folder. Valid values are AmazonSqs, Amqp, Directory, Http, IbmMq, Kafka, RabbitMQ, Solace and AzureSb. | RabbitMQ |
| uri | Specify the URI, to which the messages should be sent. | `www.tricentis.com` |
| username | Specify this property if authentication is required. | myUser |

Use these connection properties to define a specific file directory on your local machine or a network drive:

| Connection property | Description | Example |
| ------------------- | ----------- | ------- |
| create | Specify whether the connection should create a new folder in the file system if it doesn't exist. The default value is false. | true |
| fileExtension | Specify the file extension. | xml |
| folder | Specify the path to a folder that API simulation will check for new messages. The type property of the connection must be set to folder. | /path/to/messages |

These are the Amazon MQ broker connection properties that you can use:

| Connection property | Description | Example |
| ------------------- | ----------- | ------- |
| accessKey | Enter your Amazon SQS access key ID. | your_access_key_id |
| account | Enter the name of your Amazon web service account (AWS). | myAccount |
| region | Some operations require you to define a region. For the best network performance, you can select a region that is geographically near to you. | us-east-1 |
| secretKey | Enter your Amazon SQS secret access key. | your_secret_access_key |

These are the Apache Kafka broker connection properties that you can use:

| Connection property | Description | Example |
| ------------------- | ----------- | ------- |
| clientPropertiesFile | Specify the path to the file that contains specific Kafka connection settings. | /path/to/kafka.properties |
| groupId | Specify the identifier of the group you want to use. | myGroup |
| partition | Specify the number of the partition you want to use. | 1 |

These are the Azure Service Bus broker connection properties that you can use:

| Connection property | Description | Example |
| ------------------- | ----------- | ------- |
| accessKey | Enter your Azure Service Bus access key to connect to an Azure Service Bus. | your_access_key |
| accessKeyName | Enter your Azure Service Bus access key name. | RootManageSharedAccessKey |

These are the IBM MQ broker connection properties that you can use:

| Connection property | Description | Example |
| ------------------- | ----------- | ------- |
| channel | Specify the name of the channel you want to used to communicate with the message broker. | CH1 |
| CipherSpec | Specify the combination of encryption algorithm and message authentication code algorithm to enable encrypted communication (SSL). | ECDHE_ECDSA_3DES_EDE_CBC_SHA256 |
| CipherSuite | Specify your suite of cryptographic algorithms. Cipher Suite is an alternative way to select a spec to enable encrypted communication (SSL). | SSL_ECDHE_ECDSA_WITH_3DES_EDE_CBC_SHA |
| KeyRepository | Specify the file path to your key repository without file extension to enable encrypted communication (SSL). | C:\Data\ssl_certificate |
| manager | Specify the name of the IBM MQ queue manager you want to use to communicate with the message broker. | myManager |
| peerName | Specify this property to verify the Distinguished Name (DN) of the certificate from the peer queue manager. | QMGR |

These are the RabbitMQ broker connection properties that you can use:

| Connection property | Description | Example |
| ------------------- | ----------- | ------- |
| exchange | Specify the name of the exchange you use for routing the messages to different queues. | myExchange |
| routingKey | Specify the routing key if the broker requires it. | myRoutingKey |
| virtualHost | Specify the name of the virtual host. | myVirtualHost |

These are the Solace broker connection properties that you can use:

| Connection property | Description | Example |
| ------------------- | ----------- | ------- |
| trustStorePath | Specify this property if the validateCertificate property is enabled. Enter the path to a collection of trusted certificates. | /path/to/trust.jks |
| validateCertificate | Specify whether to trust the certificates defined in the trust store. The default value is false. To trust the certificates, set the value to true. If this property is enabled, you must specify the trustStorePath property. | true |
| privateKeyFile | If you use a client certificate file, you must define this property. Enter the path to the client certificate key file. | /path/to/clientkey.pem |
| privateKeyFilePassword | If a private key file is password protected, specify this property. | myPass |
| vpnName | Specify the name of the message VPN you want to join. | myVPN |

### Example - connection

This is an example of how a connection could look like:

- The simulation contains two connection templates named myConnection01 and myConnection02.
- The service named service01 refers to the connection template myConnection01 with the port property 8080.

```yaml
schema: SimV1
name: my simulation

templates:
  connections:
  - name: myConnection01
    port: 8080
  - name: myConnection02
    port: 8081

services:
- name: service01
  steps:
  - direction: In
    connection:
      basedOn: myConnection01
```

---

## Supported message brokers

Use message brokers to receive and forward messages from API simulation to one or more recipients. You can define the broker technology as a connection property in your simulation.

API simulation supports the following broker technologies:

- Amazon MQ
- Apache ActiveMQ
- Apache Artemis
- Apache Kafka
- IBM MQ
- RabbitMQ
- Solace
- Azure Service Bus

---

## Message properties

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

### Example - message

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

---

## Work with multipart messages

Multipart or mixed MIME messages contain different types of content, such as images, videos, text, and so on. You can insert, verify, save, trigger, or buffer these message parts in your simulations without losing the other parts of those messages.

To define a multipart message, simply add the following rules properties to your simulation:

- type with the value multipart.
- file with the file path or value if you want to use a text string.
- optionally, use properties such as key to better identify message elements.

### Example - send multipart message

This is an example of how to send a multipart message:

- The insert property of the outbound step writes the text this is a text to `http://tricentis.com`. It also sends the image test_attachment.png and the file test_attachment_pdf to the same directory.

```yaml
schema: SimV1
name: default

services:
  - name: send multipart message
    steps:
      - direction: Out
        to: http://tricentis.com
        insert:
          - type: Multipart
            value: this is a text
          - type: Multipart
            key: my image
            file: images/test_attachment.png
          - type: Multipart
            key: my pdf
            file: pdf/test_attachment.pdf
```

### Example - receive multipart message

This is an example of how to verify a multipart message:

- The verify property of the inbound step checks whether the message contains an image named image_attachment.png and the text this is a text.

```yaml
schema: SimV1
name: default

services:
  - name: receive multipart message
    steps:
      - direction: In
        verify:
          - type: Multipart
            key: my image
            file: image_attachment.png
          - type: Multipart
            index: 0
            value: this is a text
```

---

## Step rules properties

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

### Example

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

---

## Services properties

Use service properties in your simulation file to specify the order in which to send or receive messages.

These are the services properties you can use:

| Services property | Description | Example |
| ----------------- | ----------- | ------- |
| description | Optionally, add a description of the service. | myDescription |
| name | Specify the name of the service. | myService01 |
| steps | Specify the steps that the virtual service should execute. Depending on what type of virtual service it is, this can be done in two or more steps. For instance, the first step is incoming and the second outgoing (request response pair) or vice verse (contract). Or you can define several steps to simulate messages in a specific order (scenario). | [Example](#example---services) |

### Example - services

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

## Steps properties

Use steps properties in your simulation file to define the process of a service. This specifies the simulation that you want to perform.

These are the steps properties you can use:

| Steps property | Description | Example |
| -------------- | ----------- | ------- |
| buffer | Use buffer in messages to save values. This property includes a collection of rules that are applied to create buffers. | [Example](#examples) |
| connection | Specify a connection for sending or receiving messages. To do this, use the connection properties. | [Example](#example---connection) |
| counter | Define how many times the specified message should be sent or received before proceeding to the next step. | 5 |
| description | Description of what the step does. | contract verification |
| direction | Specify whether the step is for sending or receiving messages. To send messages, enter the value Out. To receive messages, enter the value In.If you leave the value blank, the direction is derived logically (implicit). It's assumed that the first step is In, the next step Out, and so on. | Out |
| from | Reference to a connection by using the name of the connection to receive messages.If you want to use this, you need to set the direction property to In. | myConnection |
| insert | Collection of rules to insert values. | [Example](#example---insert) |
| message | Specify the message to send. To do this, use the message properties. | [Example](#example---message) |
| name | Specify the name of the step. | step 1 |
| order | This property defines whether messages should be received in a certain order or not.Enter STRICT to use the specified order or ANY to accept any order. The default value is STRICT. | STRICT |
| resource | In your simulation file, you can reference a resource, such as a data structure. To define what happens to this resource, specify a rule in conjunction with a resource steps property. | [Example](#example---resources) |
| save | Collection of rules to store message payloads or other properties of a message in a specific file. Use the file property to set the file name and file extension. | [Example](#example---save) |
| to | Reference a connection by using the name of this connection to send messages.If you want to use this, you need to set the direction property to Out. | myConnection |
| trigger | Collection of rules to start a services. If all rules apply, API simulation starts the service. | [Example](#example---trigger) |
| verify | Collection of rules for message verification. If the verification fails, API simulation logs a warning. | [Example](#example---verify) |

### Examples

#### Example - insert

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

#### Example - save

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

#### Example - trigger

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

#### Example - verify

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

---

## Set service step rules

A virtual service is a sequence of steps that simulate message flows. A service step rule specifies the action, in combination with a rule, that the step performs. For instance, you can define rules for when a simulation should send a message, or which message a simulation should wait for before sending a response.

The following properties determine how to use the value of a rule:

- For inbound messages, use the properties Trigger, Save, Verify, and Buffer.
- For outbound messages, use the properties Insert, Save, and Buffer.

### Trigger

This property identifies a service by comparing a value. API simulation proceed to the next step when the specified condition is met.

Specify a Trigger service step as follows:

- Enter the Trigger property in an inbound message. It is required to respond to a request.
- For the value, you can use literal expressions, an asterisk * as a wildcard, or dynamic expressions. You can also work with operators, such as 'greater than' or 'less than'.

The verification of values is case-sensitive. If you use case-insensitive matching in your simulations, you have to adapt the values or use a regular expression.

Example values:

| Value | Result |
| ----- | ------ |
| * | Any string |
| AX* | Any string starting with AX. Examples: AX1234, AXANYVALUEEUEUEU, AX |
| *AX | Any string ending with AX. Examples: 123AX, BCAAAX, AX |
| < 5 | Any number smaller than 5. Examples: -5, 0, 1 |
| != {DATE} | Any date, except for the current date. Examples: 2010-01-01 |
| < `{DATE[11/03/2015][][MM/dd/yyyy]}` | Any date before November 3, 2015. |

### Verify

This property compares a specified value in inbound messages with the value received from a service. This lets you ensure that a service works as expected.

To verify values, follow these steps:

1. Add the Verify property to an inbound message.
2. Enter the expected value. You can use literal and regular expressions.

### Buffer

This property temporarily stores the value of a message, so that you can reuse it within a virtual service. You can insert these values into messages or use them for verifications. Make sure to assign meaningful names to your buffer values, so you can find them again.

This property has the following specifications:

- Save values from in- or outbound messages as a buffer.
- Use buffer values to insert them into outbound messages.
- Use buffer values in the same message. For instance, to verify other message elements in the message.

### Insert

With this property you can insert message elements into outbound messages.

It has the following specifications:

- Enter a value to insert the message element with the specified value.
- If you leave the value empty, API simulation inserts the message element without a value.

### Save

With this property you can save message payloads or other properties of a message to a specific file.

It has the following specifications:

- Save values from in- or outbound messages to a file.

---

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

---

## Resources properties

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

### Example - resources

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

---

## Resource steps properties

You can work with resources to share data at runtime between different running simulations. Use a resource step property in your simulation file to define how a resource is controlled. You can modify the data of a resource or read it.

These are the steps properties you can use:

| Resource steps property | Description |
| ----------------------- | ----------- |
| delete | Clears data in a resource. |
| insert | Pastes data into a resource. |
| read | Reads data from a resource. |
| update | Overwrites a value in a resource. |

### Example - update

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

---

## Resource rules properties

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

### Example - one

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

### Example - where

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

### Example - template

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

---

## Create simulations from captured traffic

When you run applications, it generates traffic between services. For example, a sign in page requires an authentication token to verify the user's authorization. In the background, the application sends a request for a JSON Web Token.

To create a simulation from the traffic, you must first capture the traffic between the services. From this captured traffic, you select the content you need to simulate the interaction between these services. API simulation stores your selections as simulation files in the agents inventory. This allows you to create simulations automatically with minimal manual editing.

To create a simulation from captured traffic, follow these steps:

1. Go to API simulation > Simulator agent and select a connected Simulator agent.

   You need the Simulator agent to capture the traffic.

2. Go to the Capture tab and enable the switch next to the connection that you want to capture.

   A connection defines the endpoint to which a service sends messages.

3. Check the captured traffic on the left side of the page under Network Capture.

   If you don't see any captured traffic, make sure the service is sending messages to your connection. You can also simulate these messages using the API playground.

4. Select one or more network captures that you want to save as a simulation file.

5. Select Create service and the type of service you want to create in the following dialog. You have these choices:

   - Service: A separate service for each selected network capture that waits for an incoming message and responds like a real service (inbound).
   - Service as a scenario: A service scenario consists of multiple messages that the service must trigger in a specific order. For example, if a message needs to be triggered before the next one can be triggered.
   - Runnable: A runnable service for each selected network capture that starts with an outgoing message to request data from other services (outbound).
   - Runnable as a scenario: A runnable scenario consists of multiple messages that the service must send in a specific order. For example, if a message needs to be started before the next one can be sent.

6. Enter a unique name for your simulation. If not, API simulation automatically sets a name with a timestamp.

7. Optionally, you can enable these settings:
   - Create pretty print: Structures the content of the file to make it easier to read.
   - Save payloads to separate files: This is useful for large payloads to keep the content of the services smaller: the service then links to the payload files instead of storing the payload content in the simulation.

When API simulation creates the simulation file, it tries to automatically detect possible trigger elements from the captured traffic and save them as trigger properties.

### Download captured traffic as a file

If you don't want to create a simulation from the captured traffic right away, you can save it as a .har file. Upload these files anytime you need them.

To save captured traffic, select one or more network capture entries, then select Download.

---

## Create simulations from API actions

Application programming interfaces (APIs) allow different software applications to communicate with each other and exchange information. API testing involves sending requests to the API and analyzing the responses to ensure that they behave as expected.

If you want to save your API test as a simulation, you can easily create one:

1. Go to API simulation > API playground.

2. Create a new API action or select an existing one from the left pane.

3. In the subsequent dialog, select Create simulation and the type of service you want to create:
   - Service: A service that waits for an incoming message and responds like a real service (inbound).
   - Runnable: A runnable service that starts with an outgoing message to request data from other services (outbound).

4. Enter a unique name for your simulation.

5. Select whether you want to create your simulation with or without a connection. You need a connection in your simulation to define the endpoint to which a service sends messages. To create a new connection, enter the Port and Connection name. Select No connection, if you want to manually add an existing connection later.

And that's it! To view your new simulation, go to API simulation > Simulation registry.

---

## Organize your simulations

The Simulation registry page is a list of all the simulation files you have uploaded to Tosca Cloud.

To open it, go to API simulation > Simulation registry in the menu bar on the left.

The page shows the following information:

- The name of the simulation file.
- The date and time when the simulation file was created.
- The date and time when the simulation file was last modified.
- Which components the simulation files contain. These can be the main elements connections, services, runnable services, resources, or includes.
- Tags that you've added to your simulations. You can use tags, for instance, to sort or filter your simulations.

The Simulation registry page.

You can search for simulation files or click the column header to sort columns in ascending or descending order.

If you want to create custom filters for your simulation files select .

### Upload simulation files

You can upload a simulation file from your local machine to the simulation registry if you have already created one. To do so, follow these steps:

1. On the upper right corner of the Simulation registry page, click Upload file.
2. Choose the simulation files you want to upload.
3. Click Open.

### Create new simulation files

You can create new simulation files directly in the simulation registry.

To do so, follow these steps:

1. On the upper right corner of the Simulation registry page, click Create service.
2. Select Edit to leave the read- only mode.
3. Enter a name for your simulation in the Simulation File field.
4. Optionally, enter one ore more tags into the Tags field. Confirm each entry with Enter. You can use tags, for instance, to sort or filter your simulations.
5. Define the content of your simulation.
6. Click Save.

Create simulation file.

### Deploy simulation files to Simulator agents

After you have uploaded or created simulation files, you can deploy these files to a Simulator agent that is connected to Tosca Cloud and run them there.

To do so, follow these steps:

1. Use the checkboxes to choose which simulation files you want to deploy.
2. Right-click and select Deploy from the context menu.
3. Select a workspace and an agent from the respective lists.
4. Click Deploy. This starts the simulation on the Simulator agent.

### Edit simulation files

If you want to extend or modify a simulation, follow these steps:

1. Click on the simulation file that you want to edit.
2. Select Edit to leave the read-only mode.
3. Make your changes and click Save.

### Download simulation files

You can download your simulation files from Tosca Cloud, for instance, to archive them. To do so, follow these steps:

1. On the Simulation registry page, use the checkboxes to choose which simulation files you want to download.
2. Right-click and select Download. This automatically creates a zip file and saves it in File Explorer under Downloads.

### Remove simulation files

To remove a simulation file from the simulations overview page, right click on it and select Delete from the context menu.

If you want to remove multiple simulation files, select the simulation files via the checkbox next to the files. Then click the delete icon in the top menu bar.

---

## Test APIs

Application programming interfaces (APIs) allow different software applications to communicate with each other and exchange information. API testing involves sending requests to the API and analyzing the responses to ensure that they behave as expected.

To test an API, you need one or more API actions. These API actions contain all the information you need to communicate with the API, including connection, message, verifications, or required authentication. Once you have created an API action, you can run a single test on demand by sending a request, or create an API test case to use it in multiple test cases that you create in the test case editor.

### Create API action

To create an API action, follow these steps:

1. Go to API simulation > API playground and select New API action.

2. In the right pane, choose an appropriate name for your API action.

3. Define the properties for the request message:
   - Method for sending messages.
   - The Endpoint URL of a connection.
   - Headers if you want to add additional information.
   - Authorization if your service requires it. You can enter basic credentials or a Bearer access token.

4. Define a Verification for the response message, if you want to check that the expected value matches the actual value.

5. Select Send to test whether your entries are valid and the verification returns the expected value.

6. Save your API action.

The tab titles of Headers, Authorization and Verification show the total number of items.

You can see a list of all available API actions in the API playground panel:

API playground

### Define verifications

Verifications allow you to test whether a service works as expected. For example, if the messages have the correct format, content, or behavior. To do this, you can define property values in the response and compare them to the actual values returned by the service.

To define verifications, follow these steps:

1. Go to API simulation > API playground.

2. Select an existing API action from the list or create a new one.

3. In the Response section on the right, select the Verification tab.

4. Select the icon to add a new verification.

5. Select an entry of the newly created verification and edit the following properties:
   - Give your verification a unique Name.
   - Specify the Type of your verification. You can use a Header, JsonPath, XPath, StatusCode or ResponseTime to verify a value.
   - If you selected JsonPath or XPath, define the corresponding Expression.
   - Select your Operator.
   - Enter the expected Value.

6. Save your API action.

This example shows how to use an XPath expression to check the result of a calculation.

### Create API test case

To create an API test case, follow these steps:

1. Go to API simulation > API playground.

2. Select an existing API action from the list, or create a new one.

3. Select Create API test case.

4. In the menu bar on the left, select Test cases to find your new API test case.

5. Select an API test case to open it in the test case editor to further design your test case.

### Test API action

1. Go to API simulation > API playground.

2. Select an existing API action from the list or create a new one.

3. Select Send.

4. On the Response tab of your message you can see the following results:
   - The status code in the response header. It shows you if the message was successfully sent to the API.
   - The result of your verifications if you set any.

Response message

### Delete API action

To delete an API action, go to API playground and select the API action you want to delete from the left pane. Then select Delete.

---

## View Simulator agents

The Simulator agents page shows you all Simulator agents registered with API simulation and which simulation file they provide. To open it, go to API simulation >Simulator agents in the menu bar on the left.

The page shows the following information:

- The name of the agent machine where the Simulator agent is running.
- The connection status of the registered Simulator agent. It's Connected if the Simulator agent is currently running and Disconnected if it's not running.
- The version of the Simulator agent.
- The date and time when the Simulator agent was last connected.

The Simulator agent overview page.

Click on a Simulator agent to show its inventory page.

### View Simulator agent inventory

The Simulator agents details page shows you which simulation files are available on a particular Simulator agent.

Click on a Simulator agent to view its details. It opens on the Inventory tab.

The page shows the following information:

- The name of the simulation file it provides.
- The date and time a simulation file was created.
- Which components the simulation files contain. These can be the main elements connections, services, runnable services, resources, or includes.

The Simulator agents details.

### Remove simulation files

To remove a simulation file from the Simulator agents inventory page, open its context menu and select Delete. If you want to remove multiple simulation files, select the simulation files and click the delete icon in the top menu bar.

### Upload simulation files to the simulation registry

To organize your simulation files in one place, you can upload them from a Simulator agent to the simulation registry of your cloud organization. Select one or more simulation files, open the shortcut menu and select Upload to Simulation Registry.

### Check the logs

Click on a Simulator agent and select the Logs tab to view all the processes that the Simulator agent has performed. It lists the following log details:

- Any message sent or received by the Simulator agent.
- A timestamp in milliseconds for each log event.
- The log level to check how important the log message is.
- What type of log event it is.
- Connection name, if available.
- Name of the service, if available.
- The name of the step where the log event occurred, if a name is defined.
- The added or modified simulations.
- Any error that occurred.

### Check the simulation hits usage

A simulation hit happens when the Simulator agent receives an inbound message and processes it, such as performing a verification. The number of simulation hits you can use depends on your Tosca Cloud plan (opens in new tab).

To check your simulation hit usage, switch to the Utilization tab, which shows the following information:

- The total number of simulation hits used by all agents in the workspace. You can select the time period to get the appropriate data.
- A list of all Simulator agents in the workspace and the number of simulation hits they've consumed.

---

## Export scenarios from Tosca

If you already use Tosca OSV, you can export OSV scenarios from Tosca on-prem to a YAML file. This allows you to re-use your existing scenarios in API simulation. Upload these YAML files to Tosca Cloud or run them with the Simulator agent.

### Prerequisites

To use OSV scenarios in API simulation, you have to enable the OSV early access feature Export Scenarios as a File. This feature is available in Tosca version 15.2 or later.

### Export your scenario to file

Once you have enabled the early access feature, you can export your Tosca OSV scenarios. To do so, follow these steps:

1. In Tosca on-prem, select a specific scenario or a ComponentFolder that contains scenarios. Hold CTRL to select multiple objects.

2. Right-click the scenarios or folders and select OSV->Export to File from the context menu.

3. If you have selected multiple scenarios, define the export mode in the subsequent dialog.

4. Click Yes to export all scenarios as a single file or No to create a separate file for each scenario.

5. If you export all scenarios to a single file, specify destination folder and file name in the subsequent dialog and click OK.

6. If you export each scenario to a separate file, select a destination folder and click OK.

7. Tosca uses the name of the scenario as the name of the corresponding file. If the destination folder already contains a file with this name, Tosca overwrites it.

We recommend that you store your simulation files in the Simulator agent workspace folder.
