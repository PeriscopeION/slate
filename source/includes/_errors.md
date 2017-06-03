# Errors

The ION API returns the following error codes:


Error Code | Meaning
---------- | -------
400 | Bad Request -- Your request sucks
401 | Unauthorized -- Your API key is wrong
403 | Forbidden -- Access to the resource requested is not allowed 
404 | Not Found -- The specified resource could not be found
405 | Method Not Allowed -- You tried to access a resource with an invalid method
406 | Not Acceptable -- You requested a format that isn't json
410 | Gone -- The resource requested has been removed from our servers
429 | Too Many Requests -- Slow down!
500 | Internal Server Error -- It broke. Try again later. Please submit your request to us so we can debug!
503 | Service Unavailable -- We're temporarily offline for maintenance. Please try again later.
