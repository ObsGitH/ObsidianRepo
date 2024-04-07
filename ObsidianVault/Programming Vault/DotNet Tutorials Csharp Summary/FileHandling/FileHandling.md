#### Things to remember: what is a file, what is a stream, the System.IO namespace file handling class hierarchy

A file is a collection of data stored on a disk with a specific name, extension, and directory path.

##### **What is Stream?**

A stream is a sequence of bytes travelling from a source to a destination over a communication path. There are two main streams: the Input Stream (Read a file) and the Output Stream (Write to a file).

a stream is a sequence of bytes that is used for communication. When you open a file for reading or writing, it becomes a stream. There are two types of streams for a single file.

##### **Why do I need to Learn File Handling in C#?**

We will not get a database everywhere to save the information and our project may require saving information in a TEXT file, DOC file, XLS file, PDF file, or any other file type. So, we must know the concept of saving data in a file


##### **System.IO Namespace in C#**

The parent class of file processing is Stream. Stream is an abstract class used as the parent of the classes that implement the necessary operations.

file handling class hierarchy in C#:

1. File, Path, Directory and DriveInfo derive directly from the Object class.
2. FileInfo and DirectoryInfo derive from FileSystemInfo class that derives from MarshalbyRefObject that derives from  Object


## **What is FileStream Class in C#?**

#### Things to remember from FileStream: FileMode, FileAccess and FileShare enumerations. using block, Encoding.Default.GetBytes() method, filestream.Write(byte[] buffer, int offset, int maxAmountOfBytes), and the enum SeekOrigin.
used to perform both Synchronous and Asynchronous Read and Write operations. With the help of FileStream Class, we can easily read and write data into files.

constructor parameters:

1. FileMode  **mode**: A constant that determines how to open or create the file. Creation mode.
2. FileAcess  **access**: A constant that determines read/write permission by the FileStream object. This also determines the values returned by the System.IO.FileStream.CanRead and System.IO.FileStream.CanWrite properties of the FileStream object. System.IO.FileStream.CanSeek is true if the path specifies a disk file.
3. **FileShare share**: constant that determines how the file will be shared by processes.


##### **FileMode Enum in C#:**

1. **CreateNew**: the operating system should create a new file. This requires a System.Security.Permissions.FileIOPermissionAccess.Write permission. If the file already exists, a System.IO.IOException exception is thrown.
2. **Create**: the operating system should create a new file like the CreateNew constant. But in this case, if the file already exists, it will be overwritten instead. This also requires System.Security.Permissions.FileIOPermissionAccess.Write permission. So, FileMode.Create is equivalent to requesting that if the file does not exist, use System.IO.FileMode.CreateNew; otherwise, use System.IO.FileMode.Truncate. If the file already exists but is a hidden file, then an UnauthorizedAccessException Exception is going to be thrown.
4. **Open**: It specifies that the operating system should open an existing file. The ability to open the file is dependent on the value specified by the System.IO.FileAccess Enumeration  value. A System.IO.FileNotFoundException exception is thrown if the file does not exist.
5. **OpenOrCreate**: It specifies that the operating system should open a file if it exists; otherwise, a new file should be created. If the file is opened with FileAccess.Read, System.Security.Permissions.FileIOPermissionAccess.Read permission is required. If the file access is FileAccess.Write, System.Security.Permissions.FileIOPermissionAccess.Write permission is required. If the file is opened with FileAccess.ReadWrite, both Systems.Security.Permissions.FileIOPermissionAccess.Read and System.Security.Permissions.FileIOPermissionAccess.Write permissions are required.
6. **Truncate**: It specifies that the operating system should open an existing file. When the file is opened, it should be truncated so that its size is zero bytes. This requires a System.Security.Permissions.FileIOPermissionAccess.Write permission. Attempts to read from a file opened with FileMode.Truncate causes a System.ArgumentException exception.
7. **Append**: It opens the file if it exists and then adds the content at the end of the file, or creates a new file. This requires a System.Security.Permissions.FileIOPermissionAccess.Append permission. FileMode.Append can be used only in conjunction with FileAccess.Write. Trying to seek a position before the end of the file throws a System.IO.IOException exception, and any attempt to read fails and throws a System.NotSupportedException exception.

	##### **FileAccess Enum in C#:**

