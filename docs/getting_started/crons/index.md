Crons are scheduled jobs, allowing you  to run an action, query, or external URL request on a schedule that you define. Simply set the schedule's time and frequency, and the desired task will be executed automatically by Backand's servers. Through this mechanism, you can run periodic queries for things like reports, recurring actions to control you application's behavior, or you can regularly post to external services that support your application's behavior.

## Create a cron

To create a new cron job, click on "+ New Cron" in the "Crons" section of the navigation bar. Simply add a name, a description, a start time or frequency, and set the type of job (Action, Query, External URL) to execute.

#### Advanced Options

In the advanced options, you can configure the job to operate via either GET request, or via a POST. The GET request allows you to configure the query string used in the request, as well as modify the headers sent - allowing you to send additional parameters to the job when it executes. For POST requests, you get both the headers and the query string to modify, but you are also given the Request Data field which you can use to send additional parameter formats, like long text.

This can be very useful in distinguishing between two jobs that run the same action. For each job, you can send a "type" parameter to indicate the specific job being run. You can accomplish this by setting the "Query String" field to the appropriate value: 'parameters={type:1}' for job 1, and 'parameters={type:2}' for job 2.

## Update an existing cron

You can update existing cron jobs by selecting the cron job from the navigation menu and pressing the "Edit" button. This gives you the capability to change all aspects of the cron job.

## Test a cron

You can test both new and existing cron using the Test buttons on the right hand side of the page. These buttons execute the job on Backand's servers, displaying the request sent and the response received. In addition to simple verification, Testing should be used to verify that advanced parameters function properly, and that the job responds as expected.
