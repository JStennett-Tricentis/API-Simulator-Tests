# Create simulations from API actions

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
