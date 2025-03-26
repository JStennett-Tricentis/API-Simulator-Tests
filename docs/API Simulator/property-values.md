# Specify property values

In a simulation, steps properties represent relevant message elements, such as a header, payload, or name. To define these steps properties you can use rules and values. These values can be either static or dynamic.

## How can you use values?

This topic shows practical examples on how to use values in your simulations. The examples also explain which steps property you can use with which values.

## Use multiline values

YAML uses indentation instead of indicators to mark the structure. This is essential if you use multiline values. Make sure that these values are indented under the property.

### Example

In this example, you can see an indented multiline payload value.

```yaml
schema: SimV1

connections:
  - name: http

services:
  - name: Welcome
    steps:
      - name: Request
      - name: Response
        message:
          payload:
            <?xml version="1.0" encoding="utf-8"?>
            <soapenv:Envelope xmlns:soapenv="http://www.w3.org/2003/05/soap-envelope">
              <soapenv:Header xmlns:wsa="http://schemas.xmlsoap.org/ws/2004/08/addressing">
                  <wsse:Security soapenv:mustUnderstand="false" xmlns:wsse="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-secext-1.0.xsd">
                    <wsse:UsernameToken wsu:Id="UsernameToken-1" xmlns:wsu="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-wssecurity-utility-1.0.xsd">
                        <wsse:Username>acs_devel_111</wsse:Username>
                        <wsse:Password Type="http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-username-token-profile-1.0#PasswordText">PASSWORD</wsse:Password>
                    </wsse:UsernameToken>
                  </wsse:Security>
                  <wsa:To>http://sensetrunkcn1:1080/STS/services/SecurityTokenService</wsa:To>
                  <wsa:ReplyTo>
                    <wsa:Address>http://schemas.xmlsoap.org/ws/2004/08/addressing/role/anonymous</wsa:Address>
                  </wsa:ReplyTo>
                  <wsa:MessageID>urn:uuid:9667500f-4751-46c3-bb2a-5a2c27f757c6</wsa:MessageID>
                  <wsa:Action>http://docs.oasis-open.org/ws-sx/ws-trust/200512/RST/Issue</wsa:Action>
              </soapenv:Header>
```

## Use dynamic date and time values

You can use a dynamic expression to specify date and time values. This allows you, for instance, to define the current date as an identifier for a message.

- The base date is derived from the Coordinated Universal Time (UTC) and expressed in ISO 8601 format.
- Define date and time values with one of the step properties Insert, Verify or Trigger.

### Use current date and time values

The dynamic expressions DATE and TIME use the current date or the current time as base values.

The examples in the table use January 13, 2022 and 2:18 PM (14:18:00) as base values:

| Expression | Description | Example |
| ---------- | ----------- | ------- |
| {DATE} | Full date | 2022-01-13 |
| {TIME} | Current date and time | 2022-01-13T14:18:00Z |

### Calculate dates from a base date

You can calculate dynamic date and time values by defining a base date along with deviations.

Syntax: `{<EXPRESSION>[<Base date>][<Offset>][<Format>]}`

| Parameter | Description |
| --------- | ----------- |
| Base date | Specify a date value. If you leave the value empty, API simulation uses the current date as the base date. You can also use buffer values as the base date. |
| Offset | Define what to add or deduct from the base date. Offsets consist of a signed integer and a unit. These are the possible values:<br>- d: days<br>- M: month<br>- y: years<br>- h: hours<br>- m: minutes<br>- s: seconds |
| Format | Define the date format if the date format of the test object differs from the ISO 8601 format. The default value is yyyy-MM-dd. |

To apply the standard behavior to one of the parameters, leave the parameter value empty []. For instance, {DATE} and {DATE[][][]} have the same meaning. API simulation uses the current date without deviations.

### Examples

With the expression +3M-1d, you add three months to the base date and deduct one day.

The expression `{DATE[2022-05-23][+3M-1d][]}` returns 2022-08-22.

