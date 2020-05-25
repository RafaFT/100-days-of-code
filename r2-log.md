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

### R2D6 - 2020/05/13

1. Changed the behaviour of both httpDoc and firestoreDoc structs to be more explicitly, and interchangeable between one another.

### R2D7 - 2020/05/14

1. Update handler implementation now correctly ignores (skips) nil values. This implementation does use the `reflect` package, but in a more limited way than expected (simply verifying the internal value of an empty interface variable).

### R2D8 - 2020/05/15

Today I made a bunch of "minor" adjustments on the Metadata API, which is now oficially _good enough_.

1. The Router's patterns now only matches lowercase alphanumeric characters.

2. Semantic validation of a document being added is done explicitly by the handler.

3. The handler for getting all documents now returns a JSON object (dict), instead of a list.

4. HTTP error status code are now more precise (ex: I can check failed firestore operation codes from [this](https://github.com/grpc/grpc-go/blob/master/codes/codes.go#L29) package, and return better HTTP error codes).

### R2D9 - 2020/05/16

Today I did not code at all.

With the Metadata API done (for now at least), I spent my time reading the docs and testing [Cloud Functions triggered by Cloud Firestore events](https://cloud.google.com/functions/docs/calling/cloud-firestore).

### R2D10 - 2020/05/17

1. Created a [(firestore) Cloud Function](https://cloud.google.com/functions/docs/calling/cloud-firestore) triggered by Metadata Firestore collection document events (create, update or delete), that updates metadata records on Cloud Storage.

### R2D11 - 2020/05/18

1. Spent some time reading documentation about how [notifications on Cloud Storage Bucket](https://cloud.google.com/storage/docs/pubsub-notifications) works. Notifications allow Cloud Storage Buckets to send messages to a Pub/Sub Topic (key value pairs and the Blob metadata), given a specific event.

2. I tried configuring notifications to the records Bucket using the Python's Cloud Storage Client, but it did not work. I was able to create a Notification class object, but I couldn't find a way to add it to the Bucket (had a similiar problem with CORS configuration in the past) nor save it. This was annoying. I checked the Go's Client library and it did have an [AddNotification](https://godoc.org/cloud.google.com/go/storage#BucketHandle.AddNotification) method on the Bucket struct... I settled by configuring the notifications using the `gsutil` command.

3. Removed deprecated logic for updating metadata.

### R2D12 - 2020/05/19

1. Created the first implementation (support for OBJECT_FINALIZE events) of the Cloud Function for updating metadata records on Cloud Firestore.

### R2D13 - 2020/05/20

1. Add support for OBJECT_DELETE events on the Cloud Function for updating metadata records on Cloud Firestore.

2. Add handler for PUT methods on the Metadata API.

3. Adjusted the front-end to the new Metadata URL and format. Now the end date input HTML also has a max value.
