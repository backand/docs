Background jobs (also known as cron jobs) allow you to run an action, query, or external URL request on a schedule that you define. Simply set the time and frequency, and the desired task will be executed automatically by Backand's servers. With this mechanism, you can run periodic queries for things like reports, recurring actions to control your application's behavior, or regularly post to external services.

## Create a job

To create a new Background job, click on "+ New Job" in the "Background jobs" section of the navigation bar. Simply add a name, a description, a start time or frequency, and set the type of job (Action, Query, External URL) to execute.

#### Advanced Options

In the advanced options, you can configure the job to operate via a POST or GET request. The GET request allows you to configure the query string used in the request, as well as modify the headers sent - allowing you to send additional parameters to the job when it executes. For POST requests, you can modify both the headers and the query string, but you are also given the Request Data field which you can use to send additional parameter formats, such as long text.

This can be very useful in distinguishing between two jobs that run the same action. For each job, you can send a "type" parameter to indicate the specific job being run. You can accomplish this by setting the "Query String" field to the appropriate value: 'parameters={type:1}' for job 1, and 'parameters={type:2}' for job 2.

## Update an existing job

You can update existing job by selecting the background job from the navigation menu and pressing the "Edit" button. This allows you to change all aspects of the job.

## Test a job

You can test both new and existing jobs using the Test buttons on the right hand side of the page. These buttons execute the job on Backand's servers, displaying the request sent and the response received. In addition to simple verification, testing can be used to verify that advanced parameters function properly, and that the job responds as expected.

## Run background job using REST API
Background Jobs are useful for long running tasks such as integrating with external sites (where the response time could be slow), or sending out batched push notifications. If you commonly encounter timeout errors while running on-demand actions, then you should consider using a Background Job.

All messages and errors are logged into the console. These messages are available in the dashboard under Log --> Console.

You can use this REST API endpoint to call a job by its ID:

 ```
 https://api.backand.com/1/jobs/run/{id}
 ```

You can also use this JavaScript code to call a job from an action:

 ```javascript
    var response = $http({
        method: "GET",
        url: CONSTS.apiUrl + "/1/jobs/run/" + cronId
    });
 ```

## Test background job
In order to test the job and get a response immediately (either valid results or an error message), use the following REST endpoint. 

*Note: (this call runs synchronously, so it may take a long time to finish)*

```
https://api.backand.com/1/jobs/run/{id}/test
```
