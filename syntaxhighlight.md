
## API (Beta)

### Custom traces

**Note: This API is still in beta, so it might change in upcoming versions.**

If you want to track and time a custom event that happens in your application during a transaction, you can add a new trace to an existing transaction.

In the example below we create an Express app that:

1. times how long it takes to receive the body of an HTTP POST or PUT request
1. and times how long it takes to parse JSON sent by the client

```js
var opbeat = require('opbeat').start()
var app = require('express')()

// body reader middleware
app.use(function (req, res, next) {
  if (req.method !== 'POST' && req.method !== 'PUT') return next()

  // `buildTrace` will only return a trace if there's an active transaction
  var trace = opbeat.buildTrace()

  // start the trace to measure the time it takes to receive the body of the HTTP request
  if (trace) trace.start('receiving body')

  var buffers = []
  req.on('data', function (chunk) {
    buffers.push(chunk)
  })
  req.on('end', function () {
    req.body = Buffer.concat(buffers).toString()

    // end the trace after we're done loading data from the client
    if (trace) trace.end()

    next()
  })
})

// JSON parser middleware
app.use(function (req, res, next) {
  if (req.headers['content-type'] !== 'application/json') return next()

  var trace = opbeat.buildTrace()

  // start the trace to measure the time it takes to parse the JSON
  if (trace) trace.start('parse json')

  try {
    req.json = JSON.parse(req.body)
  } catch (e) {}

  // when we've processed the json, stop the custom trace
  if (trace) trace.end()

  next()
})

// ...your route handler goes here...

app.listen(3000)
```

### Custom transactions

**Note: This API is still in beta, so it might change in upcoming versions.**

Opbeat works by grouping incoming HTTP requests to your application into logical buckets. Each HTTP request is recorded in what we call a transaction. But if your application is not a regular HTTP server, Opbeat will not able to know when a transaction starts and when it ends.

If  for instance your application is working as a background job processing worker or is only accepting WebSockets, you'll need to manually start and end transactions.

#### `opbeat.startTransaction(name[, type])`

The `startTransaction` function takes two arguments:

- `name` - The name of the transaction. This is the value you'll see in the list of transactions on the Performance page on opbeat.com. All transactions with the same `name` will be grouped together.
- `type` - The type of the transaction. This string is used as a key to group the transactions into different tabs on opbeat.com. It defaults to `request` if not set.

A Transaction object is returned.

#### Transaction

##### `.result`

Set this property to a numeric value similar to the HTTP status codes to indicate the result of the transaction. E.g. `200` for success or `500` if an error occurred.

```js
trans.result = 200
```

##### `.end()`

Call this function to end the transaction.

#### Example usage

Example of a background job application polling the SQS queuing system for jobs:

```js
var opbeat = require('opbeat').start()
var sqs = require('simple-sqs')()

// listen for jobs on the queue
var queue = sqs(queueUrl, function (msg, cb) {
  // The SQS queue will send multiple messages using an array of records
  msg.Body.Records.forEach(function (job) {
    // start one new transaction for each job record received on the queue
    var name = 'Job ' + job.type
    var type = 'job'
    var trans = opbeat.startTransaction(name, type)

    // call the function that actually processes the job
    processJob(job, function (err) {
      // if the job could not be processes, set the result to a 5xx error code
      trans.result = err ? 500 : 200 // 500 indicates error, 200 is ok

      // end the transaction
      trans.end()

      cb(err)
    })
  })
})

queue.on('error', function (err) {
  // in case the queue encounters an error, report it to Opbeat
  opbeat.captureError(err)
})
```
