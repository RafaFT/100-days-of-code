# #100DaysOfCode Log - Round 3 - Rafael

The log of my #100DaysOfCode challenge. Started on December 28, Wednesday, 2022.

## Log

### R3D1 - 2022/12/28

I'm currently working on the new Workdays service.

My goal is to re-implement it with clean architecture, better doc, tests, benchmarks and other upgrades.
I also intend to use this service as a practice for containerizing the application, databases and having a development environment that allows me to choose which database implementation I want to use (mongo, mysql, firestore, in-memory, etc...).

I've already created the Workday entity, some usecases and I'm currently working on the controllers.


### R3D2 - 2022/12/29

I didn't code today.
Instead, I spent a lot of time mapping and understanding all the Federal and Banking laws that define what a baking service workday is.
Unfortunately, it was not as straight forward as I hoped.

### R3D3 - 2022/12/30

Continued working on understanding all brazilian workdays.
Started writing holiday's check as a new version, with way more documentation and reference for each day.

### R3D4 - 2022/12/31

Could not work on the holidays logic and documentation because of lack of internet.
However, I did update documentation on other packages.

I also decided to embrace the idea of having all of the workdays determined by a conjunction of selic values from BCB's API, from 1994, with ANBIMA's official holiday's list from 2001 to 2078, on the form of a CSV file.

This means the concept of a Workday entity is not technically necessary and I might remove it in the future.
Fow now, however, I added a couple of tests to make sure the workdays on the CSV file match the same workdays from NewWorkday function.

### R3D5 - 2023/01/01

Refactored and improved the tests from yesterday, for comparing the "different" implementation approaches of Workdays definition.
I basically wrote 3 tests:
1. one for verifying that all CSV dates are indeed Workdays, and checking total number.
2. one for verifying that generating all workdays from start to end resulted in the same expected total number.
3. one checking that the workdays from test 1 are the same as the ones from check 2.

Since the third test needed data from the first and second, I decided to use sub-testing for implementing all 3 tests inside the same parent.

I also had the idea of exposing the min and max possible workdays from the entity package as constant millisecond values. The trade-off is that the min and max dates are now finally "protected" and immutable (const milliseconds instead of var time.Time), but now each client package has to convert the exposed milliseconds to time.Time objects themselves.

### R3D6 - 2023/01/02

Played around a little with slog package (for structure logging), and started working on a new use-case for getting the number of workdays between two dates, instead of getting the actual dates.

### R3D7 - 2023/01/03

Finished the implementation of the new use-case for counting the number of workdays between two dates, WorkdaysCounter input port.
I also added tests and benchmarks for successful and error cases, and made sure test coverage was 100%, with the newly discovered `go test -coverprofile=cover.out` and `go tool cover -html=cover.out` commands.

### R3D8 - 2023/01/05

Worked on a big DRY refactor on the repository package.
Moved both in-memory repository implementations to the same file and made both implementation tests work on the same input and define single test functions that make use of sub-testing and sub-benchmarking for each implementation.

### R3D9 - 2023/01/16

I'm back! =)

Started working on a refactor/fix on date limits.

At both entity and usecase levels, workdays should be considered between [1994-01-01, 2079-01-01) (inclusive and exclusive).

The difference between the two layers is that at entity, the upper limit (2079-01-01) should be invalid and considered out of bounds. But at usecase, it makes sense to accept it, as it makes it easier to ask for workdays range. For example, for determining the workdays of December of 2078, it's easier to provide (2078-12-01, 2079-01-01), than (2078-12-01, 2079-12-31).

### R3D10 - 2023/02/27

I'm back! For reals this time =)

Studied a little bit the new experimental slog package, which is a package for structure logging with golang.

Updated my Go version to 1.20 (already have an eye on the new multiple errors handle technique) and started working on the Workdays refactor mentioned at D09.

### R3D11 - 2023/02/28

Finished the refactor from D09.
Workday entity date limit is 2078-12-31, but usecase for fetch considers 2079-01-01 as valid, which easies search queries.

### R3D12 - 2023/03/01

Fixed a pesky bug on WorkdaysRepository pre loaded implementation on the Fetch method. Which was ignoring the latest date, even when that date was lower than the provided end date filter. In other words, the end exclusive filter was happening even when it shouldn't.

I also implemented a new Count method for each WorkdaysRepository implementation.

Started working on a Trello board to keep track of everything.

### R3D13 - 2023/03/02

