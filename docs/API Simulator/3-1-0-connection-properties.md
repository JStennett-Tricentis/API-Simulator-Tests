# Connection properties

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

## Example - connection

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
