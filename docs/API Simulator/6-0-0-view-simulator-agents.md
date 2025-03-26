# View Simulator agents

The Simulator agents page shows you all Simulator agents registered with API simulation and which simulation file they provide. To open it, go to API simulation >Simulator agents in the menu bar on the left.

The page shows the following information:

- The name of the agent machine where the Simulator agent is running.
- The connection status of the registered Simulator agent. It's Connected if the Simulator agent is currently running and Disconnected if it's not running.
- The version of the Simulator agent.
- The date and time when the Simulator agent was last connected.

The Simulator agent overview page.

Click on a Simulator agent to show its inventory page.

## View Simulator agent inventory

The Simulator agents details page shows you which simulation files are available on a particular Simulator agent.

Click on a Simulator agent to view its details. It opens on the Inventory tab.

The page shows the following information:

- The name of the simulation file it provides.
- The date and time a simulation file was created.
- Which components the simulation files contain. These can be the main elements connections, services, runnable services, resources, or includes.

The Simulator agents details.

## Remove simulation files

To remove a simulation file from the Simulator agents inventory page, open its context menu and select Delete. If you want to remove multiple simulation files, select the simulation files and click the delete icon in the top menu bar.

## Upload simulation files to the simulation registry

To organize your simulation files in one place, you can upload them from a Simulator agent to the simulation registry of your cloud organization. Select one or more simulation files, open the shortcut menu and select Upload to Simulation Registry.

## Check the logs

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

## Check the simulation hits usage

A simulation hit happens when the Simulator agent receives an inbound message and processes it, such as performing a verification. The number of simulation hits you can use depends on your Tosca Cloud plan (opens in new tab).

To check your simulation hit usage, switch to the Utilization tab, which shows the following information:

- The total number of simulation hits used by all agents in the workspace. You can select the time period to get the appropriate data.
- A list of all Simulator agents in the workspace and the number of simulation hits they've consumed.
