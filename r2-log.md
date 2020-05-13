# #100DaysOfCode Log - Round 2 - Rafael

The log of my #100DaysOfCode challenge. Started on May 8, Friday, 2020.

## Log

### R2D1 - 2020/05/08

1. Added support for updating field values of an existing record from the Metadata Firestore collection. It's the first time I've implemented an HTTP handler that responds to a PATCH request.

2. I noticed that the VNA LFT records are being updated incorrectly (for each day that the records are updated, all of the old records are added as well, increasing costs). After some debugging, I found the origin of the problem on the API responsible for interacting with VNA records on Firestore.

### R2D2 - 2020/05/09

1. Fixed bug described on day 1. The VNA API GET route now has support for query parameters `start_date`, `end_date` and `order`.

### R2D3 - 2020/05/10

1. Finished implementing an API for interacting with the Firestore Metadata records. The API implementation is bothering me little, as a lot of my decision seem kind of hacky! Specially after I've found [this blog](https://drstearns.github.io/tutorials/) with a bunch of relevant Go patterns (middlewares, making values available to all handlers, etc...). The metadata API implements a GET, POST and PATCH method.

### R2D4 - 2020/05/11

1. Updated the POST handler on the Metadata API, to allow adding records without `latest_date` and/or `latest_calc_date`. Because those values cannot be known at the moment a record is being added for the first time (instead of using "0001-01-01" date values).

2. Spent a lot of time writing down and organizing some ideas about Metadata. The next steps are to adjust a few things on the Metadata API, and later create a routine to update Metadata records/values on both Firestore, as well as Cloud Storage.

### R2D5 - 2020/05/12

1. Continued working on the Metadata API... Made some adjustments (couple more in mind).

2. Modifying the way the update handler works proved to be more challenging than expected. I've started studying the `reflect` package, since it appears the be the best (correct) way of iterating both field names and field values of a struct.
