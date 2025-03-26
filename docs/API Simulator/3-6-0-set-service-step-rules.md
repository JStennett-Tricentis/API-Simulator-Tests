# Set service step rules

A virtual service is a sequence of steps that simulate message flows. A service step rule specifies the action, in combination with a rule, that the step performs. For instance, you can define rules for when a simulation should send a message, or which message a simulation should wait for before sending a response.

The following properties determine how to use the value of a rule:

- For inbound messages, use the properties Trigger, Save, Verify, and Buffer.
- For outbound messages, use the properties Insert, Save, and Buffer.

## Trigger

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

## Verify

This property compares a specified value in inbound messages with the value received from a service. This lets you ensure that a service works as expected.

To verify values, follow these steps:

1. Add the Verify property to an inbound message.
2. Enter the expected value. You can use literal and regular expressions.

## Buffer

This property temporarily stores the value of a message, so that you can reuse it within a virtual service. You can insert these values into messages or use them for verifications. Make sure to assign meaningful names to your buffer values, so you can find them again.

This property has the following specifications:

- Save values from in- or outbound messages as a buffer.
- Use buffer values to insert them into outbound messages.
- Use buffer values in the same message. For instance, to verify other message elements in the message.

## Insert

With this property you can insert message elements into outbound messages.

It has the following specifications:

- Enter a value to insert the message element with the specified value.
- If you leave the value empty, API simulation inserts the message element without a value.

## Save

With this property you can save message payloads or other properties of a message to a specific file.

It has the following specifications:

- Save values from in- or outbound messages to a file.
