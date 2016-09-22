Crons are schedule jobs allows you to schedule a call to an action, query or external URL. By settings the schedule
time and frequency the action will be called by Backand's servers at the background (same for query and external URL). The external URL allows you to use Backand to call any other service on the internet you may need to use with your app.

## Create a cron

You can add a new cron by clicking on "+ New Cron" in the menu bar.
To add a new job schedule, give it a name to be displayed in the cron menu, description, start time or frequency and type (Action / Query / External URL).

### Advanced Options
You can use additional parameters in the job by using query string or headers information. You can also send long
text by using POST method.

For example, if you need to use the same action in two jobs, you can send a "type"
parameter in order to distinguish which job was running. In the 'Query String' field you would have 'parameters={type:1}' for the first Job and 'parameters={type:2}' for the second job calling the same action.

## Update an existing cron

To update an existing cron, select the cron from the navigation menu and press the "Edit" button.

## Test a cron

You can test both new and existing cron using the Test on the right hand side of the page. This executes the job on
Backand's servers returning the request and response of the call. The test is recommended to use in order to verify that all the advanced parameters are sent correctly and the response is as expected.
