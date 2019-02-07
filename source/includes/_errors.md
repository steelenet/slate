# Errors

The Amelia API uses the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request is invalid.
401 | Unauthorized -- Bad or expired token.
403 | Forbidden -- Disallowed endpoint for your user.
404 | Not Found -- Endpoint could not be found.
405 | Method Not Allowed -- Disallowed endpoint.
418 | I'm a teapot.
500 | Internal Server Error -- We had a problem with our server. Try again later.
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
