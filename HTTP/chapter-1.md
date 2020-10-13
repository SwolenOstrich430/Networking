## Overview of HTTP

### Media Types
#### MIME (new)
HTTP tags each object being trransported through the Web with a format called a MIME type 
    MIME: Multipurpose Internet Mail Extensions 
        originally designed to solve problems ecountered in moving messages between electronic mail systems 
        works so well for email that HTTP adopted it to desribe and label its own multimedia content 
    
    servers attach a MIME type to all HTTP object data 
    when a browser gets an object back fromn a server
        looks at the MIME type in order to understand how to process that object 
    
    MIME type 
        usually a text label represented as primary object type and specific subtype, separated by a slash
            e.g. text/html, text/plain (ASCII), image/jpeg, image/gif, video/quicktime, applicaiton/vnd.ms-powewrpoint 
        hundreds exist (many experimental or limited-use)

#### URIs (review)
every resource on a server has a name so that clients can point to which resource they need 
this name is referred to as a Uniform Resource Identifier (URI)
    similar to a postal address on the internet 
    e.g. www.google.com/super-cool-html-page.html (same thing as any other file system)

#### URLs (review)
Uniform Resource Locator (URL) most common form of resource identifier 
    describe the specfic location of a resource on a particular server 
    tells you exactly how to fetch a resource from a precise, fixed location 
    URLs typically follow a standard format of 3 parts: 
        1. scheme 
            descrbes the protocol used to access the resource 
            usually HTTP/HTTPS
        2. server internet address (www.google.com)
        3. the rest names a resource on the server (e.g. /super-awesome-page.html)

#### URNs (new)
the second flavor of URIs = Uniform Resource Name 
unique name for a specific piece of content, independent of where the resource currently lives 
allow resource to move from place to place within a server 
allow resources to be accessed by multiple network access protocols without changing a name 

e.g. urn:ietf:rfc:2141

URNs apparently still experimental (look at age of book and compare with other resources)
    need a supporting infrastrucutre to resolve resource locations 
        has slowed adoption 
        but do hold some promise4 for the future 


#### Transactions (only writing down what's new)
HEAD method: send just the HTTP headers from the response for the named resource 

#### Messages (review)
made up of 3 main parts 
    1. Start line: indicate what to do for a request/what happened for a response (text)
    2. Headers: metadat about the request (key:value) (text)
    3. Body: data to or from server (can be bianry)

#### TCP (new)
HTTP is an application layer protocol 
    doesn't worry about nitty-gritty details of network communication 
    leaves the details of networking to TCP/IP

TCP/IP provides: 
    error-free data transportation 
    in-order delivery (data arrives in order it was sent)
    unsegmented data stream (can send out data in any size at any time)

internet itself is based off of TCP/IP
    abstracts away individual network and hardware specs 
    lets computers and networks of any type talk to one another reliably 

HTTP network protocol stack 
HTTP application
TCP transport 
IP network  
network-specific link interface data link
phyiscal network hardware physical layer 

#### Connections, IP Addresses, and Port Numbers (semi-review)
before a messages can be sent to a server, the client needs to establish a TCP/IP connection b/w the client and the server using IP (internet protocol) addresses and port numbers 
    in TCP, you need:
        the IP adress of the server 
        the TCP port number associated with the software running on the server 

how do you get that....?
    URL provides that information 
        e.g. http://207.000.83.29:80/index.html (machine's IP address with the port number)
             http://www.netscape.com:80/index.html 
                (hostname/textual domain name...alias for IP address, can be converted to IP address through Domain Name Service (DNS)) 
             http://www.netscape.com/idnex.html 
    
    Full process 
        1. browser extract's the server's hostname from the URL
        2. browser converts the server's hostname into the server's IP address 
        3. browser extracts the port number (if any) from the URL 
        4. browser establishes a TCP connection with the server 
        5. browser sends an HTTP request message to the server 
        6. server sends an HTTP response back to the browser 
        7. connection is closed, and the browsers handles the response 

#### Protocol Versions (new)
are several version of HTTP inuse today 
    HTTP/0.9: 1991 prototype version of HTTP 
        contains many design flaws and should only be used to interact with legacy clients 
        only supports the GET method 
        does not support MIME types, headers, or version numbers 
        only designed to fetch simple HTML objects, was quickly replaced with HTTP/1.0
    HTTP/1.0: first widely deployed version of HTTP 
        added version numbers, headers, additional methods, and mutlimedia object handling 
        made it practical to support grpahically appealing web pages + interactive forms 
        spec was never well specified however b/c of rapid adoption/changes needed at time 
    HTTP/1.0+
        added long-lasting, "keep alive" connections, virtual hosting support, proxy connection support
        was an informal, extended version of HTTP/1.0
    HTTP/1.1 current version of HTTP
        focused on correcting arhictectural flaws in design
        specified semantics, introduced performance optimization, and removed mis-features 
    HTTP-NG (a.k.a. HTTP/2.0)
        prototype proposal for a successor to HTTP/1.1
        focuses on performance optimization + powerful framework for remote execution of server logic 
        (definitely old because it says this isn't a thing yet)

#### Components of the Web (mostly new)
Proxies/Proxy Servers
    HTTP intermediaries that sit between clients and servers 
        i.e. access a server on the client's behalf 
    often used for security 
    can also filter requests and responses 

Caches (web cache/cache proxy)
    HTTP proxy server that keeps copies of popular documents that pass through the proxy
    next client requesting the same document can be served from the cache's copy 

Gateway     
    servers that act as intermediaries for other servers
    often used to convert HTTP traffic to other protocols 
    always recieves requests like it's the origin server 
        client most likely won't be aware it's communicating with a gateway
        e.g. HTTP/FTP gateway

Tunnels 
    HTTP applications that blindly relay raw data between two connections 
    often used to transport non-HTTP data over one or more HTTP connections 
        without looking at the data 
    popular use: 
        carry encrypted SSL traffic through an HTTP connection 

Agents (User Agents)
    clietn programs that make HTTP requests on the user's behalf 
    any app that issues web requests is an agent 
        e.g. a browser would be an agent, crawlers as well (this book calls them spiders, confirmed old)


