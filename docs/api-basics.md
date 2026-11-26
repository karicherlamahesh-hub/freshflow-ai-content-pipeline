# API basics

A short guide to how APIs work, using the calls from the FreshFlow project.

## What is an API
API is a messenger that allows two different software applications to communicate and exchange information with each other.

For example, when you order food in a delivery app, the app sends your order to the restaurant's system through an API.

## Parts of a request
When an application communicates with another application using an API, it sends a request.

- **Method:** what you want to do (GET, POST, PUT or DELETE).
- **URL:** the address where you send the request.
- **Headers:** extra information, such as the data type or an API key.
- **Body:** the data you send, used with POST and PUT.

## Parts of a response
When an application sends a request to a server through an API, the server processes that request and sends back a response.

- **Status code:** a number that tells you if it worked, such as 200, 201 or 404.
- **Body:** the data that comes back, usually in JSON.

## JSON in one minute
JSON is a simple format used to store and exchange data between different applications.

It has five types: string (text), number, boolean (true or false), object (key and value pairs in curly brackets) and array (a list in square brackets).

```json
{
  "title": "FreshFlow stock alert",
  "userId": 1,
  "sent": true,
  "tags": ["stock", "alert"]
}
```

## The 5 calls in the Postman collection

| # | Call | Method | What it shows |
|---|---|---|---|
| 1 | Get all posts | GET | A list of items (200 OK) |
| 2 | Get one post | GET | One item by its ID (200 OK) |
| 3 | Create a post | POST | Sending JSON, getting 201 Created |
| 4 | Delete a post | DELETE | Removing an item |
| 5 | FreshFlow - Create content | POST | Starting the FreshFlow n8n workflow through a webhook |

## Try the collection yourself

1. Download `postman/freshflow-api-basics.postman_collection.json` from this repository.
2. In Postman, click **Import** and choose the file.
3. Calls 1 to 4 work straight away, because they use a free test API.
4. For call 5, create an environment with a variable named `freshflow_webhook_url` and paste your own n8n webhook URL. The URL in the file is only a placeholder.
