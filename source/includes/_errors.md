# Errors

The Holmes API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- No API key provided.
403 | Forbidden -- Your API key is invalid.
404 | Not Found -- The specified resource could not be found.
405 | Method Not Allowed -- You tried to access an endpoint with an invalid method.
500 | Internal Server Error -- We had a problem with our server. Try again later.
502 | Bad Gateway -- Our backend services are unavailable. Please try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
504 | Gateway Timeout -- Our backend services are taking too long to respond. Please try again later.
