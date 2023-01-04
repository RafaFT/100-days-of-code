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
