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

You can also create a buffer of specified length for future use if you don't have the content yet:
```js
var buf = new Buffer(1024);
```

#### Getting and setting bytes in a buffer

You can access any byte of a buffer using the `[]` operator like so:
```js
var buf = new Buffer('my buffer content');
// access the 10th position of the buffer content
console.log(buf[10]); // -> 99
```
It's important to remember that initializing an empty buffer fills it with random data:
```js
var buf = new Buffer(1024);
console.log(buf[10]); // -> 5 (some random number);
```
You can also manipulate any point in the buffer using similar markup:
```js
buf[99] = 125; // sets the 100th byte to 125
```

NOTE In certain cases, some buffer operations will not yield an error. For instance:

1. If you set any position of the buffer to a number greater than 255, that position will be assigned the 256 modulo value.
2. If you assign the number 256 to a buffer position, you will actually be assigning the value 0.
3. If you assign a fractional number like 100.7 to a buffer position, the buffer position will store the integer part — 100 in this case.
4. If you try to assign a position that is out of bounds, the assignment will fail and the buffer will remain unaltered.

You can obtain the buffer length with the length propery:
```js
var buf = new Buffer(100);
console.log(buf.length); // -> 100
```
You can use the length to iterate over the bytes in a buffer, for example, if you wanted a buffer with length 100 and numbers 0-99 ascending:
```js
var buf = new Buffer(100);
for (var i=0; i < buf.length; i++){
	buf[i] = i;
}
```

#### Slicing Buffers
```js
var buffer = new Buffer('this is the content of my buffer');
var smallerBuffer = buffer.slice(8,15);
console.log(smallerBuffer.toString()); // -> "the content"
```
If you're concerned that you'll leak memory by keeping an old buffer around, use the copy method instead
#### Copying a buffer
```js
var buffer1 = new Buffer("this is the content of my buffer");
var buffer2 = new Buffer(11);

var targetStart = 0; 
var sourceStart = 8; 
var sourceEnd = 19;

buffer1.copy(buffer2, targetStart, sourceStart, sourceEnd); console.log(buffer2.toString()); // -> "the content"
```
Here, you are copying position 8 to 19 of the `source` buffer into position 0 of the `target` buffer.

#### Decoding a buffer
You can decode a buffer by using the `.toString()` method like so:
```js
var str = buf.toString();
var str = buf.toString('base64'); // convert to base64 string
```