1. **Read**–  Data can be read from the file. Combine with Write for read/write access.
2. **Write** – Data can be written to the file. Combine with Read for read/write access.
3. **ReadWrite** –  Data can be written to and read from the file.


##### **FileShare in C#:**

1. **None**: Declines sharing of the current file. Any request to open the file (by this process or another process) will fail until the file is closed.
2. **Read**: Allows subsequent opening of the file for reading. If this flag is not specified, any request to open the file for reading (by this process or another process) will fail until the file is closed. However, even if this flag is specified, additional permissions might still be needed to access the file.
3. **Write**: Allows subsequent opening of the file for writing. If this flag is not specified, any request to open the file for writing (by this process or another process) will fail until the file is closed. However, even if this flag is specified, additional permissions might still be needed to access the file.
4. **ReadWrite**: Allows subsequent opening of the file for reading or writing. If this flag is not specified, any request to open the file for reading or writing (by this process or another process) will fail until the file is closed. However, even if this flag is specified, additional permissions might still be needed to access the file.
5. **Delete**: Allows subsequent deleting of a file.
6. **Inheritable**: Makes the file handle inheritable by child processes. This is not directly supported by Win32.



Common methods and operations used with file streams:
1. byte[] byteData = Encoding.Default.GetBytes(string str): gets bytes from a string.
2. using (StreamReader streamReader = new StreamReader(fileStream)) // passing fileStream object as a //parameter to a StreamReader object.

{

//ReadToEnd method reads all characters from the current position to the end of the stream.

//It will return the rest of the stream as a string, from the current position to the end.

//If the current position is at the end of the stream, returns an empty string ("").

data = streamReader.ReadToEnd();

}

FileStream Methods:
1. FileStream.Close(): closes the stream represented by the FileStream object.
2. fileStream.Write(byte[] (serves as a buffer to hold the data that will be written) bytedata, int(offset. Starting point of the write operation in the file) 0, int(maximum number of bytes to write)bytedata.Length);
3. fileStream.Seek(int offset, SeekOrigin pointOfReference)

#### **Seek Origin Enumeration Values**:

1. **Begin:**
    
    - Represents the beginning of a stream. When you seek from the beginning, you specify an offset relative to the start of the stream.
2. **Current:**
    
    - Represents the current position within a stream. When you seek from the current position, you specify an offset relative to the current position.
3. **End:**
    
    - Represents the end of a stream. When you seek from the end, you specify a negative offset to move backward from the end of the stream.



## StreamReader and StreamWriter in C#:


##### **StreamWriter Class in C#**:

###### Remember: what is stream writer does, constructor parameters, 5 methods and 3 properties

StreamWriter class in C# is used for writing characters to stream in a particular format. Implements TextWriter abstract class.

Constructor:

public StreamWrite(string path, bool append, (determines encoding format)Encoding encoding, int bufferSize)

The Constructor is used to initialize a new instance of the **System.IO.StreamWriter** . Also decides with a bool if the writer is goin to do append write operations or, if bool = false, do just traditional write operations.

Has other overloaded versions with less parameters.

##### **Methods:**

1. **Close():**  closes  the underlying stream.
2. **Flush():**  Clears data from all buffers for the current writer and causes any buffered data to be written to the underlying stream. Used to clear the bufferrs for the current writer before closing the stream.
3. **Write():** Writes data to the stream. It has different overloads for different data types to write in the stream.
4. **WriteLine:**  same as Write() but it adds the newline character at the end of the data. 
5. **Dispose():** It releases the unmanaged resources used by the StreamWriter and optionally releases the managed resources.


##### **Properties:**

1. **AutoFlush:** Gets or sets a value indicating whether the StreamWriter will flush its buffer to the underlying stream after every call to System.IO.StreamWriter.Write(System.Char).
2. **BaseStream:** Gets the underlying stream that interfaces with a backing store.
3. **Encoding:** Gets the System.Text.Encoding in which the output is written.

##### **StreamReader Class in C#**:


##### Things to remember: what stream reader does and its traits, understand the constructor parameters, the  5  methods and the 3 properties. Common things to use with StreamReader.,
reads characters from a byte stream in a particular encoding. UTF-8 Encoding by default.

1. Use this class for reading a standard text file.
2. By default, it is not thread-safe.
3. Implements TextReader abstract class.

Most complex constructor:

public StreamReader(Stream stream, Encoding encoding, bool detectEncodingFromByteOrderMarks, int bufferSize, bool leaveOpen)

