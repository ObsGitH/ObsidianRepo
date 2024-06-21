# Methods
* method
	* argument
		* argumentExplanation
* subprocess.run()
	* returns a `CompletedProcess` object.
	* 'bash/windows' command
		* executes the bash/windows command and prints stdout  to console in the current directory.
	* shell
		* shell=True: allows you to run shell commands in windows and mac. Allow you to pass a entire command (command + args) as a string like `subprocess.run('ls -la', shell=True)`.
			- shell=True is a safety hazard so it should only be used when you have trusted input, use only you are passing in the arguments yourself.
		- if shell=False (default value for shell `arg`). Then: `subprocess.run(['ls', '-la'])` (the command and arguments have to passed as a list of strings)
	- `capture_output`
		- if set to True: captures output and redirects it  to the `CompletedProcess.stdout` attribute
	- `text`
		- if set to true: does not encode `stdout` and stderr to byte and keep it as a string
	- `stdout`
		- allows you to set where the `stdout` should be directed to.
			- `stdout=subprocess.PIPE`: redirects `stdout` to [[SubProcess Module#Classes and Objects]]
			- redirect `stdout` to a file:
				- `with open('file.txt', 'w') as f: p1 = subprocess.run(['ls', '-la'], stdout=f, text=True)`
			- subprocess.run(stdout=subprocess.DEVNULL) to discard the output of the command.
	- `stderr`
		- same thing of `stdout` argument but for `stderr`
	- `stdin
		- used to specify the source of input for the subprocess, which can be a file-like object or one of the special constants like `subprocess.PIPE` or `subprocess.DEVNULL`.
		- `stdin` is specified, the `input` argument is ignored.
		- if `stdin` is not specified, it is set to `None`, which means the subprocess will inherit the standard input of the parent process.
	- `input`
		- used to directly provide input data to the subprocess
		- It accepts a string or bytes-like object that will be sent to the standard input (stdin) of the subprocess.
		- `input` is specified, the `stdin` argument is ignored.
# Classes and Objects

- Class/Object
	- Methods
		- args
			- args explanation


- `CompletedProcess`
	- Attributes
		- Attr explanation
	- `returncode`: 
		- return 0 if no errors happened at execution and 1 if `CompletedProcess.stderr != None`
	- `args`: 
		- represents arguments passed to `subprocess.run()` as list or string 
	- `stdout`: 
		- he standard output of the process as a byte string. This attribute is `None` if `stdout` was redirected or if capturing output was disabled.
		- it is a byte by default. Can be turned into a string easily with `.decode()` method.
	- `stderr`: 
		- The standard error output of the process as a byte string. Like `stdout`, this attribute is `None` if stderr was redirected or if capturing output was disabled.
	- `check_returncode()`: 
		- A method that raises a `CalledProcessError` if the return code is non-zero. This is useful for enforcing that a subprocess call succeeded.
	- `from subprocess`: 
		- A class method for constructing a `CompletedProcess` object from `subprocess.Popen` objects. This method is mainly for internal use and not typically called directly by users.


- `subprocess.PIPE` - imitates a shell pipeline
	- `fileno()`: 
		- Returns the file descriptor of the pipe. This can be useful if you need to perform lower-level I/O operations.
	- `close()`: 
		- Closes the pipe. This is typically called automatically when the subprocess exits or when the file object associated with the pipe is garbage-collected.
	- `flush()`: 
		- Flushes any buffered data in the pipe. This is typically not necessary when using pipes in subprocess communication.
	- `read()`: 
		- Reads data from the pipe. This method blocks until data is available or the pipe is closed.
	- `readline()`: 
		- Reads a line of text from the pipe. Like `read()`, this method blocks until a line is available or the pipe is closed.
	- `readlines()`: 
		- Reads all lines from the pipe until it's closed or EOF is reached. This returns a list of lines.
	- `write(data)`:
		- Writes data to the pipe. This method blocks until all data is written or the pipe is closed.
	- `writelines(lines)`: 
		- Writes a list of lines to the pipe. This is equivalent to calling `write()` for each line in the list.


- `subprocess.DEVNULL` - represents a special file that discards all data written to it and returns an end-of-file (EOF) when read from.
	- used to redirect `stdout, stdin and stderr`. This effectively silences the output or error messages produced by the `subprocess` and for `stdin` it can be useful when you don't need to provide any input to the `subprocess`.
```python
import subprocess
# Run a command without providing any input
subprocess.run(["grep", "pattern"], stdin=subprocess.DEVNULL)
```
