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

### R2D24 - 2020/06/03

`[GCP-FUNDAMENTALS][GCP-SPECIALIZATION]`

Today I started taking the first course of Coursera's [Developing Applications with Google Cloud Platform Specialization](https://www.coursera.org/specializations/developing-apps-gcp), [Google Cloud Platform Fundamentals: Core Infrastructure](https://www.coursera.org/learn/gcp-fundamentals).

So far, the basic information was:

GCP offers 4 big categories of services:
1. Compute (Compute Engine, App Engine, Cloud Functions, Kubernetes and Cloud Run)
2. Storage (Cloud Storage, Cloud Firestore, Cloud SQL and many more)
3. Big Data
4. Machine Learning

Cloud Computing can be defined by these 5 characteristics:
* Self-Service (no-human interaction for allocating resources)
* Elastic Resources (it must be possible to get more resources as needed)
* Measured Service (pay only for whats it's use)
* Broad Network (access from anywhere)
* Resource Pooling (provider must shares resources to custumers)

GCP resources are organized in zones and regions (region contains multiple zones).

### R2D25 - 2020/06/04

Instead of working of my personal project or studying, I spent my time figuring out a BUG I implemented at work (found and fixed =P).

### R2D26 - 2020/06/06

`[GCP-FUNDAMENTALS][GCP-SPECIALIZATION]`

Continued my study of [GCP](https://www.coursera.org/specializations/developing-apps-gcp)...

GCP offers a few features for helping control spendings:
* **Budgets and Alerts**: It's possible to define multiple budgets and alerts on both Project and Resource level. You can set a Budget by either a literal value or by last month's spending percentage. It's possible to define any number of Alerts by percetages thresholds of your defined Budget (alerts at 50, 90 and 100% by default).
* **Reports**: GCP also offers beautiful (graphical) reports, that break down costs by resource operation's (for example: Firestore's costs are seperated by read, write, delete, storage, etc...).
* **Billing Export**: It's possible to export billing details to BigQuery and/or to Cloud Storage (csv or json formats).
* **Quotas**: Quotas exist to help both control spendings as well as making sure resources are available for all GCP clients. There are two types of quotas:
1. **Rate Quotas**: It's a number of operations allowed on a resource by a period of time. This quota resets, and the time depends on the resource and the operation.
2. **Allocation Quotas**: Define a limit of resource allocation at a Project level (for example, 105 App Engine services by project).

It's possible to interact with GCP in 4 different ways:
1. Console (Web Application)
2. SDK (provides 3 command line tools, `gcloud`, `gsutil` and `bq`)
3. API's
4. Mobile Application

All GCP resources are organized by Projects. If GCP has an Organization, it's Projects can also be organized using Files.

Every GCP Project has:
* **Project Name**: Defined by the user, it's mutable.
* **Project ID**: Defined by the user, immutable and unique.
* **Project Number**: Defined by GCP, immutable.

GCP contains an Identity Access Management (IAM or IM), that allows the creation of policies at Organization, Folder, Project and even at some Resource levels. IAM is a great tool for making sure the [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) is applied to every GCP Project (or organization).
IAM Policies are inherited and broader policies override more restrict ones.

Every policie contains 3 parts:
1. **WHO part**: Identifies a user. A user could be a Google account and even a GSuite domain. It's worth mentioning that when the _who_ is a software, GCP allows the use of a service-acocunt, which is just a google managed account that we can use for giving it IAM permissions. An example of a service-account being useful is if a Compute Engine instance (VM) needs permission for accessing another GCP resource.
2. **CAN DO WHAT part**: A role is a set of actions a given user can take on specific resources. GCP has 3 types of roles.
_Primitive_ roles have actions that apply to _all_ GCP resources.
_Pre-Defined_ roles have both fewer actions and resources than primitive roles (usually apply to just one resource).
_Custom_ roles user defined roles (usually combining _Pre-Defined_ Roles).
3. **WHICH RESOURCE part**: Identifies the resources for a Role.

### R2D27 - 2020/06/07

1. I waited a while for confirming that the issue (and fix) described on _R2D23_ actually worked. Today I deployed that fix.
2. After the new knowledge of IAM, service-account and roles described on yesterday studies, I've updated my defined service accounts (and their roles) for the Indicators API and the service-account I use locally for testing with GCP Client Libraries.

### R2D28 - 2020/06/08

`[GCP-FUNDAMENTALS][GCP-SPECIALIZATION]`

The next section of the GCP studies was about networking (VPC, Firewalls, etc...) and Compute Engine (VMs).

I'm very deficit on networking and I have never used a VM before (for nothing useful anyway). This lesson was not only difficult for me, but it was also somewhat out of scope (most resources I use on GCP are serverless).

I did manage to understand a couple of things, but I don't feel confortable writing it down just yet. I finished the exercises and moved on to the next section.

### R2D29 - 2020/06/09

`[GCP-FUNDAMENTALS][GCP-SPECIALIZATION]`

Every application must store data in some way.
GCP offers a variety of storage options for structure and unstructure data. The core offerings are Cloud Storage, Cloud SQL, Cloud Datastore/Firestore, Cloud Spanner and Cloud Big Table.

***

**Cloud Storage**

Cloud Storage is a Binary [Object Storage system](https://en.wikipedia.org/wiki/Object_storage), which means:
1. Each object contains it's own information (in bytes).
2. An object may contain metadata (set of key/value pairs).
3. Each object is indexed by a unique identifier.

GCS organizes it's objects in _Buckets_, and each bucket must have a unique identifier that is translated to a URL. This means that objects can be accessed on the web, by HTTPS requests.
Buckets have 6 properties, the first 4 are mandatory:
1. Global unique identifier (URL)
2. Region
3. Default storage class (Multi-Regional, Regional, Nearline and Coldline)
4. IAM policies and/or ACL (Access Control List)
5. Object versioning
6. Object lifecycle policies

There are 4 types of Cloud Storage classes:
1. Multi-Regional: Good for highly accessible objects with redundancy within a region
2. Regional: Also great for highly accessible objects, but with less redundancy
3. Nearline: Class meant for objects that are accessed only once a month
4. Coldline: Meant for objects that are accessed only once a year

**Cloud BigTable**

Cloud BigTable is a NoSQL database suitable for big data.

Each item is meant to have only one index, and some application developers like to think of BigTable as a _persistant hashtable_.

### R2D30 - 2020/06/10

`[GCP-FUNDAMENTALS][GCP-SPECIALIZATION]`

**Cloud SQL**

GCP offers tradional RDBMS with MySQL, PostgreSQL and/or SQL Server engines. Even thou it's also possible to host a RDBMS on a VM, the Cloud SQL databases are fully managed by Google and have a few features that make them very easy to use, such as scaling very well vertically, as well as security configurations and automatic replicas.

**Cloud Spanner**

Cloud Spanner is a relational database made by Google (with full support for SQL) that is suitable when the dataset size is too big for Cloud SQL and/or when the database also needs to scale horizontally.

**Cloud Datastore/Firestore**

[Cloud Firestore is the future of Datastore](https://cloud.google.com/datastore/docs/firestore-or-datastore).
Cloud Firestore is a NoSQL database, made for back-end applications. It's horizontally scalable, supports ACID transactions and has support for SQL-like queries.

### R2D31 - 2020/06/12

`[GCP-FUNDAMENTALS][GCP-SPECIALIZATION]`

**Containers**

The course's definition of containers start by comparing it to VM's.

VM's and Containers can have the same goals of packaging an application logic and it's dependencies into a self-contained unit, that could be run anywhere. The difference between the two is how they are implemented.

VM's virtualize the hardware, which means that each VM has it's own operating system, that can be configured and execute a given application. Because of this, VM's are highly configurable, but are not good when a service needs to scale, because VM's are "heavy" and each new instance would take maybe minutes to start, and consume a lot of resources.

Containers on the other hand, virtualize the operating system, which means they are lightweigth when compared to the footprint of a VM. Therefore, containers are great for scalability, because it's faster to start them, but they offer less configurability than VM's.

### R2D32 - 2020/06/13

Today I decided to take the [first course](https://www.qwiklabs.com/focuses/10531?parent=catalog) of the [Qwiklabs's Getting Started with Go on Google Cloud](https://www.qwiklabs.com/quests/129) quest.
It was basically a 1 hour course on how to deploy a Go application on App Engine, and understand how the app accessed data on both BigQuery and Firestore.

### R2D33 - 2020/06/14

Worked a little bit on my [calculators-app](https://calculadoras.app). I started implementing the LFT investment calculation. I've basically replicated all of the middlewares (CORS and pre-processing) and wrote a few helper functions. I need to re-visit how a [LFT investment is calculated](http://www.tesouro.fazenda.gov.br/documents/10180/258262/C%C3%A1lculo+da+Rentabilidade+dos+T%C3%ADtulos+P%C3%BAblicos+ofertados+via+Tesouro+Direto+-+LFT.pdf/582e2ca6-adab-459b-b841-ef33d4863b3b#:~:text=A%20LFT%20%C3%A9%20um%20t%C3%ADtulo,des%C3%A1gio%20no%20momento%20da%20compra1.) before writing the actual calculation.


`[Go]`

Continued my study of Go and Web Development from the [Go Web Programming book](https://www.manning.com/books/go-web-programming).

Chapter 2 (skipped) was the description of a completed Web Application written in Go.

Chapter 3 is about multiplexers (routers) and handlers.

**Summary of Chapter 3**

* While web frameworks can be a great way of developing web apps, using them without understanding which problems they are trying to solve, and how, can lead to [cargo cult programming](https://en.wikipedia.org/wiki/Cargo_cult_programming).
* It's possible to create a web app in Go, using only the standard libraries `net/http` and `html/template`.
* HTTPS is just the HTTP protocol on top of TLS, instead of TCP. The TLS protocol adds both encryption and authentication.
* The TLS protocol consists of the server respoding to a client with a SSL certificate (a form of X.509 certificate), that contains a public key and a signature from a Certificate Authority (CA). If the client trusts the signature, it's going to create a private key, for symetric encryption and encrypt this new key with the public key from the server's certificate. In Go, creating your own certificate, public key and starting an HTTPS server is trivial.
* Go's `net/http` package contain types for both client and server

Client:
1. Client
2. Response

Server:
1. Server
2. ServeMux
3. Responsewriter
4. Handler
5. HandlerFunc

For both Client and Server:
1. Request
2. Header
3. Cookies

### R2D34 - 2020/06/15

`[Go]`

Continued my study of Chapter 3 from [Go Web Programming book](https://www.manning.com/books/go-web-programming).

**Summary of Chapter 3**

_Server_: It's the struct with all of the server's configurations. The port, the multiplexer and everything else must be passed to the Server type.

_ServeMux_: It's a multiplexer (router). A multiplexer is responsible for directing each request to it's Handler, based on some variables such as the URL path and the HTTP method. In Go, every ServeMux also implements a `ServeHTTP` method, which means that every ServeMux is also a Handler. Go has a default multiplexer, _DefaultServeMux_, which is the ServeMux used on all calls from the `net/http` package where a ServeMux was not explicit.

_ResponseWriter_: It's the type responsible for writing the output to a HTTP response.

_Handler_: The `net/http` package defines a handler interface that implements just one method called `ServeHTTP`, that receives a `ResponseWriter` and a pointer to a `Request`. A handler therefore, is any type that implements the `ServeHTTP` method.

_HandlerFunc_: Is a function that has the same signature as the `ServeHTTP` method.

### R2D35 - 2020/06/22

`[Go]`

Continued my study of Chapter 3 from [Go Web Programming book](https://www.manning.com/books/go-web-programming).

Problems that exist throughout the source code and whose solution implementation would need to be done more than once, are known as _cross cutting concern_. Logging, error handling and authentication checking are some examples of cross cutting concerns.

One way of solving these problems just once, and have them apply anywhere, is to chain handlers and/or handler functions using the adapter pattern (in Python known as the decorator pattern). Go has support for function types, anonymous functions and closures. Chaining handler functions and handlers is fairly easy:

Example 1: Handler Function
```
func CheckAuthentication(hf http.HandlerFunc) http.HandlerFunc {
    return func(w http.ResponseWriter, r *http.Request) {
        // logic for checking authentication...
        hf(w, r)
    }
}
```

Example 2: Handler
```
func CheckAuthentication(h http.Handler) http.Handler {
    return http.HandlerFunc(func (w http.ResponseWriter, r *http.Request) {
        // logic for checking authentication...
        h.ServeHTTP(w, r)
    })
}
```

As mentioned "yesterday", _ServeMux_ is both a multiplexer and a handler in Go (because it implements the ServeHTTP method). As it turns out, the ServeMux is just a handler, whose ServeHTTP method has the logic for mapping different URLs to different handlers that have been previously registered. In other words, ServeMux implements routing by chaining.

_DefaultServeMux_ is just an instance of a ServeMux, that is publicly available and implicitly used when the functions `http.Handle` and `http.HandleFunc` are called. If the `ListenAndServe` method of a Server struct is called without specifying a handler, the DefaultServeMux is used automatically.

### R2D36 - 2020/06/23

`[Go]`

Started studying the fourth chapter of the [Go Web Programming book](https://www.manning.com/books/go-web-programming).

So far is has given a basic review of the Web and the Client-Server communication model.

### R2D37 -> 48  - 2020/06/30 -> 2020/07/10

Worked solely on the [Truck Driver Trips System API](https://github.com/RafaFT/truck-driver-trip-system).

Software is never finished, it can only be abandoned. I have a couple of ideas I want to change/implement on this API (had a lot of fun and learned a lot with it), so I don't intend to abandoned this completely just yet.

### R2D49 -> 51 - 2020/07/17 -> 2020/07/18

I've decided to dual-boot my laptop and start developing with Ubuntu (20.04) to match my work's every day environment and to learn more about Linux and bash.

I did [Ubuntu's intro tutorial on bash](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview) and learned a bit about basic commands, "operators" and the Linux filesystem.

I also wrote a one-time configuration bash file for installing and configuring some of my Ubuntu's workspace (such as google chrome, vscode, vscode extensions, git, insomnia, GCP SDK, define some aliases, environment variables, etc...)

Some of the commands I "learned":
`pwd`, `cd`, `mv`, `mkdir`, `rmdir`, `rm`, `echo`, `cat`, `less`, `sort`, `uniq`, `man`, `wc`, `ls`, `su` and `sudo` and a few _options_ (flags) to change their behavior.

Came in touch with some _operators_:
`&&`, `||`, `|` (pipe in awesome)

I also came in touch with the filesystem:
`/`, `~`, `/etc/environment`, `/etc/profile`, `/etc/profile.d/`, `~/.profile`, `~/.bashrc`, `~/.bash_aliases` and `~/.pam_environment`.

### R2D52 - 2020/07/19

Started re-writing the Indicators API (implemented today as GAE) as a Cloud Function. I've finished implementing the first handler.

### R2D53 - 2020/07/20

I did two things.

1. Finally learned how to push errors to the GCP's Error Reporting Service (which includes an e-mail and a notification push on Android) without having to raise a runtime error with `panic` (which would break a CF instance, hitting performance). It turns out I was trying to use the [wrong package (_logging_)](https://pkg.go.dev/cloud.google.com/go/logging), when I should be using the [errorreporting package](https://pkg.go.dev/cloud.google.com/go/errorreporting). Both packages are fairly similar (same concepts of async, flush and entry) and I think they are both awesome!

2. Implemented query string capabilities to the `getRecords` handler, with support for `from`, `to`, `order`, `limit` and `fields`.

### R2D54 - 2020/07/21

Today was a very productive day...

1. Created a couple more handlers for the Indicator's API.
2. Adjusted the query parameters for each handler (very similar to the implementation on the [Truck Driver Trip System API](https://github.com/RafaFT/truck-driver-trip-system)).
3. Started working on the `addIndicator` handler, and solving the [Firestore issue](https://github.com/googleapis/google-cloud-go/issues/2610) of not supporting custom types (and therefore marshal and unmarshal).

### R2D55 - 2020/07/22

Finished implementing the `addIndicator` handler of the Indicator's API, and also concluded the design and implementation of an Indicator Model struct, that can be used for both Firestore and JSON.

### R2D56 -> 66 - 2020/07/23 -> 2020/08/02

Worked on (yet) another major refactor on all of my micro-services.
After re-writing the Indicator's API as a CF, I realized a few changes that would be welcomed on the VNA and Metadata APIs as well.

1. Refactor all Firestore underlying collections.
2. Return json containing the error message on client errors (4XX).
3. Use the `errorreporting` library to report 5XX errors to Stack Driver (e-mail and app notification =P).
4. Change the way dates are stored on Firestore.

### R2D67 -> 68 - 2020/08/03 -> 2020/08/04

Worked on my tech-talks presentation of Go.

### R2D69 - 2020/08/05

All of my micro-services are exposed to the internet. In trying to understand how to access a CF that requires authentication, I learned a bit more about IAM policies.

1. Each IAM policy consists of one of more bindings, which are the association of members (the _who part_) with roles (the _can do what part_).
2. Each IAM policy must belong to a GCP resource. Organization, Folders and Projects are also considered resources, and by default, when associating a role to a member the policy belongs to the Project.
3. IAM policies are all inherited from child resources. This explains why a policy defined at Project level cannot be overwritten by a more restrict policy of a child resource.
4. I can call a "private" CF locally by generating an identity token from gcloud: `gcloud auth print-identity-token`, and writing the token in the `Authorization` header (bearer).
5. A CF can call another private CF by either generating an authentication token directly, or by using the `idtoken` library to generate a token on it's behalf. The calling CF must have the permission `cloudfunctions.functions.invoke`.

A few other things I learned was about member types (google account, service-account, g suite domain or google-groups), how roles are made up of individual permissions (that have the following format: `service.resource.action`) and how it's possible to get a client of a GCP client library even when using a service-account with insufficient permissions.

### R2D70 -> 71 - 2020/08/06 -> 2020/08/07

Worked and presented my tech-talks presentation of Go.

### R2D72 -> 73 - 2020/08/12 -> 2020/08/13

Continued studying React from the [pure-react book](https://purereact.com).

So far I've created a re-usable Tweet component, and I'm starting to learn the advantages and how to use PropTypes.

### R2D74 - 2020/08/15

Continued my study of [GCP](https://www.coursera.org/learn/gcp-fundamentals)

Here is a minor revision of what I "learned" so far...

1. A Cloud service must have the following characteristics:
* Resources must be self-service (no human interaction needed)
* Pay only for what you use
* Resources should be scalable (up and down)
* Resources should be highly available through network
* Cloud provider shares resources among customers

2. About IAM:
* IAM stands for Identity Access management, and allows the creation of IAM policies
* IAM policies are a collection of member(s) and role associations
* A member is an e-mail that identifies a user or a group of users (google account, service account, google groups or a G Suite domain)
* A role is a collection of permissions, that when associated to a member, determines what the member can do
* A permission is structured as `service.resource.action`
* There are 3 types of roles (primitive, pre-defined and custom)
* IAM policies must be associated to a resource
* Resources inherit IAM policies from their parent

3. About storage options:
* GCP has different storage options that include: Relational db, NoSQL db and Object db
* Relational dbs include Cloud SQL (MySQL, PostgreSQL and SQL Server) and Cloud Spanner (for horizontal scalability)
* NoSQL dbs include Cloud BigTable (for big data) and Cloud Firestore (successor of datastore, great for backend applications)
* Object db has Cloud Storage
* Cloud Storage organize objects into buckets
* Buckets have many configurations, that include: global unique name, region, storage class (multi-region, region, nearline and coldline), IAM policies and/or ACLs (Access Control List), object versioning and Life-Cycle rules

### R2D75 - 2020/08/16

The part I was stuck at the [GCP](https://www.coursera.org/learn/gcp-fundamentals) course was about containers and kubernetes... Therefore, I decided to read the official documentation and take the time to deploy the `calculate-indicator` logic from CF using Cloud Run.

Cloud Run is yet another GCP compute option, which is basically a serverless container.
Each service has a unique URL and may have multiple revisions (the latest healthy one is always served).
Some differences between Cloud Run and CF:

1. CR services can handle concurrent requests (up to 80)
2. Each service has access to a great CPU (+-2.4 ghz), which is decoupled from the amount of memory
3. The pricing for CPU milliseconds is the same across all requests that are processed at the same time

I did some testing and the calculate-indicator logic deployed on CR performed much better than CF..

For a single calculation, CR (0.04s) was faster than CF (0.12s), most likely because of the better CPU. In a spike of 1000 requests at the same time, the individual time execution was slower on CR (average of 1.5s), probably because 80 requests are more than one container can handle at the same time.

However, the time necessary for completing all 1000 requests was much lower and consistent on CR (+- 3s) than CF (max >1min, min 3s). My guess is that the biggest bottleneck on CF for a huge spike of requests is the time necessary to spin 1000 CF instances, while the CR needed to scale much less (1000/80).

### R2D76 - 2020/08/17

After my amazing experience using Cloud Run, and the successful deployment of the calculate-indicator logic using serverless containers, I could not help my self and I spent my time trying to implement a custom function on Google Sheets that calls my Cloud Run container.

Therefore, Today I studied the very basics of [Google Apps Script](https://developers.google.com/apps-script/overview) and I actually did create a custom function that made HTTP calls to my endpoint (the GAS docs are very good) and everything went OK.

In my tests, around 100 calculation took around 2-3 seconds, which was good enough.

### R2D77 - 2020/08/18

I got an [error](https://console.cloud.google.com/errors/CL3t59TCsIjfaQ?time=P1D&project=calculators-app) on the cron job for updating VNA values, where a Selic calculation returned a json where the `adjusted_end_date` was the same as the provided `end_date`, which doesn't make sense.

The bug is a wrong logic, where `adjusted_end_date` is defined without checking if it's equal to the `end_date` provided.

### R2D78 - 2020/08/19

Designed a calculation optimization by using binary search for finding the index of the records corresponding to the `start_date` (or it's adjusted value), very similar to Python's `bisect.bisect_left` implementation.

### R2D79 - 2020/08/21

Implemented the calculation optimization described on `r2d78` (for all indicators), while also fixing the bug from `r2d77`.

I also optimized the `tr` calculation further, by not iterating an array of birthday dates, but instead checking the date of each index more intelligently. After all of these optimizations, a realistic calculation (of +- 3 years) sometimes takes **25 milliseconds**, which is faster than I could have ever imagined!

### R2D80 -> 83 - 2020/08/22 -> 2020/08/25

Continued working on the calculate indicator implementation on Cloud Run.

In these 4 days I implemented the following changes:
1. Cache calculation by default (query string parameter for disabling cache).
2. getRecords receives the start date and returns only the necessary records.
3. Don't panic during runtime to prevent cold starts.
4. Use `errorreporting` to report Internal Server Errors.
5. Implemented all new logic and optimizations to the calculate-indicators CF.

OBS: After reading more about [caching](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching), I was assuming that simply setting the response header `Cache-Control: public, max-age=3600` would make cache work both locally and shared (proxy and CDN's), but I was surprised to noticed that the cache was only working locally (browser). I'm not entirely sure why this is happening, but I think I may need to enable Google's Cloud CDN service to take advantage of shared caching... I still need to work on this!