In the following examples, you use the value of a buffer as the base date:

- If the buffer date is 2022-11-10, the expression `{DATE[{B[Variable]}][+1M+1d][]}` returns 2022-12-11.
- If the buffer date is 2022-12-20, the expression `{DATE[{B[Variable]}][-1M][01.MM.yyyy]}` returns 01.11.2022.
- If the buffer date is 2022-12-24T12:34:56Z, the expression `{TIME[{B[Variable]}][+2d+1h][]}` returns 2022-12-26T13:34:56Z.

## Escape special characters

If you want to use the special characters },],{, [ and " as text in your simulations, first you must escape them. You can escape either individual characters or entire strings.

To do this, use double quotation marks (") before and after the individual character or string.

Syntax: `"<special characters>"`

### Example

In this example, you want to use the value {[Hi]} as plain text. Therefore, you enter "{[Hi]}".

The special character " requires an additional quotation mark to indicate that the subsequent " is part of the actual string and not the end of an escaped string.

### Example for "

In this example, you want to use the value {"Text"} as plain text. Therefore, you enter "{""Text""}".

## Use buffered values

If you need values of message elements within your virtual service more than once, use the property Buffer to temporarily save them. You can read out these buffer values within the virtual service and insert them into another message element.

### Create buffer values

To buffer a value, you need two things:

- A buffer name in order to find the buffer values again. Buffer names are not case-sensitive.
- A buffer with the step property buffer.

### Read out buffered values

To specify a buffered value for a message element, use the following syntax:

Syntax: `{B[<buffer name>]}`

- Use buffered values with the step property Insert.

### Example

In this example, you run a service named Service02:

- The inbound step contains two properties:
  - The trigger property to wait for a message containing any value as the value of the Path property.
  - The buffer property to create a buffer named myBuffer, which stores the value of the Path property. The property range specifies that you only want to store characters 1 to 5 of the Path property.
- The outbound step property insert writes the value of the buffer named myBuffer into the payload.

```yaml
schema: SimV1
name: buffer
description: set buffer and readout bufferd value

connections:
- name: myConnection
  port: 8080

services:

- name: Service02
  steps:
  - direction: In
    trigger:
    - uri: '*'
    buffer:
    - type: Path
      name: myBuffer
      range: 1..5
  - direction: Out
    insert:
    - value: '{B[myBuffer]}'
```

### Read out buffered values into the payload of a message

Typically, buffered values require a clear path to locate the section of the message you want to read. This can be a challenge with complex payloads, especially those with many special characters. You can use the following syntax to read a buffered value and insert it directly into the payload of a message.

Syntax: `%{<buffer name>}`

- Use buffered values within the payload property of a message.

### Example

In this example, you run a service named Service02:

- The inbound step contains two properties:
  - The trigger property to wait for a message containing any value as the value of the Path property.
  - The buffer property to create a buffer named myDate, which stores the value of the current date.
- The outbound step writes the value of the buffer named myDate directly to the payload.

```yaml
schema: SimV1
name: buffer
description: set buffer and readout bufferd value

connections:
- name: myConnection
  port: 8080

services:

- name: Service02
  steps:
  - direction: In
    trigger:
    - uri: '*'
    buffer:
    - name: myDate
      value: "{DATE}"
  - direction: Out
    message:
      payload: |-
        "{
          "a": {
            "c:" %{myDate}
          }
        }"
```

## Use split buffers

To split the value of a message element based on a separator, you can define a split buffer.

### Create split buffer values

To split a value and store it as a buffer, you'll need three things:

- A buffer name to find the buffer values again.
- A separator to identify the individual parts of a value.
- A buffer with the buffer step property.

Syntax: `{SPLITBUFFER[<Separator>]}`

When buffering, API simulation creates a separate buffer with consecutive numbers for each partial value.

### Read out buffered values

To specify a buffered value for a message element, use the buffer name along with the corresponding buffer value number:

Syntax: `{B[<Buffer name><number>]}`

- Use buffered values with the step property Insert.

### Example

In this example, you run a service named Service04:

- The inbound step contains two properties:
  - The trigger property to wait for a message that contains any value as the value of the Path property.
  - The buffer property to create a split buffer named myBuffer, which stores the value of the message payload. For each value separated by a semicolon, API simulation creates a separate buffer with consecutive numbers. For example, if the payload is 123;456;789;ABC, you get the following buffers: MyBuffer1 with the value 123, MyBuffer2 with 456, MyBuffer3 with 789, and MyBuffer4 with ABC.
- The insert outbound step property writes the value of the buffer named myBuffer1 to the payload: 123.

```yaml
schema: SimV1

- name: Service04
  steps:
  - direction: In
    trigger:
    - uri: '*'
    buffer:
    - name: myBuffer
      value: '{SPLITBUFFER[;]}'
  - direction: Out
    insert:
    - value: '{B[myBuffer1]}'
```

## Use XBuffers

With an XBuffer you can save a dynamic value within a static string in a buffer.

### Create XBuffer values

To buffer a value within a string you'll need two things:

- A buffer name to find the buffer values again.
- A buffer with the buffer step property.

Syntax: `<string>{XB[<Buffer name>]}<string>`

### Read out buffered values

Specify XBuffer values in the same way as buffer values.

### Example

In this example, you run a service named Service03:

- The inbound step contains two properties:
  - The trigger property to wait for a message that contains any value as the value of the Path property.
  - The buffer property to create two XBuffers named OrderID and CustomerID to store the values of the message payload. API simulation buffers the OrderID, which is located between Transfer order and has been created. It also buffers the CustomerID, which is located after for customer.
- The insert outbound step property writes the value of the buffer named OrderID to the payload.

```yaml
schema: SimV1

- name: Service03
  steps:
  - direction: In
    trigger:
    - uri: '*'
    buffer:
    - value: Transfer order {XB[OrderID]} has been created for customer {XB[CustomerID]}
  - direction: Out
    insert:
    - value: '{B[OrderID]}'
```

## Generate random text

You can create a random text of uppercase letters as a value for a message element. Specify the length of the text with a parameter.

Syntax: `{RANDOMTEXT[<Length of random text>]}`

- Define a random text with the step property Insert.

## Generate random number

You can create random numbers as a value for a message element.

- You can specify a random number with one of the following parameters:
  - To define the length of a random number, enter `{RND[<Length of random number>]}`. You can have a maximum of 19 digits.
  - To define an upper and lower limit of a random number, enter `{RND[<Lower limit>][<Upper limit>]}`. The lower limit must be less than the upper limit, and negative values are possible. The generated integer is a value that lies between the upper and lower limit and includes boundaries.
- Define a random number with the step property Insert

### Examples

This example creates a 7-digit number:

```yaml
schema: SimV1

- name: Service05
  steps:
  - direction: In
    trigger:
    - uri: '*'
  - direction: Out
    insert:
    - value: '{RND[7]}'
```

This example creates an integer number between -789 and 123:

```yaml
schema: SimV1

- name: Service06
  steps:
  - direction: In
    trigger:
    - uri: '*'
  - direction: Out
    insert:
    - value: '{RND[-789][123]}'
```

## Generate unique IDs

You can create a unique ID as a value for a message element. Unique IDs are generated consecutively, and each ID is assigned only once. Specify the length of the unique ID with a parameter.

Syntax: `{UNIQUEID[<length of uniqueID>]}`

- Define a unique ID with the step property Insert.

If there are not enough IDs available and the system cannot generate new IDs, an error occurs. Restart the environment to reset the unique IDs, or increase the length of the unique IDs.

### Example

In this example, you generate a unique ID that ranges from 0 to 9:

```yaml
schema: SimV1

- name: Service07
  steps:
  - direction: In
    trigger:
      - uri: '*'
  - direction: Out
    insert:
      - value: '{UNIQUEID[1]}'
```

## Use regular expressions

Use regular expressions to compare whether a searched value contains a string that matches the regular expression.

API simulation uses the .NET Framework syntax for regular expressions.

Syntax: `{REGEX["<regular expression>"]}`

- Define regular expressions with the Verify or Trigger step properties.

### Example 1: Telephone number

In this example, you run a service named Service03:

- With the trigger inbound step property, the service waits for a message that contains telephone numbers in various possible formats as the value of the Path property.
- The pattern includes:
  - (((00|\+)(420|43))|0): finds 00420, 0043, +420, +43, or 0
  - \s?: possible space
  - \d+: one or more digits
  - /?: possible forward slash
  - (\s?\d)+: string of digits with possible whitespace in between
- The possible search results include +43664 1234567, 0699 1234 5678 9, or 00420669/1 23 456 7.
- If the telephone number matches, the insert outbound step property writes the value valid phone number to the payload.

```yaml
schema: SimV1

- name: Service08
  steps:
  - direction: In
    trigger:
      - type: Path
        value: '{REGEX["(((00|\+)(420|43))|0)\s?\d+/?(\s?\d)+"]}'
  - direction: Out
    insert:
      - value: 'valid phone number'
```

### Example 2: Date

The expression `{REGEX["^\d{1,2}[ /.-]\d{1,2}[ /.-](\d{4}|\d{2})$"]}` searches for dates in various possible formats.

The pattern includes:

- ^: beginning of the line
- \d{1,2}: day (one or two digits long)
- [ /.-]: separators (space, forward slash, dot, or hyphen)
- \d{1,2}: month (one or two digits)
- [ /.-]: separators (space, forward slash, dot, or hyphen)
- (\d{4}|\d{2}): year (two or four digits)
- $: end of line

Possible search results are 9/3, 12.12.87, 1-1-1999, 11.1986, 04/23/2007, or 01.03.2000, 4.8.

### Example 3: ISO8601 date and time

Use the expression `{REGEX["^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}Z$"]}` to search for combined date and time in Coordinated Universal Time (UTC) in ISO 8601 format.

The pattern includes:

- ^: beginning of line
- \d{4}: year (four digits)
- -: separator (hyphen)
- \d{2}: month (two digits)
- -: separator (hyphen)
- \d{2}: day (two digits)
- T: delimiter between date and time
- \d{2}: exactly two digits
- :: separator (colon)
- \d{2}: hour (two digits)
- :: separator (colon)
- \d{2}: second (two digits)
- Z: time zone designator (UTC)
- $: end of line

Possible search results are 2013-10-03T12:00:00Z, 1776-07-04T13:37:11Z, or 1993-01-01T00:00:00Z.

## Generate random strings with regular expressions

You can generate random character strings that are limited by regular expressions. You must specify the regular expressions within double quotation marks.

Syntax: `{RANDOMREGEX["<Regular expression>"]}`

- Generate random strings with regular expressions with the Insert step property.

### Example

In this example, `{RANDOMREGEX["^[A-Z][a-z]+[0-9]{4}$"]}` creates a value that starts with an uppercase letter between A and Z, followed by any number of lowercase letters, and exactly four ciphers between 0 and 9. The ^ character marks the beginning of the line, and $ marks the end of the line.

This expression creates, for instance, Ecqwp1989.

## Perform calculations

You can perform calculations with the MATH expression.

Syntax: `{MATH[<Operand 1><Operator><Operand 2>..<Operator><Operand n>]}`

- Perform calculations with the Insert or Verify step properties.
- Use numerical values and scientific notations as operands.
- Operators are processed according to the PEMDAS rule. However, you can modify this behavior by using brackets.

You have to separate decimal places with a decimal point.

API simulation supports the following operators:

| Operator | Description |
| -------- | ----------- |
| +, -, *, / | basic arithmetic operations |
| % | Modulo operation |
| ^ | potentiation |
| == | equals |
| != | not equal |
| < | less than |
| > | greater than |
| <= | less or equal |
| >= | greater than or equal to |

### Example

This example illustrates how to increase the value of the buffer A by 1:

`{MATH[{B[A]}+1]}`

## Use confidential data

To use confidential data in your simulations you can store them in variables as:

- Environment variables
- Variables in the appsettings.yml file

This allows you to easily reference this data in the simulation files and prevent other users, who have access to these files, from viewing sensitive information.

### Set variables

If you set environment variables in your system, it's important that you use the prefix Simulator__Variables__ with the variable name, for example Simulator__Variables__My Secret Password.

If you set the variables in the appsettings.yml file, make sure you have administrator rights and follow these steps:

1. Open the appsettings.yml file.
2. Enter your variables in the Simulator settings under Variables with the following syntax: <Variable name>: <Value>.

Appsettings.yml example entry.

### Fetch data from variables

To fetch data from variables use the dynamic expression VAR.

Syntax: `'{VAR[<Variable name>]}'`

- Use variables with the step properties Insert or Verify.

### Example

In this example the step property Insert writes the value of the variable named my_password into the header.

```yaml
schema: SimV1

connections:
  - name: token provider
    endpoint: www.my-token-provider.com
    listen: false

services:
  - name: get valid token
    steps:
    - to: token provider
      insert:
        - type: Header
          name: authentication
          value: '{VAR[my_password]}'
    - buffer:
        - name: token
```

## Read data from a resource

You can work with resources to share data at runtime between different running simulations. The FROM expression is a way to use data from a resource.

Syntax: `{FROM[resource name][column][condition]}`

| Parameter | Description |
| --------- | ----------- |
| Resource name | Specify the name of the resource that contains the data you want to use. |
| Column | Specify the column in which you want to search. |
| Condition | Define a SQL WHERE clause to narrow the value you want to use. |

- Use the expression with the step properties Insert, Verify or Trigger.

### Example

In this example, we read data from a resource named movies, which is a CSV table of movies.

- The step trigger waits for a message with an uri property containing the value movies. If an incoming message meets this condition, API simulation uses an XBuffer to save the value of the property id.
- The next step inserts a value from the movies resource. Specifically, it reads the value of the title column from the row whose id matches the buffered id.

```yaml
schema: SimV1
name: Test01

resources:
- name: movies
  file: movies.csv

services:

- name: movies
  steps:
  - trigger:
    - uri: /movies/*
    buffer:
    - type: Path
      value: '/movies/{xb[id]}'
  - insert:
      - value: '{FROM[movies][title][id = "{B[id]}"]}'
```

## Use replacement value

You can use the TRY dynamic expression to enter a replacement value for a property if a specific dynamic value can't be found. For example, if you want to read a buffered value that doesn't exist and don't want an exception, use the replacement value.

Syntax: `{TRY[<dynamic expression>][<replacement>]}`

- Specify a TRY expression with the Insert step property.

### Example

In this example, we want to read data from a resource named movies using the FROM expression.

The insert step tries to insert a value from the movies resource. Specifically, it reads the value of the title column from the row whose id matches the buffered id. If it doesn't find a value, the replacement value movie not found is used.

```yaml
schema: SimV1
name: Test01

resources:
- name: movies
  file: movies.csv

services:

- name: movies
  steps:
  - trigger:
    - uri: /movies/*
    buffer:
    - type: Path
      value: '/movies/{xb[id]}'
  - insert:
      - value: '{TRY[{FROM[movies][title][id = "{B[id]}"]}][movie not found]'
```

## Specify a time stamp

The dynamic expression TIMESTAMP returns the number of milliseconds that have elapsed since 00:00:00 Coordinated Universal Time (UTC), Thursday, 1 January 1970.

Syntax: `{TIMESTAMP}`

- Specify a time stamp with the step property Insert.

## Use a counter

Use the dynamic expression COUNTER for instance, to set how many times a specified message should be sent or received before proceeding to the next step.

Syntax: `{COUNTER}`

- Use a counter with the step property Insert.