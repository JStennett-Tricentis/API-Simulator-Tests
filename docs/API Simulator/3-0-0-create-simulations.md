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
| connections | Connections that are required for communication with a service. | Example |
| includes | Links to other simulation files. You can reuse simulations in multiple other simulations, so you don't have to redo your work. The file path can be absolute or relative to the current schema location. | `C:\my_simulations\test.yml` |
| name | Name of the simulation. | My simulation |
| resources | Resources that you want to share between different services. | Example |
| schema | Version name of the schema applied to the simulation YML file. This property is mandatory and the value must be SimV1. | SimV1 |
| services | Virtual services that you want to simulate. | Example |
| templates | Messages and connections that can be referenced in the virtual services via the property basedOn. | myMessage, myConnection |

- The simulation file must include one or more connections to communicate with a service. Use connection properties to define them.

- Use message properties to define the content of your outgoing messages.

- Use resources properties in your simulation file to reference a resource, such as a data structure.

- Use service properties to specify the order in which you want to send or receive messages.

- Use steps properties to define the process of a service.

- Define rules properties to select which messages you want to send or receive.

- Find out which values you can use to define your properties and how to specify them.

The schema file is a YML file and follows YML patterns. For information on how to work with YML files, see the YML documentation.
