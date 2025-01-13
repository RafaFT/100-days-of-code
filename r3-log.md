# #100DaysOfCode Log - Round 3 - Rafael

The log of my #100DaysOfCode challenge. Started on Jan 5, Sunday, 2025.

## Log

### R3D1 - 2025/01/05

Started working on chapter-two of the [Powerful Command-Line Applications in Go](https://pragprog.com/titles/rggo/powerful-command-line-applications-in-go/).
Created the initial naive implementation of the TODO API, with `Add`, `Complete`, `Get` and `Save` methods and it's related tests.

### R3D2 - 2025/01/06

Continued working on the TODO application.
Minor adjustments to existing TODO API's and I was surprised by how difficult it was to find out which error type I should ignore on `Get`
method for file not found (`fs.ErrNotExist`).

Created the initial TODO CLI and started working on it's integration tests with `func TestMain` and `testing.M`, which were both
new to me.
CLI applications should write error to STDERR an exit with 0 or non-zero codes.

### R3D3 - 2025/01/07

Continued working on the TODO application.
Finished integration tests and started working on the new version, which is going to use multiple flag/options to define behavior.
Currently supporting `-list`, `-task` and `-complete` to list, add and mark a task completed, respectively.

It isn't clear to me how to best handle multiple flags yet. Some flag combinations might be fine, but other will most likely be incompatible
and how should the CLI handle such cases?

### R3D4 - 2025/01/08

Continued working on the TODO application.
Refactored CLI for supporting new flags and added integration tests for it.

Currently, the CLI exists with an error if no flag was provided, and it considers only the first flag found.
I'm still not sure how to handle multiple flags and was surprised that flag.<Type> method has no way of knowing whether a value was provided or not.
This means It's not possible to distinguish the default value from the user's input when they match.

I've also learned about flag.Usage and flag.PrintDefaults functions, which seem a great way of customizing CLI output for invalid flags and
for displaying customizable doc.

### R3D5 - 2025/01/12

Continued working on the TODO application.
Finished the chapter and initial implementation, now I just have to finish the optional exercises.

Interesting knowledge from today's session is that CLI tools are usually designed to be used by humans AND other tools.
This seems obvious after thinking about bash's pipe `|`.

Allowing CLI tools to being composable include using code exits, accepting arguments from stdin, writing correctly to stdout/stderr, supporting
different output formats, human and/or machine readable and I'm sure, many others things.
