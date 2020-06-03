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

### R2D14 - 2020/05/21

Now that the new Metadata information is correctly updated daily for each new indicator/vna, I spent my time basically replacing all of the uses of the old metadata for the new format.

1. Use the new Metadata on the indicators API.

2. Use new Metadata on the calculate-indicator API, and verify input parameters using the new fields _first\_calc\_date_ and _latest\_calc\_date_.

### R2D15 - 2020/05/22

With the calculate-indicator API now getting the new Metadata format, it doesn't have to access the workdays API anymore, for determining the _adjusted\_end\_date_. So today I:

1. Removed the use of workdays API on the calculate-indicator.

2. Wrote a script to profile the old calculate-indicator with it's new version. While the time necessary to perform calculations with dates within the indicator's range were virtually the same, calculations where the _end\_date_ was higher than the indicator's _latest\_date_ went from 0.75 to 0.15 seconds =).

### R2D16 - 2020/05/23

Today I merged feature and development branches into master. Spent the day writting tag annotations, taking care of deployments and services versions numbers... Still not done.

### R2D17 - 2020/05/24

Finally took the time to finish the last unit of the [Client-Server Communication Course](https://www.udacity.com/course/client-server-communication--ud897) from Udacity. The unit was about security on the Web and it talked about:

1. [same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) is a security mechanism that determines how resources from one origin can interact with resources from another one.

2. While same-origin policy protects the end user and is enforced by the user-agents, there are valid cases where origin A should be allowed to fetch resource(s) from origin B. [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) is a mechanism that standardizes how that can be accomplished, by using new HTTP headers.

3. [CSRF](https://pt.wikipedia.org/wiki/Cross-site_request_forgery) stands for Cross Site Request Forgery and is a type of attack where a WebSite makes a HTTP request to another site without the user knowing, trying to perform some kind of action that needs authorization by the user (such as transfer money from the user's account to it's own). A few words on why these attacks may work and how to prevent them...

**A**: A different origin can make a request to another origin programmatically, so a good CORS implementation, without the `Access-Control-Allow-Origin: *` is always a good idea. Althou in some cases fetch API is not even necessary, so same-origin policy and CORS would not help.

**B**: The attacked user is logged in and has an authentication cookie that the browser would send to the target origin (damn cookies).

**C**: A CSRF token can help prevent this attack (for example: a hidden input on a form could have a random token, that the server would try to match before performing it's action. Just a way to make sure the request came from the actual website hosting the form, not some
other origin).

4. The course also talked about Cross-Site Scripting (XXS) attacks (always sanitize and verify user inputs).

After finishing the course, I also read a blog post about [HTTP Sessions](https://drstearns.github.io/tutorials/sessions/).
The post was amazing and it taught me a lot about the workflow of a session, the differences between two types of tokens, a Digitally-Signed Session ID and JSON Web Token (JWT). It also talked about why a memory Database System comes in hand and two distinct systems of delivery (cookies and HTTP headers + local storage).

### R2D18 - 2020/05/25

Just like yesterday, today I spent my time studying. I read most of chapter one from [Go Web Programming book](https://www.manning.com/books/go-web-programming). I'm considering creating a new repo just for writing a summary of the book.

### R2D19 - 2020/05/26

Today I finished reading the chapter one from the [Go Web Programming book](https://www.manning.com/books/go-web-programming). It's the best introduction to the basic concepts of web applications and HTTP I've ever read.

Here are some of the concepts the book explained:

1. Reasons why Go is a great choice for building Web Applications (_scalable_, _maintainable_, _modular_ and _fast_)
2. Definition and differences between _Web Applications_ and _Web Services_
3. HTTP protocol (stateless, text based, request-response)
4. HTTP Request structure and HTTP methods (safe and idempotent)
5. HTTP Response structure and HTTP status codes
6. HTTP/2 and how it greatly improves performance (request and response multiplexing, compressed headers and binary based)

### R2D20 - 2020/05/27

Did a [HackerRank exercise](https://www.hackerrank.com/challenges/equality-in-a-array/problem)

### R2D21 - 2020/05/28

Finished Project Euler's exercise number 95. I remember attempting it in the past unsuccessfully =).

### R2D22 - 2020/06/01

Today I:

1. Finilized the deployment started on 2020/05/23.
2. Identified a bug and spent most of my time trying to understand it. I hope to have a fix by tomorrow, and an explanation of what happened and how it was fixed.

### R2D23 - 2020/06/02

The bug I mentioned yesterday was converting the `latest_date` and `latest_calc_date` fields of Metadata information (either on Cloud Storage or Firestore) on Null values, making calculations impossible.

**Issue**: The cloud function for updating Metadata on Firestore is triggered by Cloud Storage bucket events _OBJECT\_FINALIZE_ and _OBJECT\_DELETE_. The problem is that the CF would reset the field values to Null when an _OBJECT\_DELETE_ event happend, and these events are triggered even when a blob is not explicitly deleted, but also overwritten. This was my bad, since the [documentation on Cloud Storage Notifications](https://cloud.google.com/storage/docs/pubsub-notifications#events) makes this behaviour clear.

**Solution**: Removed the _OBJECT\_DELETE_ event listener from the Cloud Storage buckets (now only trigger update Metadata on _OBJECT\_FINALIZE_) and also removed the support and logic for delete events on the Cloud Function mentioned above.
