## HTTP Messages

### Flow of Messages 
Messages: 
    blocks of data sent between HTTP applications 
    begin with: 
        metadata describing message content and meaning 
    followed by: 
        optional data 
    flow between clients, servers, and proxies 
    main terms describing message direction: 
        inbound 
        outbound 
        upstream
        downstream

#### Direction
inbound and outbound describe transactional direction 
    messages travel inbound to the origin server 
    and travel outbound back to the user agent

Messages flow downstream, regardless of whether they are requests or responses 
    the sender of the message is upstream to the reciever 
        i.e. request flows downstream to server and is upstream from client 
        and response flows downstream to client from server 

### Parts of a Message 
start line and headers are ASCII text, broken up by lines 
    terminated with two-character end of line sequence 
        carriage return (ASCII 13, \r) and line-feed character (ASCII 10, \n)
            written CRLF

body is optional but can be binary or text 

request syntax: 
<method> <request-URL> <version> (HTTP/major.minor)
<headers>

<entity-body>

resposne syntax: 
<version> <status> <reason-phrase>(human readable version of status code)
<headers>

<entity-body>


#### Status code classes 

100-199: informational 
200-299: successful
300-399: redirection
400-499: client error
500-599: server error


#### Header classifications 
General headers: appear in both request and response messages 
Request headers: provide more information about the request 
Response headers: same but responses 
Entity headers: describe body size and content, or the resource itself 
Extension headers: new header that are not defined in the spec 


#### Early limitations 
HTTP/0.9 messages only consisted of requests and responses 
    the request only contained the method and request URL 
    the response only contained the entity 

### HTTP Methods 
not all methods implemented by every server 
    to be compliant with HTTP/1.1, a server only needs to implement GET and HEAD methods 
        HEAD: 
            behaves exactly like GET, but the server only returns that headers in the response (no body)
            with head, you can: 
                find out about a resource without getting it
                see if an object exists (by looking at the response's status code)
                test if the resource has been modified by looking at the headers 

#### Safe Methods 
i.e. methods like GET and HEAD which (hopefully) guarantee that no action is taken on the server as a result of the method

#### PUT, POST, PATCH 
replace resource in its entirety, create new resource, modify a given resource 

#### TRACE 
allows clients to see how its request looks when it finally makes it to the server 
    initiates a loopback diagnostic at the destination server 
    i.e. server at end of request bounces back a TRACE response with the original request method it recieved in the response body
        good for seeing the effects of proxies and other applications on requests

#### OPTIONS
asks the server to tell you about its supported capabilites 
    good for determining how to access resources without having to directly request for them

#### Extension Methods 
HTTP was designed to be field-extensible 
    i.e. new features wouldn't cause old ones to fail 
    these methods allow devs to expand the HTTP/1.1 spec 

    common examples include: 
        LOCK: allows a user to "lock" a resource
            i.e. prevent others from editing something at the same time that you are
        MK-COL: allows a user to create a resource 
        COPY: facilitates copying a resource on the server
        MOVE: moves a resource on a server

    important to know that not all extension methods are defined in a formal spec 
        many are unlikely to be understood by most HTTP applications 
        possible that your HTTP applications could HTTP could encounter methods it doesn't understand 
            best to be tolerant of extension methods 
            proxies should relay messages unknown messages if they are capable
            and respond with 501 Not Implemented if they can't 
                "be conservative in what you send, be liberal in what you accept


### Status Codes 

#### 100-199 Informational Status Codes 
HTTP/1.1 inroduced informational status codes to the protocol 
    100: continue — indicates that an initial part of the request was recieved and the client should continue 
        used when a client wants to see if a server will/can accept it
        must send an "Expect" header, otherwise, the server might think that the client is sending the body
            clients should not wait indefinitely for a response from the server in this scenario, but rather set some 
            timeout 
        server that revieves this request should respond either with 100 Continue response or an error code 
            should never send a 100 Continue response to a client that does not send the 100-continue first 
            if the server recieves the subsequent request from the client that originated the 100-continue, it 
            doesn't need to respond to the original request 
        a proxy that recieves the 100 continue needs to: 
            know that the next reciever is HTTP/1.1 compliant 
                if it doesn't know that the recieve is HTTP/1.1 compliant, it should forward the request with the "Expect" header 
                if it knows that the server is not HTTP/1.1 compliant, it should respond with 417 Expectation Failed 
    101: Switching Protocols — indivates that the server is changing protocols specified by the client to one listed in the "Upgraded" header 

    THERE'S DEFINITELY MORE THAN THIS LOOK UP LATER (don't forget so this is just there when you look at it in like 3 months) 
