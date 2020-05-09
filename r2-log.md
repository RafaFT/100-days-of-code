# #100DaysOfCode Log - Round 2 - Rafael

The log of my #100DaysOfCode challenge. Started on May 8, Friday, 2020.

## Log

### R1D1 - 2020/05/08

1. Added support for updating field values of an existing record from the Metadata Firestore collection. It's the first time I've implemented an HTTP handler that responds to a PATCH request.

2. I noticed that the VNA LFT records are being updated incorrectly (for each day that the records are updated, all of the old records are added as well, increasing costs). After some debugging, I found the origin of the problem on the API responsible for interacting with VNA records on Firestore.
