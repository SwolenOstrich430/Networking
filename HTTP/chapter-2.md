## URLs and Resources 
URLs can direct you to resources available through protocols other than HTTP    
    e.g. they can point to any resource on the internet
        person's email account 
        files available through FTP
        movies hosted off of streaming video servers 

### Days Before URLs
had to have deep knowledge of multiple different protocols and go-betweens in order to access resources at all 
    no longer need a news reader to read internet news 
    or an FTP client to access files on FTP servers
    elecontronic mail programs to send revieve email messages 

### Schemes 
the scheme is the main identifier of how to access a given resource 
    tells the application interpreting the URL what protocol is needs to "speak" 
    must start with an alphanumeric and is separated from the URL by the first colon
    is case insensitive 

### Params 
    key-value pairs denoted by semi-colon and separated by path segments 
        not the same as query strings

### Fragments 
    # used to identify a specific part of the resource that the client is interested in 
        think going to a specific section in an HTML doc 
        doesn't mean that the server just sends that one part 
            jsut means the client focuses on that part and handles accordingly based on the resource 

## URL Shortcuts
    clients understand and use several URL shortcuts

### Relative URLs
    you already know what this is 
### Expandomatic URLs
    some browsers try to expand URLs automatically 
    comes in two flavors: 
        1. hostname expansion
            browser expands the hsotname you type in into the full hostname without your help
            e.g. if you type google and get www.google.com
                can cause issues for other HTTP applicaitons like proxies 
        2. History expansion 
            what it sounds like 

### Shady Characters 
    URLs were designed to be portable and general across the internet 
        meaning they'll be transmitted through various protocols 
            which might (will) have different methods for transmitting data 
            important that they be able to be transported through any internet protocol 
                i.e. not lose any information from initial request 
            means they're only allowed to contain a certain number of characters 
                creators also wanted URLs to be human-readable, so nonprinting characters aren't allowed either 
            URLS also need to be complete 
                people might want to have unsafe characters in binary data in URLs
                escape mechanism was added, allowing for unsafe characters to be encoded as safe characters 
    
#### URL Charset 
    % followed by hex digits (ASCII code of the character) for encoding problematic characters 

    Restrictions 
        %, /, ., .., #, ?, ;, :, $ \,, +, @, &, =, {}, |, \, ^, ~, [], `, <>, ", 0x00, 0x1F, 0X7F (nonprintable characters in ASCII), >0x7F (hex values don't fall within the 7-bit range of US-ASCII character set)

### Schemes 

http: http://<host>:<port>/<path>?<query>#<frag>
https: same as ^ just with "s", but uses SSL, which provides end-to-end encryption of HTTP connections 
mailto: refer to email addresses 
    behaves differently than other schemes 
        doesn't refer to objects directly 
        syntax: mailto:<RFC-822-addr-spec> (mailt0:me@gmail.com)
ftp: File Transfer Protocol 
    used to download and upload files on an FTP server and to obtain listings of the contents of a directory structure on an FTP server (really don't think these are used anymore: see SFTP https equivalent)
rtsp, rtspu: identifiers for audio and video media resources 
    can be retrieved through real time streaming protocol 
        "u" denotes that UDP is used to reieve the resource 
        e.g. rtsp://www.joes-hardware.com:554/interview/cto_video
file
    denotes files directly accessible on the host machine   
        by local disk, network filesystem, or some other file-sharing system
news 
    access specfic articles or enwsgroups