It initializes a new instance of the StreamReader class for the specified stream or string path based on the specified character encoding, byte order mark detection option, and buffer size, and optionally leaves the stream open.


Common Method with StreamWriter:
Close()
**Seek():** It is used to read/write at a specific location from a file.


Common method to use with StreamWriter:

StringBuilder.Concatenate() + streamWriter.Read() -> to glue the bytes together more efficiently.

while (strData != null)

{
// this loop prints all lines of text in a stream
Console.WriteLine(strData);

strData = streamReader.ReadLine();

}

Unique Methods (compared to StreamWriter):

1. **Peek():** This method returns the next available character but does not consume it. An integer represents the next character to be read, or -1 if there are no characters to be read or if the stream does not support seeking.
2. **Read():** This method reads the next character from the input stream and advances the character’s position by one character. The next character from the input stream is represented as a System.Int32 object, or -1 if no more characters are available.
3. **ReadLine():** This method Reads a line of characters from the current stream and returns the data as a string. The next line from the input stream, or null if the end of the input stream is reached.

4. ReadToEnd(): reads all data in a stream.

##### **Properties:**

1. **CurrentEncoding:** It gets the current character encoding that the current System.IO.StreamReader object is using.
2. **EndOfStream:** It gets a value that indicates whether the current stream position is at the end of the stream.
3. `**BaseStream`:** It returns the underlying stream.



## **File Class in C#**:

#### do not memorize it, just understand it.

Static class that provides some static methods to perform most file operations, such as Creating a File, Copying and Moving a File, Deleting Files, and working with `FileStream` to read and write streams. Exists because sometimes we want to work with files directly.

Pay attention to the fact that this is a static file, this methods work with all files. You can call this method with any file and you need to pass the files path to every method.

Methods:
1. **Copy**:  used to Copy an existing file to a new file. Overwriting a file of the same name is not allowed.
2. **Create**: creates or overwrites it in the specified path.
3. **Decrypt**: used to Decrypt a file encrypted by the current account using the System.IO.File.Encrypt(System.String) method.
4. **Delete**: used to delete the specified file.
5. **Encrypt**: encrypt a file so that only the account used to encrypt the file can decrypt it.
6. **Open**:  Open a System.IO.FileStream on the specified path, having the specified mode with reading, write, or read/write access and the specified sharing option. Returns a FileStream object.
7. **Move**: Move a specified file to a new location, providing the option to specify a new file name.
8. **Exists**: determine whether the specified file exists.
9. **OpenRead**: open an existing file for reading.
10. **OpenText**:  opens an existing UTF-8 encoded text file for reading. Returns a StreamReader object.
11. **OpenWrite**: open an existing file or create a new file for writing.
12. **ReadAllBytes**: open a binary file, read the file’s contents into a byte array, and then close the file.
13. **ReadAllLines**: Open a file, read all lines of the file with the specified encoding, and then close the file.
14. **ReadAllText**: Open a text file, read all the text in the file, and then close the file.
15. **ReadLines**: read the lines of a file.
16. **Replace**: replace a specified file’s contents with another file’s contents, delete the original file, and create a backup of the replaced file.
17. **WriteAllBytes**: create a new file, write the specified byte array to the file, and then close the file. If the target file already exists, it is overwritten.
18. **WriteAllLines**: create a new file, write the specified string array to the file, and then close the file.
19. **WriteAllText**: create a new file, write the specified string to the file, and then close the file. If the target file already exists, it is overwritten.
20. CreateText():  takes the path of the file to be opened for writing. It creates or opens a file for writing UTF-8 encoded text. If the file already exists, then its content will be overwritten. The File.CreateText(filePath) method returns an instance of StreamWriter class.



If there any parameters that are hard to understand of these methods they should be explained here:




**File Security C#**:


1. **`FileSecurity` Class:**
    
    - The `FileSecurity` class is part of the `System.Security.AccessControl` namespace and represents the access control and audit security for a file. It allows you to manipulate the discretionary access control list (DACL) and system access control list (SACL) associated with a file.
2. **`FileSystemAccessRule` Class:**
    
    - This class represents a rule that specifies access control information for a file or directory. It includes information such as the identity of the user or group to which the rule applies, the type of access allowed or denied, and the inheritance properties.
