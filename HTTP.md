# HTTP Status Codes
## 1xx: Loading
### 100 Continue
So far, everything is OK; client should continue with the request or ignore the request if already finished. For the server to check the request headers, the client must send "Expect: 100-continue" as a header in the initial request and receive "100 Continue" in response prior to sending the respective body. 
Compatible with Firefox: Yes
Compatible with Chromium: Yes
### 101 Switching Protocols
The server must switch to another protocol. The protocol is to be specified in the "Upgrade" request header from the client. The server is to include the "Upgrade" response header for indication of the new protocol.
### 102 Processing
**Deprecated Feature**: This response code is no longer recommended. While some browsers may continue supporting this code, the relevant web standards may no longer support it. If not, this code may be in the process of being dropped or only remain for the sole purpose of compatibility. Avoid using this code, and update any existing code if and when possible. This feature may cease to function at any time, if not yet.  
_Historically_, this status code was intended to indicate that a full request had been received and that the server must work on the specifics. This code was only designed for requests that required a significant amount of time to verify that the request was not dead yet, if ever.
### 103 Early Hints
Hints should be given about the sites and resources expected by a server toward the final response. The browser may begin to "preconnect" to sites or begin "preloading" resources, even before full preparation by the server and the final response. This response is primarily intended for use with the "Link" header, indicating the resources to be loaded. There may also be a "Content-Security-Policy" header enforced while the early hint is being processed. Servers may send multiple 103 responses, such as following (a) redirect(s). Browsers should only process the first 103 response, which must be discarded if the result is a cross-origin redirect. Therefore, the resources from the early hint are in theory pre-pended to the "Document" head element, followed by the resources loaded for the final response.
## 2xx: Success
### 200 OK
The request has been successful. By default, this response code is cacheable. Also, the true definition of this response code varies by the method being utilized:
#### GET
The resource has successfully been fetched and is transmitted in the message body.
#### HEAD
The representation headers are included in the response; message body not included.
#### POST
The resource describinng the successful result of the action is transmitted in the message body.
#### TRACE
The message body contains the request message that was received by the server.
#### PUT or DELETE
Although 200 OK is generally intended for successful results, the same is <u>not</u> typical with "PUT" or "DELETE" methods. Instead, the successful result is typically either a <u>201</u> (Created) or a <u>204</u> (No Content) response code, depending on the context. The <u>201</u> is typically reserved for first-time resource uploads.
### 201 Created
The request has succeeded, resulting to the creation of a new resource. The new resource, which could simply refer to a description and a link to the new resource, is effectively created before the response is sent back; the newly created items should be returned in the body of the message, located either within the Uniform Resource Locator (URL) of the request or at the URL in the value of the "Location" header.
***
The common use case for a **201** status code is as the result of a "POST" request specifically.
### 202 Accepted
The request has been accepted for processing, but the processing has not yet been completed. Processing may not have started from the protocol yet. The request may or may not be acted upon later, as processing may not allow the code later. 202 is a non-committal status code, so there is no way for HTTP to later send an asynchronous response indicating the outcome of request processing. This is intended for cases in which another process or server handles the request or for batch processing purposes.
### 203 Non-Authoritative Information
The request was successful, but the enclosed payload has been modified by a transforming proxy from the 200 response from the origin server. This response code is relatively similar to the 214 "Transformation Applied" value from the "Warning" header code, which also contains the additional advantage of being applicable to responses with any status cude, not just a 200 (OK) code.
### 204 No Content
The request has succeeded; however, the client does not need to navigate away from the current page. This response code is cacheable by default (with an "ETag" header included in such responses).
***
This may be used in cases such as "save and continue editing" functionality on various wiki sites. In these cases, a "PUT" request should be used to save the page, and the "204" response would be sent indicating that the editor is not to be replaced by any other page.
### 205 Reset Content
The server tells the client to reset the document view, such as to clear the content of a form, to reset a canvas state, or to refresh the user interface (UI).
### 206 Partial Content
This status code indicates that the request has succeeded and the body contains the ranges of data that have been requested as described in the "Range" header of the request. If there is only one range, the "Content-Type" of the whole response is set to the type of the document, with a "Content-Range" provided. If several ranges are sent back, the "Content-Type" is set to "multipart/byteranges" and each fragment covers one range, with "Content-Range" and "Content-Type" each describing the ranges.
### 207 Multi-Status
**Note:** The ability to return a _collection_ of resources is part of the WebDAV protocol and extension to HTTP. Therefore, it may be received by web applications when accessing a WebDAV server. Browsers accessing web pages will <u>never</u> encounter this status code.
***
This response code indicates that a mixture of responses is possible.
### 208 Already Reported
The ability to _bind_ a resource to several paths is an extension to the WebDAV protocol. Therefore, it may be received by web applications accessing a WebDAV server. Browsers accessing web pages will never encounter this status code. This code is used in a "207 Multi-Status" response to save space and avoid conflicts. If the same resource is requested several times, only the first is to be reported with "200 OK" while all other bindings are to report "208" to ensure a lack of conflicts. The response will ultimately remain shorter when only the first response is a "200" code and the remainder are "208" codes.
### 226 IM Used
**Note:** Browsers do not support _delta encoding_ with HTTP. This status code is returned by custom servers when used by specific clients.
***
In the context of delta encodings, the "226 IM Used" status code is an indicator by the server for returning a _delta_ to the "GET" request received.
#### Delta Encodings
With delta encoding, a server responds to "GET" requests with differences known as deltas relative to a given base document as opposed to the current document. The "A-IM" HTTP header is utilized by the client to indicate the differencing algorithm to use and the "If-None-Match" header to hint the server about the last version received. The server should generate a delta to be sent back in an HTTP response with the "226" status code with the "IM" and "Delta-Base" headers wth the name of the algorithm used and the "ETag" matching the base document respectively.
## 3xx: Redirects
### 300 Multiple Choices
The request has more than one possible response. The user-agent or the user should choose one of them. As there is no standardized way of choosing one of the responses, this response code is very rarely used. If the server has a preferred choice, it should generate a "Location" header.
### 301 Moved Permanently
The requested resource has been definitively moved to the URL given by the "Location" headers. A browser redirects to the new URL and search engines update their links to the resource.
***
**Note:** Although the specification requires the method and the body to remain unchanged during the redirection, not all user-agents obey this requirement. The 301 code is most appropriate only as a response for "GET" or "HEAD" methods with the "308 Permanent Redirect" more appropriate for "POST" methods instead, as the method change is explicitly prohibited with this status.
### 302 Found
The requested resource has been _temporarily_ moved to the URL given by the "Location" header. A browser redirects to this page; however, search engines do not update links to the resource. Even if the specification requires the method and the body not to be altered when the redirection is successfully performed, not all user-agents conform as with the 301 code. Therefore, the 302 code is most appropriate only for "GET" or "HEAD" methods with the "307 Temporary Redirect" for "POST" methods and otherwise as the method change is explicitly prohibited in those cases.
***
If one wants the method to change to "GET" instead, they should use "303 See Other" instead. This is useful for giving responses to a "PUT" method that is not the uploaded resource but a confirmation message such as "you successfully uploaded ...".
### 303 See Other
The redirects do not link to the requested resource directly but to another page, such as a confirmation page, a real-world object representation, or an upload-progress page. This method is often sent back as the result of "PUT" or "POST" methods. However, the method to display this redirected page is always the "GET" method.
### 304 Not Modified
**Note:** Many network panels from developer tools of browsers create extraneous requests that result in 304 responses, so that local cache access is visible to developers.
***
There is no need to re-transmit the requested resources. This response code is sent when the request is a conditional "GET" or 'HEAD" request with an "If-None-Match" or an "If-Modified-Since" header and the condition evaluates to false. The resulting redirection is implicitly related to a cached resource that would have resulted in a "200 OK" response if the condition had evaluated to true instead. Therefore, while "true" results in a "200" code, "false" results in the "304" code instead.
***
The response must **not** contain a body and must include the headers that would have been sent in an equivalent "200 OK" response: `Cache-Control`, `Content`, `Date`, `ETag`, `Expires`, and `Vary`.
### 307 Temporary Redirect
The resource requested has been temporarily moved to the URL given by the "Location" headers. The method and the body of the original request are reused for performing the redirected request. In the cases where the desired method is to be changed to the "GET" method, use "303 See Other" instead. This is useful when one desires to give an answer to a "PUT" method that is not the uploaded resources but a confirmation message such as "You successfully uploaded ...".
***
The only difference between "307" and "302" is that the former guarantees that the method and the body will not be changed when the redirected request is made. With the latter, some old clients were incorrectly changing the method to "GET" instead. Therefore, the resulting behavior of "302" with methods other than "GET" is often unpredictable on the Web, while the "307" behavior is predictable. For "GET" requests, the behavior of either should be identical.
### 308 Permanent Redirect
The resource has been definitively moved to the URL given by the "Location" headers. A browser will redirect to this page and search engines will update their links to the resource.
***
As with the differences between "307" and "302" response codes, the behavior of "308" is distinguished from the "301" behavior by not altering the request method and the body while the latter may sometimes be incorrectly changed to a "GET" method specifically.
## 4xx: Client Errors
### 400 Bad Request
The server cannot or will not process the request due to something that is perceived to be a client error (for example, malformed request syntax, invalid request message framing, or deceptive request routing).
***
<span style="color:orange;">Warning:</span> The client should not repeat this type of request without modification.
### 401 Unauthorized
The client request has not been completed because valid authentication credentials for the resource requested are lacking. This status code is to be sent with an HTTP "WWW-Authenticate" response header containing information on how the client can request for the resource again after prompting the user for authentication credentials. This status code is often similar to the "403 Forbidden" status code, except that in situations resulting in this status code, user authentication can allow access to the requested resource.
### 402 Payment Required
**Experimental: This is an <u>experimental technology</u> (Mozilla).**  
This is a non-standard response code that is specifically reserved for future use with digital cash. This status code was created to enable digital cash or (micro-)payment systems and should indicate that the content requested is not available until a payment is made by the client. In some cases, this status code is intended to indicate that the request requires a client payment to be processed; however, no standard use convention exists and the contexts may vary by specific entities.
### 403 Forbidden
The server understands the request, but refuses to authorize it. Re-authenticating with this status code will make no difference; otherwise this response code is comparable to the "401 Unauthorized" code. The access is simply tied to the application logic, such as insufficient rights to a resource.
### 404 Not Found
The server cannot find the requested resource. Links that lead to this type of page are often referred to as "broken" or "dead" links and can be subject to "link rot." If a resource is permanently removed, the HTTP specification recommends the "410 Gone" status code instead of "404 Not Found". The "404" code simply indicates that the resource is missing, regardless of whether the absence is temporary or permanent.
### 405 Method Not Allowed
The server knows the request method, but the target resource does not support the method. The server **must** generate an "Allow" header field in a "405" status code response. The field must contain a list of methods that the target resource currently supports.
### 406 Not Acceptable
The server cannot produce a response matching the list of acceptable values as defines in the request's proactive "content negotiation" headers, and the server is unwilling to supply a default representation. Proactive headers include `Accept`, `Accept-Encoding`, and `Accept-Language`. In practice, this error is very rarely used. Servers generally ignore the relevant header and serve an actual page to the user. Even if the user would not be completely happy, the specification assumes that a "partial" page would be preferred to a simple "406" page. If a server returns such an error status, the body of the message should contain the list of available representations of resources, allowing for the user to choose among them.
### 407 Proxy Authentication Required
The request has not been applied because it lacks valid authentication credentials for a "proxy server" between the browser and the server that can access the requested resource. This status is to be sent with a "Proxy-Authenticate" header containing information on how to authorize correctly.
### 408 Request Timeout
The server would like to shut down this unused connection. This code is sent on an idle connection by some servers, _even without any previous request by the client_. A server should sent the "close" "Connection" header field in the response, since "408" implies that the server has decided to close the connection entirely rather than continuing the waiting process. This response is used much more since some popular browsers tend to prefer HTTP pre-connection mechanisms to speed up surfing. Some servers, however, merely shut the connection down without sending such a message at all.
### 409 Conflict
A request conflict exists with the current state of the target resource. Conflicts are most likely to occur in response to a "PUT" request, such as when uploading a file older than the existing file on the server resulting in a version control conflict or similar results.
### 410 Gone
Access to the target resource is no longer available at the origin server and this condition is likely to be permanent. If one does not know whether the condition is permanent or temporary, a "404" status code should be used instead. "410" responses are generally cacheable by default.
### 411 Length Required
The server refuses to accept the request without a defined "Content-Length" header. By specification, when data is sent as a series of chunks, the "Content-Length" header is omitted and at the beginning of each chunk one needs to add the current chunk length in a hexadecimal format. See "Transfer-Encoding" for more details.
### 412 Precondition Failed
Access to the target resource has been denied. This occurs with conditional requests and methods other than "GET" or "HEAD" when the "If-Unmodified-Since" or "If-None-Match" conditions have not been fulfilled. In these cases, requests cannot be made and this error response is sent back, particularly as the result of an upload or a modification of a resource.
### 413 Content Too Large
The request entity is larger than limits defined by a server; such a server might close the connection or return a "Retry-After" header field. Prior to RFC 9110 the response phrase for this status was "413 Payload Too Large" and remains a name that is widely used.
### 414 URI Too Long
The Uniform Resource Indicator requested by the client is longer than the server is willing to interpret. Conditions in which this case may occur are generally rarer types of conditions, such as when a client has improperly converted a "POST" request to a "GET" request with long query information, when a client has descended into a loop of redirection such as a redirected Uniform Resource Indicator prefix pointing to a suffix of itself, or when the server is under attack by a certain client attempting to exploit potential security holes.
### 415 Unsupported Media Type
The server refuses to accept the request; the payload format is unsupported. The format problem might be the result of the request's indicated "Content-Type" or "Content-Encoding" headers or as a result of a direct inspection of the data.
### 416 Range Not Satisfiable
The server cannot serve the requested ranges. The most likely reason is that the document does not contain such ranges, or that the "Range" header value, though syntactically correct, does not make sense. The "416" response message contains a "Content-Range" indicating an unsatisfied range ('*') followed by a '/' and the current length of the resource. When faced with this error, browsers typically either abort the operation (such as in the case that a download is non-resumable) or ask for the entirety of the document again.
***
One example where a "416" error may occur is with the syntax "Content-Range: bytes */12777".
### 417 Expectation Failed
The expectation from the "Expect" header of the request could not be met successfully.
### 418 "I'm a teapot"
The server refuses to "brew coffee" because it is permanently a "teapot." This error is a reference to "Hyper Text Coffee Pot Control Protocol" from April Fools' jokes in 1998 and 2014, and some websites use this response for requests that they do not wish to handle, including but not limited to automated queries.
### 421 Misdirected Request
The request was directed to a server that is unable to produce a response. This might be possible if a connection is reused or if an alternative service is selected.
### 422 Unprocessable Content
The server understands the content type of the request entity, and the syntax of the request entity is correct. However, it was unable to process the contained instructions. The client should not repeat this request without modification to the request.
### 423 Locked
The ability to _lock_ a resource is specific to some WebDAV servers. Browsers accessing these web pages will never encounter "423 Locked" as a response code; in the erroneous cases where this occurs the browsers will handle this as a generic "400" status code instead.
### 424 Failed Dependency
The method could not be performed on the resource because the requested action depended on another action, which failed.
### 425 Too Early
The server is unwilling to risk processing a request that might be replayed, creating the potential for a replay attack.
### 426 Upgrade Required
The server refuses to perform the request using the current protocol but might be willing to do so after the _client_ upgrades to a different protocol. The server sends an "Upgrade" header with this response to indicate the required protocol(s).
### 428 Precondition Required
The server requires the request to be _conditional_. Typically, a required precondition header, such as "If-Match" or similar headers, is missing. When a precondition header is not matching the server-side state, the response should be "412 Precondition Failed" instead.
### 429 Too Many Requests
The user has sent too many requests in a given amount of time ("rate limiting").
### 431 Request Header Fields Too Large
The server refuses to process the request because the request's HTTP headers are too long. The request _may_ be re-submitted after reducing the size of the request headers.
### 451 Unavailable For Legal Reasons
The user requested a resource not available due to legal reasons, such as a web page for which a legal action has been issued.
## 5xx: Server Errors
### 500 Internal Server Error
The server encountered an unexpected condition that prevented it from fulfilling the request. This response is a generic "catch-all" response that generally indicates that the server is unable to find a more appropriate "5xx" error code to response. Sometimes, server administrators log error responses such as the "500" status code with more details about the specific request to prevent the same error from occurring in the future.
### 501 Not Implemented
The server does not support the functionality required to fulfill the request. This status can also send a "Retry-After" header to tell the requester when to check back to verify functionality later. "501" is appropriate when the server does not recognize the request method and is incapable of supporting it for any resource. The only methods required for support by the servers are "GET" and "HEAD" methods, so "501" must not be the response code for such methods. If the server _does_ recognize the method, however, the appropriate response is "405 Method Not Allowed" for methods that have been intentionally unsupported.
***
"501" errors require fixes by the respective web servers that ones are attempting to access. Such errors are also cacheable by default (unless the caching headers instruct otherwise, that is).
### 502 Bad Gateway
The server, while acting as a gateway or proxy, received an invalid response from the upstream server. A "502" error generally requires a fix by the web server or the proxies to gain access through.
### 503 Service Unavailable
The server is not ready to handle the request. Common causes include servers that shut down for maintenance or that are overloaded. Temporary conditions are the appropriate use case of this response, and the "Retry-After" HTTP header should contain the estimated time for the recovery of the service. Together with this response, a user-friendly page for explaining the problem should also be sent. Caching-related headers that are sent along with this response should be taken care of, as a "503" status is often a temporary condition and responses usually should not be cached.
### 504 Gateway Timeout
The server, while acting as a gateway or proxy, did not receive a response in time from the upstream server necessary to complete the request. In general, the "5xx" series of errors cannot be fixed by end users but requires fixes by the web server or associated proxies that one is attempting to gain access through.
### 505 HTTP Version Not Supported
The HTTP version used in the request is not supported by the server.
### 506 Variant Also Negotiates
This status code may be given in the context of Transparent Content Negotiation, as defined in RFC 2295. This protocol enables a client to retrieve the best variant of a given resource, where the server supports multiple variants. This status code is intended to indicate an internal server configuration error in which the chosen variant is itself configured to engage in content negotiation and is thus not a proper negotiation endpoint.
### 507 Insufficient Storage
This status code may be given in the context of the Web Distributed Authoring and Versioning, or WebDAV, protocol as described in RFC 4918. This code indicates that a method could not be performed because the server is unable to store the representation necessary to successfully complete the request.
### 508 Loop Detected
This status code may be given in the context of the Web Distributed Authoring and Versioning, or WebDAV, protocol, indicating that the server terminated an operation because of the overarching result of an infinite loop that has been detected. In these cases the server is attempting to process a request with "Depth: Infinity" indicating that the entire operation failed.
### 510 Not Extended
This starus code may be given in the context of the HTTP Extension Framework, as defined in RFC 2774. In that specification a client may send a request containing an extension declaration, describing the extension to be utilized. If the server receives such a request, but any described extensions are not supported for the same, then the server should respond with the "510" status code.
### 511 Network Authentication Required
The client needs to authenticate to gain network access. This status is not generated by origin servers but by intercepting proxies that control access to the network. Network operators sometimes require some authentication, acceptancde of terms, or other user interaction before access can be granted, such as at airports and other public conveniences. They often identify clients who have not done so through Media Access Control (MAC) addresses.
