# ServiceFusion API Simulator Services/Tests

## About API Simulator

API Simulator is a Tricentis Cloud tool that provides service virtualization capabilities, allowing teams to:

- Simulate complex service interactions.
- Test applications without relying on actual services.
- Create reproducible testing environments.
- Accelerate development and testing processes.

## Setup

### API Simulator

1. Login to your Tosca Cloud tenant.

1. Click on the Downloads button in the upper righthand corner.

    ![Tosca Cloud Downloads button](./src/images/tosca_cloud_downloads_button.png)

1. In the Downloads model download the necessary version for your OS.

    ![Tosca Cloud Downloads model](./src/images/tosca_cloud_downloads_model.png)

1. Launch the API Simulator.

1. Open the [API Simulator UI](http://localhost:17070/ui).

1. Click on the Cloud icon in the upper righthand corner.

    ![Connect to Cloud button](./src/images/connect_to_cloud_button.png)

    ![Initial Cloud connection details](./src/images/cloud_connection_details.png)

1. Click on the "Connect" button.

    ![Connect button](./src/images/connect_button.png)

1. Enter your Cloud tenant URL and click "Connect".

    ![Setup your Simulator model](./src/images/setup_your_simulator_model.png)

    ![Set up complete model](./src/images/setup_connection_complete_model.png)

1. The Cloud connection details should now looks like this:

    ![Cloud connection successful](./src/images/cloud_connection_success.png)

### Import the .yml files

1. Clone the repository:

    ```bash
    git clone https://github.com/JStennett-Tricentis/API-Simulator-Tests.git
    ```

1. In the API Simulator UI, under the "`Inventory`" tab, click on the `+` button next to the Search field.

    ![Up](./src/images/upload_button.png)

1. Click on the "Upload" button and select the repo folder that you cloned.

    ![Create simulation model](./src/images/upload_model.png)

## Running the tests

1. In the API Simulator UI click on the "Play" tab.

    ![API Simulator Play tab](./src/images/play_tab.png)

1. Open one of the tests in the menu on the lefthand side, and click on the Play button in the upper righthand corner.

    ![Play test button](./src/images/play_test_button.png)

    ![Run results](./src/images/run_results.png)

## References

- [About API Simulation](https://documentation.tricentis.com/tricentis_cloud/en/content/topics/sim_intro.htm)
- [How to Simulate Services Locally using IRIS Simulator](https://tricentis.atlassian.net/wiki/spaces/TPI/pages/1540686575/How+to+Simulate+Services+Locally+using+IRIS+Simulator)
- [ServiceFusion repo](https://github.com/Tricentis-Product-Integration/ServiceFusion)
