# Work with multipart messages

Multipart or mixed MIME messages contain different types of content, such as images, videos, text, and so on. You can insert, verify, save, trigger, or buffer these message parts in your simulations without losing the other parts of those messages.

To define a multipart message, simply add the following rules properties to your simulation:

- type with the value multipart.
- file with the file path or value if you want to use a text string.
- optionally, use properties such as key to better identify message elements.

## Example - send multipart message

This is an example of how to send a multipart message:

- The insert property of the outbound step writes the text this is a text to `http://tricentis.com`. It also sends the image test_attachment.png and the file test_attachment_pdf to the same directory.

```yaml
schema: SimV1
name: default

services:
  - name: send multipart message
    steps:
      - direction: Out
        to: http://tricentis.com
        insert:
          - type: Multipart
            value: this is a text
          - type: Multipart
            key: my image
            file: images/test_attachment.png
          - type: Multipart
            key: my pdf
            file: pdf/test_attachment.pdf
```

## Example - receive multipart message

This is an example of how to verify a multipart message:

- The verify property of the inbound step checks whether the message contains an image named image_attachment.png and the text this is a text.

```yaml
schema: SimV1
name: default

services:
  - name: receive multipart message
    steps:
      - direction: In
        verify:
          - type: Multipart
            key: my image
            file: image_attachment.png
          - type: Multipart
            index: 0
            value: this is a text
```
