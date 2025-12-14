# This is just a file for scratch brainstorming, ideas, todo-list, things learned, navigation, etc.

## Navigation
` CMD/`: Entry points. When I add CLI client later, that will go into `cmd/client/`.

`internal/`: Basically, the core backend application code.

`pkg/pb/`: Where generated Go code goes after running `protoc` (protobuf definitions).

`proto/`: For `.proto` files. Edit here and then regenerate `pb/`.

## Notes for me
- Makefile was not originally designed as a task runner, but built as a file builder. This means that the Makefile describes how to produce files from other files.

- In Makefile, when we have the format of:
```
output_name: file1.go file2.go
    go build -o output_name
```
First, we're checking if a file called `output_name` exists. If not, we run the second line.

If it does, then second, we check `file1.go` and `file2.go` to see if they're newer than the existing `output_name` file. If `output_name` is older than any of those two file, we run the second command, otherwise we do nothing cause we're up to date.

- `go build ./...` will build Go files per package (since those packages all depend on each other), and will look through from the current directory (`.`) to go recursively down (`/...`).

- Makefile terminology for reference is below where target is the name of the thing, prerequisites are the dependencies, and the recipe are the indented shell commands (MUST BE INDENTED).
```
target: prerequisites
<TAB>command 1
<TAB>command 2
```

- Can use `go help build` to see all different flags for the `build` subcommand of Go.

- Default behavior of `go build` is to build the current package. If it's a `main` package, we produce a binary named after the directory, otherwise it just compiles and caches (no output file) [we can see where cache lives with `go env GOCACHE`; cleaning cache can be done with `go clean -cache`, but completely optional since there's auto garbage collection for it].

- Just for specifically `.PHONY` in the Makefile, the files listed after the colon are targets whose recipes will be ran EVERY time regardless of checks. They're not true prereq., they just use the same syntax as the Makefile structure as before, so it's kind of an exception.