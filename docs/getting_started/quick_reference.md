Here are a few basic URL guidelines for quickly accessing your application's data:

## Base URL

The base URL for your application will be `https://api.backand.com:8078/1` - all of your access will be driven from this URL

## Request Format

All requests must be made using JSON request bodies. Make sure to set the `Content-Type` header to `application/json`. Additionally, all requests must include an authentication token in the request headers. This process is deatiled in our [documentation](http://docs.backand.com/en/latest/security/index.html).

## Response Format

All responses are returned as JSON objects. The status code of the response indicates the success (2xx) or failure (4xx) of the request.

## Security & Authentication
| URL | HTTP Verb | Functionality |
| ----- | ----------- | --------------- |
| /token | POST | Obtains a 24-hour acccess token. A Backand username and password must be provided, along with the application name |
| /user/signup | POST | Registers a new user with the application. Must use a SignUpToken, which is configured for the application |


## Manipulating objects
| URL | HTTP Verb | Functionality |
| ----- | ----------- | --------------- |
| /objects/{object name} | GET | Gets a list of all objects of type {object name} |
| /objects/{object name} | POST | Creates an object of type {object name} |
| /objects/{object name}/{id} | GET | Gets an object of type {object name} with ID {id} |
| /objects/{object name}/{id} | PUT | Updates an object of type {object name} with ID {id} |
| /objects/{object name}/{id} | DELETE | Deletes an object of type {object name} with ID {id} |

## Custom Object Actions
| URL | HTTP Verb | Functionality |
| ----- | ----------- | --------------- |
| /objects/action/{object name}/{id}?actionName={action name} | GET | Executes custom action {action name} on object {object name} with an id of {id} |

## Queries
| URL | HTTP Verb | Functionality |
| ----- | ----------- | --------------- |
| /query/data/{query name} | GET | Executes custom query {query name} |