#### 200-299 Success Status Codes 
    200: ok — request is good, body contains the requested resource 
    201: created — the entity body of the response shuld contain the URLs for referencing the resource with the Location header containing the most specific reference 
        server must have created the object prior to sending this status 
    202: accepted — request was accepted, but the server hasn't performed any action with it 
        no guarantess that the server will complete the request 
        just means the request initially looked valid 
        should send a description of the status of the request and an estimate for when it will be completed 
            or a pointer to where this information can be obtained 
    203: non-authoritativ-information: information contained in the headers came not from the origin server but from a copy of the resource 
        i.e. if something is held in cache and returned in response 
    204: no content — message contains headers and status line but no body
    205: reset content — clear any HTML form elements on the current page 
    206: partial content — come back to in chapter 15 

#### 300-399 Redirection Status Codes 
either tell clients to use alternate locations for the resource they're interested in or provide an alternate response instead of content 
    if a resource has moved, a redirection status code and a Location header can be sent to the client 
        allows browser to go to the new location without the user knowing 
can also allow for a client to check if its current copy of a resource matches the server's 
    i.e. it hasn't been modified 
    send with an If-Modified-Since header 
        gets the document only if it has been modified since a given date time 
            if the document hasn't been changed, server will respond with a 304 instead of the content 
good practice for responses to non-HEAD requests that include a redirection status code to: 
    inlcude a description and links to the redirected URL 

300: multiple choices — client has requested a URL with mutliple resources (i.e. english and french version of HTML)
    code is returned along with a set of options 
301: moved permanently — used when requested URL has been moved 
    response should contain new location in "Location" header 
302: moved permanently — used when URL has been moved, "Location" header should again be sent 
303: See other — tell client that the resource should be fetched with a different URL 
    again, new URL is in "Location" header
304: not modified — used to indicate that a resource has not changed within the time specified by client request 
305: use proxy — resource must be accessed using a proxy
306: unused
307: temporary redirect — same as 301 but "Location" header is not a permanent new location, old one can be used to access resource in the future 

#### 400-499 Client Error Codes 
400: Bad Request — client has malformed request 
401: unauthorized — returned with appropriate headers, telling the client to authenticate before it can gain access 
402: payment required — sometimes used to tell the user they need to make a payment, but usually not in use 
403: forbidden — request was refused by the server 
    can include more information in body, but is usually returned b/c it does not want to reveal more information
404: not found — resource doessn't exist 
    entity often included 
405: method not allowed — request is made with an unsupported method per the URL 
406: not acceptable — used when a client sets parameters around the resource they want and server can't fulfill that request 
407: proxy authentication required — like 401 but for proxy servers 
408: request timeout — request takes too long to complete 
409: conflict — causing an issue around a given resource, explanation should be provided in the response 
410: gone — similar to 404, but the server had the resource at one point 
    used often for website maintenance 
411: length required — used when server requires Content-Length header in request 
412: precondition failed — one of the client's conditions in the request fails 
413: request entity too large 
414: request entity URI too long 
415: unsupported media type 

#### 500-599 server error codes 
500: internal server error — server encounters an error that prevents it from handlign the request 
501: not implemented — client makes request the server does not have capability of fulfilling
502: bad gateway — when server acting as proxy or gateway gets a bad response from the next server in request-response chain
503: service unavailable — server cant fulfill request rn but can in the future 
504: gateway timeout
505: HTTP version unsupported 

### Headers 