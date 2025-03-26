# Export scenarios from Tosca

If you already use Tosca OSV, you can export OSV scenarios from Tosca on-prem to a YAML file. This allows you to re-use your existing scenarios in API simulation. Upload these YAML files to Tosca Cloud or run them with the Simulator agent.

## Prerequisites

To use OSV scenarios in API simulation, you have to enable the OSV early access feature Export Scenarios as a File. This feature is available in Tosca version 15.2 or later.

## Export your scenario to file

Once you have enabled the early access feature, you can export your Tosca OSV scenarios. To do so, follow these steps:

1. In Tosca on-prem, select a specific scenario or a ComponentFolder that contains scenarios. Hold CTRL to select multiple objects.

2. Right-click the scenarios or folders and select OSV->Export to File from the context menu.

3. If you have selected multiple scenarios, define the export mode in the subsequent dialog.

4. Click Yes to export all scenarios as a single file or No to create a separate file for each scenario.

5. If you export all scenarios to a single file, specify destination folder and file name in the subsequent dialog and click OK.

6. If you export each scenario to a separate file, select a destination folder and click OK.

7. Tosca uses the name of the scenario as the name of the corresponding file. If the destination folder already contains a file with this name, Tosca overwrites it.

We recommend that you store your simulation files in the Simulator agent workspace folder.
