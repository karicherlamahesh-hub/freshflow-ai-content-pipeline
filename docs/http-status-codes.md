# HTTP status codes

A status code is a 3-digit number that a server sends back after every API request. It tells you if the request worked.

## 200 OK
Your request worked, and the server sent back what you asked for.

**What to do:** nothing to fix. Read the data in the response.

## 201 Created
You asked the system to create something, and the system successfully created it.

**What to do:** nothing to fix. Save the new item's ID from the response.

## 400 Bad Request
You asked the server to do something, but your request contains incorrect or invalid information.

**What to do:** check your body for a missing field or bad JSON, fix it, and send again.

## 401 Unauthorized
You are trying to access something, but you haven't proved your identity to the server.

**What to do:** check your API key or login details, and send again.

## 404 Not Found
You asked the server to show you something, but the server couldn't find it.

**What to do:** check the URL and the ID for spelling mistakes.

## 500 Server Error
Your request reached the server, but the server encountered a problem and couldn't complete your request.

**What to do:** this is not your mistake. Wait a little and try again. If it keeps happening, tell the server's owner.
