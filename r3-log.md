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
