The main Go distribution includes tools for [building](https://en.wikipedia.org/wiki/Build_automation "Build automation"), [testing](https://en.wikipedia.org/wiki/Unit_testing "Unit testing"), and [analyzing](https://en.wikipedia.org/wiki/Static_program_analysis "Static program analysis") code:

- `go build`, which builds Go binaries using only information in the source files themselves, no separate makefiles
- `go test`, for unit testing and microbenchmarks as well as fuzzing
- `go fmt`, for formatting code
- `go install`, for retrieving and installing remote packages
- `go vet`, a static analyzer looking for potential errors in code
- `go run`, a shortcut for building and executing code
- `godoc`, for displaying documentation or serving it via HTTP
- `gorename`, for renaming variables, functions, and so on in a type-safe way
- `go generate`, a standard way to invoke code generators
- `go mod`, for creating a new module, adding dependencies, upgrading dependencies, etc.

It also includes [profiling](https://en.wikipedia.org/wiki/Profiling_(computer_programming) "Profiling (computer programming)") and [debugging](https://en.wikipedia.org/wiki/Debugging "Debugging") support, [fuzzing](https://en.wikipedia.org/wiki/Fuzzing "Fuzzing") capabilities to detect bugs, [runtime](https://en.wikipedia.org/wiki/Run_time_(program_lifecycle_phase) "Run time (program lifecycle phase)") instrumentation (for example, to track [garbage collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science) "Garbage collection (computer science)") pauses), and a [data race](https://en.wikipedia.org/wiki/Data_race "Data race") detector.

Another tool maintained by the Go team but is not included in Go distributions is `gopls`, a language server that provides [IDE](https://en.wikipedia.org/wiki/Integrated_development_environment "Integrated development environment") features such as [intelligent code completion](https://en.wikipedia.org/wiki/Intelligent_code_completion "Intelligent code completion") to [Language Server Protocol](https://en.wikipedia.org/wiki/Language_Server_Protocol "Language Server Protocol") compatible editors.[[128]](https://en.wikipedia.org/wiki/Go_(programming_language)#cite_note-137)

An ecosystem of third-party tools adds to the standard distribution, such as `gocode`, which enables code autocompletion in many text editors, `goimports`, which automatically adds/removes package imports as needed, and `errcheck`, which detects code that might unintentionally ignore errors.