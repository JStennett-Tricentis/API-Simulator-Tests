# Create simulations from captured traffic

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

## Download captured traffic as a file

If you don't want to create a simulation from the captured traffic right away, you can save it as a .har file. Upload these files anytime you need them.

To save captured traffic, select one or more network capture entries, then select Download.
