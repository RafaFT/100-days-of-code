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
