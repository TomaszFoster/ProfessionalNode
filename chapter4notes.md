## Using Buffers to Manipulate, Encode, and Decode Binary Data

1. Why you need buffers in node
2. Creating a buffer from a string
3. Converting a buffer to a string
4. Manipulating the bytes in a buffer
5. Slicing and copying a buffer

Node includes a binary-buffer implementation which is exposed in the JavaScript API as the `Buffer` pseudoclass.
A buffer length is set in bytes and you can randomly get and set bytes from a buffer.

```
/* 
Note:
A cool feature about this buffer class is that the memory where the data sits is allocated outside of the JavaScript VM memory heap. This means that these objects will not be moved around by the garbage-collection algorithm: It will sit in this permanent memory address without changing, which saves CPU cycles that would be wasted making memory copies of the buffer contents.
*/
```

#### Creating a Buffer
You can create a buffer from a UTF-8 encoded string like this:
```js
var buf = Buffer("Hello World");
// You can also specify the encoding as the second argument
var buf = Buffer("Hello World", 'base64');
```
Accepted buffer encodings:

1. ascii—ASCII. This encoding is limited to the ASCII character set.
2. utf8—UTF-8. This is a variable width encoding that can represent every character in the Unicode character set. It has become the dominant encoding on the web. This is the default encoding if you don’t specify one.
3. base64—Base64. This encoding is used to represent binary data in an ASCII string format by translating it into a radix-64 representation. Base64 is commonly used to embed binary data into textual documents in a way that ensures that the data remains intact during transport.