Continued working on mapping some issues on Trello.
The most obvious inconsistency was the way errors are being handled on testing. I actually made custom errors implement the `Is` method, which is satisfied if the types match (`As` exists for this already).
I guess at the time I was confused about differences between `Is` and `As`, and how to test errors.

### R3D14 - 2023/03/03

Continued working on mapping some issues on Trello.
The most obvious inconsistency was the way errors are being handled on testing. I actually made custom errors implement the `Is` method, which is satisfied if the types match (`As` exists for this already).
I guess at the time I was confused about differences between `Is` and `As`, and how to test errors.

### R3D15 - 2023/03/05

Normalize how different packages compare errors on testing, by always comparing by value with `errors.Is`, instead of comparing by types with `As`.
I also fixed the implementation of the `Is` method on the custom errors, which was unduly returning `true` if errors had same type.


### R3D16 - 2023/03/06

Started (re)-working on the HTTP controller API for exposing the get workday logic.
The code I wrote in the past is a little more complicated than it needs to, but I'm sort of proud of my self. I created custom errors for invalid path and query parameter values that might be sent, and also a HTTP aware error interface for wrapping those custom errors.

As always, there are a lot of questions and thoughts when it comes to decide who is responsible for writing the HTTP response body, status code and headers (I don't expect a single component being responsible for all), and things get more complicated and blur when I question how would this API (and others) support multiple output formats, like JSON or XML.
For now, I'll just try to keep things simple...

Found a bug on how the controller was writing the error messages. Using `http.Error` method actually appends a new line character at the end of the message and calls `w.Header().Set("Content-Type", "text/plain; charset=utf-8")`...


<!-- TODO -->
<!-- * better document and organize holidays check (this also should remove necessity for specialDates) -->
<!-- * improve documentation and make use of new go 1.19 doc features -->
<!-- * understand how stack traces work, and whether they should be defined at error creation or log -->
<!-- * make use of new structure log package from go - slog -->
<!-- * move workdays csv file to entity package/folder, and add tests to make sure the number of workdays matches the expected -->

### R3D3 - 2022/08/22

Started working on the use-case for getting all possible workdays.

I just realized how similar this use-case is to the generate workdays use-case. In the future, the workdays generator use-case will probably become an in-memory repository implementation, and the get all workdays use-case will call it.

I'm also not sure how to receive filter parameters, as struct, pointers, etc..

### R3D4 - 2022/09/06

Use case for returning all workdays now has default value for all of it's parameters if they are not provided, and created error types for each possible invalid parameter value.

### R3D5 - 2022/09/07

Created successful and error tests for new WorkdaysGetter usecase.
Worked on a bunch of minor refactors on both WorkdayGetter and WorkdaysGetter usecases.

### R3D6 - 2022/09/08

Created unique interface for all repository interfaces the Workdays service's usecase needs - WorkdaysRepository.
Worked on an in-memory repository implementation of WorkdaysRepository that works by generating workdays dynamically.
Basically finished the Find method and it's tests.

I'm considering changing two choices:
1. check if end date is really after start date on get workdays usecase
2. stop calculating limit value on get workdays usecase and let repository handle it, as it's a little awkward to calculate it and to use it inside repo

### R3D7 - 2022/09/09

Created unique interface for all repository interfaces the Workdays service's usecase needs - WorkdaysRepository.
Worked on an in-memory repository implementation of WorkdaysRepository that works by generating workdays dynamically.
Basically finished the Find method and it's tests.

### R3D8 - 2022/09/10

Created unique interface for all repository interfaces the Workdays service's usecase needs - WorkdaysRepository.
Worked on an in-memory repository implementation of WorkdaysRepository that works by generating workdays dynamically.
Basically finished the Find method and it's tests.

### R3D9 - 2022/09/12

Finished workdays repository generator implementation and started working on implementation that loads up workdays from CSV file.
I've basically implemented 3 different binary search implementations, one for each fallback option on the Find method.

### R3D10 - 2022/09/13

Finished the in-memory workdays repository implementation.
I'm very glad with the final result. I've implemented three different variations of binary search algorithms for looking up workdays on a workdays array. Benchmark results indicates this implementation is around 8 times faster for querying a specific date, and up to 100 faster for fetching multiple workdays than the generator implementation.

### R3D11 - 2022/09/14

Created Logger implementation that uses fmt.Println and started working on the controller for getting a specific workday.
Now that I'm on the controller - edge of the application - and started thinking about how the program actually runs, I'm having a bunch of thoughts on:

Logging:
1. How and when should I log errors? Some errors might originate at the repository, move to usecase and end up at the controller. At which step should I log the error event?
2. It might make sense to have a log implementation that has support for adding custom fields and values to be logged throughout the request lifecycle. This would be great for aggregating logs in the future.
3. Should each log identify the layer/component that is logging?

Errors:
1. I'm convinced I should define my own error types whenever possible and make my components be able to generate/handle them. The question is how far should this go? Should I really have a "presentation" client message for each possible error?
2. If I decide to log errors at the very edge of the application (most likely on controllers), I would like the error to have all information necessary. For example, if a SQL repository error is considered a usecase NotFound error, I think the log error ought to have the SQL details as well. How should I accomplish this?
3. Should my errors have stack trace like information, like filename, row and column?

### R3D12 - 2022/09/15

All of my user-defined errors now store the error's origin filename and line (I implemented this with struct embedding and custom Is() methods on all errors). This idea not only addresses my third concern about errors from yesterday, it also plays well with logging errors only once.

### R3D13 - 2022/09/17

Created HTTPError interface, which is meant to represent http aware errors that know it's response body and status code. So far, I've created 4 implementations, one for URL path errors, one for query parameter, another for resource not found and one for internal server errors.

I also implemented a function for concatenating all strings from an error and all of it's wrapped errors. This function addresses my 1º and 2º logging concerns.

### R3D14 - 2022/09/18

Finished initial implementation of HTTP controller for getting a specific workday. I've implemented the controller as a simple http.Handler interface to allow easier use of middleware's in the future. The controller cares only about the URL's path parameter and query parameters, it ignores other variables that I'll handle on an outer layer, such as HTTP methods and HTTP handler values.

Refactor repository in-memory implementation to depend on a struct variable, instead of a global variable as before.

### R3D15 - 2022/09/19

Fix bug on the loading of workdays for the in-memory repository implementation.
Started working on the HTTP controller for getting multiple workdays.
Even thou the design decision has merit, having the usecase expect golang types such as time.Time and int, instead of strings - which is what the controller receives on the path parameters - is annoying as I have to check/treat some values more than once.

### R3D16 - 2022/09/20

Refactor WorkdaysGetter.Get method to expect all filter values inside an exported struct, instead of having each filter value be a separate parameter. This change makes it easier to provide the filter values and also facilitates logging.

Continued working on the controller for getting workdays and I'm still having trouble on how to handle usecase errors on the controller.
It's bothering me a little how many custom errors and validation I'm doing. Validation on start_date is a good example. I check start_date is a valid iso 8601 string date on the controller - and return error if not -, but I can also receive an error from the usecase indicating a invalid start_date. Basically two different checks and two different errors for the same param, in different places. Perhaps the advantages of my current approach would be more clear on a system that has more than one controller accessing the same usecase..

### R3D17 - 2022/09/21

I was a little bit down today so I:
1. watched this [GopherCon talk](https://www.youtube.com/watch?v=6qAfkJGWsns) on memory profiling
2. read go blog page on [error handling](https://go.dev/blog/error-handling-and-go) techniques

The article made me feel a little better about how I'm handling errors on the Workdays service. Having custom error types and checking those types with type assertion appear to be common and maybe even an incentivize approach.

I really liked the `json.SyntaxError` example, which has an exported field that's not even shown in the `Error` method, but that a "sophisticated" caller can check for more details. Another interesting example was the `net.Error` interface, which is basically an extension of the `error` interface. This is very similar to my `HTTPError` interface idea.

I basically got some validation from go.dev itself =)

### R3D18 - 2022/09/27

Did some minor refactor on the controller for getting a workday.
Continued working on the controller for getting multiple workdays.

Here are next steps, not in a particular order:
* Rethink logger interface. I'm currently missing two features on the logger. Support for prefixing and accepting something other than string. It might make sense to make a Logger method signature similar to print.
* Implement print logger using golang's log package.
* Solve the error stack issue. I feel like I need to log the stack trace when I encounter a program exit early error. Currently, this is achieved by making all of my custom error's keep track of the function that created it and wrapping errors whenever possible. Together with a function for concatenating error strings from all errors in a chain, I theoretically would log all of the stack traces that generated errors in the chain. A better approach might be to simply generate the last X stack traces that generated an error only at time of logging. This could even be achieved by the Logger.Error method. This would simplify greatly my error handling.
* Checkout package [validator package](https://github.com/go-playground/validator) for simplifying input validation on the controllers
