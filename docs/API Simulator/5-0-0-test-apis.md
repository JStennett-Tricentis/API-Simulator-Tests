# Test APIs

Application programming interfaces (APIs) allow different software applications to communicate with each other and exchange information. API testing involves sending requests to the API and analyzing the responses to ensure that they behave as expected.

To test an API, you need one or more API actions. These API actions contain all the information you need to communicate with the API, including connection, message, verifications, or required authentication. Once you have created an API action, you can run a single test on demand by sending a request, or create an API test case to use it in multiple test cases that you create in the test case editor.

## Create API action

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

## Define verifications

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

## Create API test case

To create an API test case, follow these steps:

1. Go to API simulation > API playground.

2. Select an existing API action from the list, or create a new one.

3. Select Create API test case.

4. In the menu bar on the left, select Test cases to find your new API test case.

5. Select an API test case to open it in the test case editor to further design your test case.

## Test API action

1. Go to API simulation > API playground.

2. Select an existing API action from the list or create a new one.

3. Select Send.

4. On the Response tab of your message you can see the following results:
   - The status code in the response header. It shows you if the message was successfully sent to the API.
   - The result of your verifications if you set any.

## Delete API action

To delete an API action, go to API playground and select the API action you want to delete from the left pane. Then select Delete.