3. **`File.GetAccessControl` Method:**
    
    - The `File.GetAccessControl` method retrieves the `FileSecurity` object associated with a file. This allows you to examine and modify the security settings of the file.
4. **`File.SetAccessControl` Method:**
    
    - The `File.SetAccessControl` method applies access control information from a `FileSecurity` object to a file.

Here's a simplified example:

```csharp
using System; using System.IO; using System.Security.AccessControl;
class Program
{     
	static void Main()     {         
	string filePath = "example.txt"; 
	// Get the current access control settings for the file         
	FileSecurity fileSecurity = File.GetAccessControl(filePath);          // Add a new access rule for a user         FileSystemAccessRule accessRule = new FileSystemAccessRule("DOMAIN\\Username", FileSystemRights.Read, AccessControlType.Allow);
	fileSecurity.AddAccessRule(accessRule);          // Apply the modified access control settings to the file         File.SetAccessControl(filePath, fileSecurity);          Console.WriteLine("File security updated successfully.");
	}
}
```
`


## **TextWriter and TextReader in C# with Examples**:

#### Things to remember: Text Writer and Text Reader are abstract so they cannot be instantiade and are supposed to be implemented by child classes, but we can create reference variables of these two data types. This also means that they can be implemented on our own user-defined readers and writers classes. Synchronized(). The classes that are derived from these 2 classes.
##### **What is TextWriter Class in C#?**

a writer that can write sequential series of characters. We can use this TextWriter class to write text in a file. It is an abstract base class of StreamWriter and StringWriter, which write characters to streams and strings respectively.

It is the base class of StringWriter and StreamWriter, so instances of these child classes can be held as literals in TextWrite reference variables.


1. By default, it is not thread-safe.
2. As it is an abstract class, its object cannot be created. Any class implementing TextWriter must minimally implement its Write(Char) method to create its useful instance.

Should not be applied in practice, but understood. Who knows, maybe you will need to create your own user defined writer class and your knowledge of this class will be crucial for that.

##### **Methods of TextWriter class in C#:**


1. **Synchronized(TestWriter textwriter)**: It is used to Create a thread-safe wrapper around the specified TextWriter.
2. **Close():** It Closes the current writer and releases any system resources associated with the writer.
3. **Dispose():** It releases all resources used by the System.IO.TextWriter object.
4. **Flush():** It Clears all buffers for the current writer and causes any buffered data to be written to the underlying device.
5. **Write(Char):** It is used to write a character to the text stream.
6. **Write(String):** It is used to write the string to the text stream.
7. **WriteAsync(Char):** It is used to write the character to the text stream asynchronously.
8. **WriteLine():** It is used to write a line terminator to the text stream.
9. **WriteLineAsync(String):** It is used to write the string to the text stream asynchronously followed by a line terminator.

many overloaded versions of Write and WriteAsync methods available in TextWriter class.


classes that implement text writer class:
1. **IndentedTextWriter**: It is used to insert a tab string and to track the current indentation level.
2. **StreamWriter**: It is used to write characters to a stream in a particular encoding.
3. **StringWriter**: It is used to write information to a string. The information is stored in an underlying StringBuilder.
4. **HttpWriter**: It provides an object of TextWriter class that can be accessed through the intrinsic HttpResponse object.
5. **HtmlTextWriter**: It is used to write markup characters and text to an ASP.NET server control output stream.


##### **What is TextReader class in C#?**

a reader that is used to read text or sequential series of characters from a text file. It is an abstract class which means you cannot instantiate it. It is an abstract base class of StreamReader and StringReader (only those two) which are used to read characters from stream and string respectively.

##### **Methods of TextReader Class in C#:**

1. **Synchronized():** create a thread-safe wrapper around the specified TextReader.
2. **Close():**  close the TextReader and release any system resources associated with the TextReader.
3. **Dispose():**  release all resources used by the TextReader object.
4. **Peek():** read the next character without changing the state of the reader or the character source. Returns the next available character without actually reading it from the reader. It returns an integer representing the next character to be read, or -1 if no more characters are available or the reader does not support seeking.
5. **Read():** read the next character from the text reader and advances the character’s position by one character. It returns the next character from the text reader, or -1 if no more characters are available. The default implementation returns -1.
6. **ReadLine():** read a line of characters from the text reader and returns the data as a string. It returns the next line from the reader, or null if all characters have been read.
7. **ReadToEnd():** read all characters from the current position to the end of the text reader and returns them as one string. That means it reruns a string that contains all characters from the current position to the end of the text reader.



The advantage of working with using block is that it releases the memory acquired by the textReader/textWriter as soon as we move from the using block.


## **BinaryWriter and BinaryReader
**

##### **What is BinaryWriter Class in C#?**

used to write Primitive type data types such as int, uint, or char in the form of binary data to a stream.  Writes binary files that use a specific data layout for its bytes.

1. The BinaryWriter in C# creates a binary file that is not human-understandable but the machine can understand it.
2. It supports writing strings in a specific encoding. UTF-8 = default
3. In order to create an object of BinaryWriter, we need to pass an object of Stream to the constructor of the BinaryWriter class.

**Costructor**:

public BinaryWriter(Stream(fileStream that will receive the output of the BinaryWriter writer) output, Encoding(encoding format) encoding, bool(close the stream or not after this write is disposed) leaveOpen)


##### **Methods of BinaryWriter Class in C#:**

1. **Write(String):** write a length-prefixed string to this stream in the current encoding of the BinaryWriter and advances the current position of the stream in accordance with the encoding used and the specific characters being written to the stream.
2. **Write(float):** write a four-byte floating-point value to the current stream and advances the stream position by four bytes.
3. **Write(long):**  write an eight-byte signed integer to the current stream and advances the stream position by eight bytes.
4. **Write(Boolean):**  write the one-byte Boolean value to the present stream; 0 represents false while 1 represents true.
5. **Write(Byte):**  write an unsigned byte to the present stream and then it advances the position of the stream by one byte.
6. **Write(Char):** write Unicode characters to the present stream and also it advances the present stream position according to the character encoding used and according to the characters being written to the present stream.
7. **Write(Decimal):**  write a decimal value to the present stream and also it advances the position of the current stream by sixteen bytes.
8. **Write(Double):** write an eight-byte floating-point value to the present stream and then it also advances the position of the current stream by eight bytes.
9. **Write(Int32):** write a four-byte signed integer to the present stream and then it advances the position of the current stream by four bytes.


 storing files as Binary format is the best practice for space utilization.


##### **What is BinaryReader class in C#?**

###### Points to remember: binaryWriter/Reader traits and what they do, constructor parameters.

If you have a binary file (with .bin extension) stored in your machine and if you want to read the binary data then you need to use the BinaryReader class in C#. A binary file stores data in a different layout that is more efficient for machines but not convenient for humans. BinaryReader makes this job simpler and shows you the exact data stored in the binary file.



##### **Methods of BinaryReader Class in C#:**

The BinaryReader class in C# provides many Read() methods to read different primitive data types from a stream.

1. **Read():**  read characters from the underlying stream and advances the current position of the stream in accordance with the Encoding used and the specific character being read from the stream. It returns the next character from the input stream, or -1 if no characters are currently available.
2. **ReadBoolean():** read a Boolean value from the current stream and advances the current position of the stream by one byte. It returns true if the byte is nonzero; otherwise, false.
3. **ReadByte():** read the next byte from the current stream and advances the current position of the stream by one byte. It returns the next byte read from the current stream.
4. **ReadChar():** read the next character from the current stream and advances the current position of the stream in accordance with the Encoding used and the specific character being read from the stream. It returns a character read from the current stream.
5. **ReadDecimal()**: read a decimal value from the current stream and advances the current position of the stream by sixteen bytes. It returns a decimal value read from the current stream.
6. **ReadDouble()**: read an 8-byte floating-point value from the current stream and advances the current position of the stream by eight bytes. It returns an 8-byte floating-point value read from the current stream.
7. **ReadInt32():** read a 4-byte signed integer from the current stream and advances the current position of the stream by four bytes. It returns a 4-byte signed integer read from the current stream.
8. **ReadInt64():** read an 8-byte signed integer from the current stream and advances the current position of the stream by four bytes. It returns an 8-byte signed integer read from the current stream.
9. **ReadString():**  read a string from the current stream. The string is prefixed with the length, encoded as an integer seven bits at a time. It returns the string being read. One string == one line of the .bin file.


Constructor:

**public BinaryReader(Stream input, Encoding encoding, bool leaveOpen)**

self explanatory if you know the BinaryWriter constructor.





The BinaryWriter and BinaryReader Classes in C# are used to read and write primitive data types and strings. If you deal only with primitive types and you dont care about the file content being human readable, then this is the best stream to use.



## **StringWriter and StringReader in C#**

`StringWriter` and `StringReader` are used to manipulate 1 string. That's because all the operations done by theses to class instance's only alter their underlying `StringBuilder` instance.
###### points to remember:  StringWriter/Reader traits and how they work, constructor parameters of both. ToString(),GetStringBuilder() StringWriter methods.

##### **What is StringWriter Class in C#?**

derived from the TextWriter class and this StringWriter class is mainly used to manipulate strings rather than files. The StringWriter class enables us to write a string either synchronously or asynchronously. So, we can write a character either with Write(Char) or WriteAsync(Char) method and we can write a string with Write(String) or WriterAsync(String) method. The text or information written by StringWriter class is stored inside a StringBuilder. The Text namespace and the strings can be built efficiently using the StringBuilder class because strings are immutable in C# and the Write and WriteLine methods provided by StringWriter help us to write into the object of StringBuilder. StringBuilder class stores the information written by StringWriter class.

`StringWriter` is not for writing files on the local disk. It is used for manipulating strings and it saves information in it's underlying `StringBuilder` instance.


#### Constructors:

public StringWriter(StringBuilder sb, IFormatProvider formatProvider):
 
 initializes a new instance of the StringWriter class that writes to the specified StringBuilder and has the specified format provider. The parameter formatProvider specifies a System.IFormatProvider object that controls formatting.

#### Properties:

1. **Encoding**: This property is used to get the Encoding in which the output is written. So, it returns the Encoding in which the output is written.


##### **Methods of StringWriter Class in C#**

1. **Close():** lose the current StringWriter and the underlying stream.
2. **Dispose():** release the unmanaged resources used by the System.IO.StringWriter and optionally releases the managed resources.
3. **FlushAsync():** asynchronously clear and flush all buffers for the current writer and causes any buffered data to be written to the underlying device (string builder instance).
4. **GetStringBuilder():** return the underlying StringBuilder.
5. **ToString():** return a string containing the characters written to the current StringWriter so far.
6. **Write(char value):** write a character to the string.
7. **Write(string value):**  write a string to the current string.
8. **WriteAsync(char value):**  write a character to the string asynchronously.
9. **WriteAsync(string value):**  write a string to the current string asynchronously.
10. **WriteLine(String):**  Write a string followed by a line terminator to the text string or stream.
11. **WriteLineAsync(string value):**  write a string followed by a line terminator asynchronously to the current string.


##### **What is StringReader Class in C#?**
 StringReader class is mainly used to manipulate strings rather than files. This StringReader class is built using a string  and Read and ReadLine methods are provided by StringReader class to read the parts of the string. The data read by the StringReader class is the data written by the StringWriter class. The data can be read in a synchronous manner or in an asynchronous manner


the only StringReader Constructor:

**public StringReader(string s):** It initializes a new instance of the StringReader class that reads from the specified string. Here, the parameter “s” specifies the string to which the StringReader should be initialized.

##### **Methods of StringReader Class in C#**

The StringReader class in C# provides the following methods.

1. **Close():** close the StringReader.
2. **Peek():** return the next available character but does not consume it. It returns an integer representing the next character to be read, or -1 if no more characters are available or the stream does not support seeking.
3. **Read():** read the next character from the input string and advances the character position by one character. It returns the next character from the underlying string, or -1 if no more characters are available.
4. **ReadLine():**  read a line of characters from the current string and returns the data as a string. It returns the next line from the current string, or null if the end of the string is reached.
5. **ReadLineAsync():**  read a line of characters asynchronously from the current string and returns the data as a string. It returns a task that represents the asynchronous read operation. The value of the TResult parameter contains the next line from the string reader or is null if all the characters have been read.
6. **ReadToEnd():**  read all characters from the current position to the end of the string and returns them as a single string. It returns the content from the current position to the end of the underlying string.
7. **ReadToEndAsync():**  read all characters from the current position to the end of the string asynchronously and returns them as a single string. It returns a task that represents the asynchronous read operation. The value of the TResult parameter contains a string with the characters from the current position to the end of the string.
8. **Dispose():** release the unmanaged resources used by the System.IO.StringReader and optionally releases the managed resources.


## **What is FileInfo Class in C#?**

#### things to remember: what it is. Also what the methods do.


FileInfo is a sealed class in C# is used for manipulating files such as creating, deleting, removing, copying, opening, and getting information.


#### **Constructor**:

**public FileInfo(string fileName)** -> represent the file in the file path. It initializes a new instance of the System.IO.FileInfo class, which acts as a wrapper for a file path. The parameter fileName specifies the fully qualified name of the new file or the relative file name. Do not end the path with the directory separator character.>)


##### **Properties of FileInfo Class in C#**

The FileInfo Class provides the following properties.

1. **Directory**: get an instance of the parent directory. It returns a DirectoryInfo object representing the parent directory of this file.
2. **DirectoryName**: gets a string representing the directory’s full path. It returns a string representing the directory’s full path.
3. **Length**: get the size, in bytes, of the current file. It returns the size of the current file in bytes.
4. **Name**: get the name of the file.
5. **IsReadOnly**: get or set a value determining whether the current file is read-only. It returns true if the current file is read-only; otherwise, false.
6. **Exists**: get a value indicating whether a file exists. It returns true if the file exists, false if it does not exist, or if it is a directory.


##### **FileInfo Class Methods in C#**

The FileInfo class in C# provides the following methods.

1. **public StreamWriter AppendText():** get a value indicating whether a file exists. It returns true if the file exists, false if it does not exist, or if it is a directory.
2. **public FileInfo CopyTo(string destFileName):**  copy an existing file to a new one, disallowing the overwriting of an existing one. It returns a new file with a fully qualified path. The parameter destFileName specifies the name of the new file to copy to.
3. **public FileInfo CopyTo(string destFileName, bool overwrite):** copy an existing file to a new one, allowing the overwriting of an existing one. It returns a new file or an overwrite of an existing file if overwrite is true. If the file exists and overwrite is false, an IOException is thrown. The parameter destFileName specifies the name of the new file to copy to, and the parameter overwrites specifies true to allow an existing file to be overwritten; otherwise, false.
4. **public FileStream Create():** creates and returns a new file.
5. **public StreamWriter CreateText():** creates a StreamWriter that writes a new text file.
6. **public void Decrypt():** decrypt a file encrypted by the current account using the System.IO.FileInfo.Encrypt method.
7. **public override void Delete():** permanently deletes a file.
8. **public void Encrypt():** encrypt a file so that only the account used to encrypt the file can decrypt it.
9. **public FileSecurity GetAccessControl():** get a System.Security.AccessControl.FileSecurity object that encapsulates the access control list (ACL) entries for the file described by the current System.IO.FileInfo object. That means this method returns a System.Security.AccessControl.FileSecurity object that encapsulates the access control rules for the current file.
10. **public void MoveTo(string destFileName):** moves a specified file to a new location, providing the option to specify a new file name. Here, the destFileName parameter specifies the path to move the file to, which can specify a different file name.
11. **public FileStream Open(FileMode mode):** opens a file in the specified mode. It returns a file opened in the specified mode, with read/write access and unshared
12. **public FileStream Open(FileMode mode, FileAccess access):**  open a file in the specified mode with read, write, or read/write access. It returns a System.IO.FileStream object opened in the specified mode, accessed, and unshared.
13. **public FileStream Open(FileMode mode, FileAccess access, FileShare share):** opens a file in the specified mode with read, write, or read/write access and the specified sharing option. It returns a FileStream object opened with the specified mode, access, and sharing options. Here, the parameter mode specifies a System.IO.FileMode constant specifying the mode (for example, Open or Append) in which to open the file. The parameter access specifies a System.IO.FileAccess constant specifying whether to open the file with Read, Write, or ReadWrite file access, and the parameter share specifies a System.IO.FileShare constant specifying the type of access other FileStream objects have to this file.
14. **public FileStream OpenRead():** creates and returns a new read-only System.IO.FileStream.
15. **public StreamReader OpenText():** creates System.IO.StreamReader with UTF8 encoding that reads from an existing text file. It returns a new StreamReader with UTF8 encoding.
16. **public FileStream OpenWrite():** creates a write-only System.IO.FileStream. It returns a write-only unshared System.IO.FileStream object for a new or existing file.
17. **public FileInfo Replace(string destinationFileName, string destinationBackupFileName):** replace the contents of a specified file with the file described by the current System.IO.FileInfo object, deleting the original file and creating a backup of the replaced file. It returns a System.IO.FileInfo object that encapsulates information about the file described by the destFileName parameter.
18. **public void SetAccessControl(FileSecurity fileSecurity):** apply access control list (ACL) entries described by a System.Security.AccessControl.FileSecurity object to the file described by the current System.IO.FileInfo object.
19. **public override string ToString():** returns the path as a string.



## **What is DirectoryInfo in C#?**
#### just understand what it does, the properties and the methods.

The DirectoryInfo sealed class contains almost a similar feature as the FileInfo class of C#. The only difference is that the DirectoryInfo focuses only on the Directory, not the file systems. So, when we talk about the DirectoryInfo class, we talk about the physical directory. With its help, we get the object with which we can create, delete, and also, we can make some subdirectorys, and many more operations can be performed.

It provides several methods to perform operations related to directories and subdirectories.

#### Constructor:

**public DirectoryInfo(string path):** It initializes a new instance of the DirectoryInfo class on the specified path.

##### **Properties of DirectoryInfo Class in C#**

The DirectoryInfo class provides the following properties.

1. **Parent**: get the parent directory of a specified subdirectory. It returns the parent directory or null if the path is null or if the file path denotes a root (such as “\”, “C:”, or * “\\server\share”).
2. **FullName**: get the full path of the directory. It returns a string containing the full path.
3. **Name**: get the name of this System.IO.DirectoryInfo instance. It returns the directory name.
4. **Exists**:  get a value indicating whether the directory exists. It returns true if the directory exists; otherwise, false.
5. **Root**:  get the root portion of the directory. It returns an object that represents the root of the directory.
6. **CreationTime**: get or set the creation time of the current file or directory. It returns the creation date and time of the current System.IO.FileSystemInfo object.
7. **LastAccessTime**: get or set the time the current file or directory was last accessed. It returns the time that the current file or directory was last accessed.
8. **LastWriteTime**: get or set the time when the current file or directory was last written. It returns the time the current file was last written.
9. **Extension**:  get the string representing the extension part of the file. It returns a string containing the System.IO.FileSystemInfo extension.



##### **DirectoryInfo Class Methods in C#**

The DirectoryInfo class in C# provides the following methods.

1. **Create():**  create a directory.
2. **Create(DirectorySecurity directorySecurity):** creates a directory using a DirectorySecurity object. The directorySecurity parameter specifies the access control to apply to the directory.
3. **CreateSubdirectory(string path):** creates a subdirectory or subdirectory on the specified path. The specified path can be relative to this instance of the DirectoryInfo class. The parameter path specified path.
4. CreateSubdirectory(string path, DirectorySecurity directorySecurity): creates a subdirectory or subdirectories on the specified path with the specified security. The specified path can be relative to this instance of the DirectoryInfo class. The parameter path specified path. This cannot be a different disk volume or Universal Naming Convention (UNC) name. The directorySecurity parameter specifies the security to apply.
5. **Delete():** delete the DirectoryInfo if it is empty.
6. **Delete(bool recursive):**  delete this instance of a DirectoryInfo, specifying whether to delete subdirectories and files. The recursive parameter specifies true to delete this directory, its subdirectories, and all files; otherwise, it is false.
7. **EnumerateDirectories():**  returns an enumerable collection of directory information in the current directory. It returns an enumerable collection of directories in the current directory.
8. **EnumerateFiles():** returns an enumerable file information collection in the current directory. It returns an enumerable collection of the files in the current directory.
9. **GetAccessControl():** get the DirectorySecurity object that encapsulates the access control list (ACL) entries for the directory described by the current DirectoryInfo object. This method returns a DirectorySecurity object that encapsulates the access control rules for the directory.
10. **GetDirectories():** returns the subdirectories of the current directory. It returns an array of System.IO.DirectoryInfo objects.
11. **GetFiles():** returns a file list from the current directory. It returns an array of type System.IO.FileInfo.
12. **MoveTo(string destDirName):** moves a DirectoryInfo instance and its contents to a new path. The parameter destDirName specifies the name and path to which to move this directory. The destination cannot be another disk volume or a directory with an identical name. It can be an existing directory to which you want to add this directory as a subdirectory.
13. **SetAccessControl(DirectorySecurity directorySecurity):** set access control list (ACL) entries described by a DirectorySecurity object. The parameter directorySecurity specifies an object that describes an ACL entry to apply to the directory described by the path parameter.
14. **ToString():** It returns the original path that the user passed.