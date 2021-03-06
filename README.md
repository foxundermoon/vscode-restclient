# REST Client

[![Join the chat at https://gitter.im/Huachao/vscode-restclient](https://badges.gitter.im/Huachao/vscode-restclient.svg)](https://gitter.im/Huachao/vscode-restclient?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge) [![Marketplace Version](https://vsmarketplacebadge.apphb.com/version/humao.rest-client.svg)](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) [![Installs](https://vsmarketplacebadge.apphb.com/installs/humao.rest-client.svg)](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) [![Rating](https://vsmarketplacebadge.apphb.com/rating/humao.rest-client.svg)](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) [![Dependency Status](https://david-dm.org/Huachao/vscode-restclient.svg)](https://david-dm.org/Huachao/vscode-restclient)

REST Client allows you to send HTTP request and view the response in Visual Studio Code directly.

## Main Features
* Send/Cancel/Rerun __HTTP request__ in editor and view response in a separate pane with syntax highlight
* Send __CURL command__ in editor
* Auto save and view/clear request history
* Support _MULTIPLE_ requests in the same file
* View image response directly in pane
* Save raw response to local disk
* Generate code snippets for __HTTP request__ in languages like `Python`, `Javascript` and more!.
* Global variables support (Already support system dynamic variables `{{$guid}}`, `{{$randomInt min max}}` and `{{$timestamp}}` now)
* Remember Cookies for subsequent requests
* Proxy support
* `HTTP` language support
    - `.http` and `.rest` file extensions support
    - Syntax highlight (Request and Response)
    - Auto completion for method, url, header, global dynamic variables and mime types
    - Comments (line starts with `#` or `//`) support
    - Support `json` and `xml` body indentation, comment shortcut and auto closing brackets
    - Code snippets for operations like `GET` and `POST`

## Usage
In editor, type a HTTP request as simple as below:
```http
https://example.com/comments/1
```
Or, you can follow the standard [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec5.html) that including request method, headers and body.
```http
POST https://example.com/comments HTTP/1.1
content-type: application/json

{
    "name": "sample",
    "time": "Wed, 21 Oct 2015 18:27:50 GMT"
}
```
Once you prepared a request, use shortcut `Ctrl+Alt+R`(`Cmd+Alt+R` for macOS), or press `F1` and then select/type `Rest Client: Send Request`, the response will be previewed in a separate panel of Visual Studio Code. When a request is issued, ![cloud upload](images/loading.gif) will be displayed in the status bar, after receiving the response, the icon will be changed to the duration and response size.

> All the shortcuts in REST Client Extension are __ONLY__ available for file language mode `http` and `plaintext`.

### Select Request Text
You may even want to save numerous requests in the same file and execute any of them as you wish easily. REST Client extension could recognize any line begins with three or more consecutive `#` as a delimiter between requests. Place the cursor anywhere between the delimiters, issuing the request as above, and it will first parse the text between the delimiters as request and then send it out.
```http
GET https://example.com/comments/1 HTTP/1.1

###

GET https://example.com/topics/1 HTTP/1.1

###

POST https://example.com/comments HTTP/1.1
content-type: application/json

{
    "name": "sample",
    "time": "Wed, 21 Oct 2015 18:27:50 GMT"
}
```
`REST Client Extension` also provides another flexibility that you can use mouse to highlight the text in file as request text.

## Install
Press `F1`, type `ext install rest-client`.

## Making Request
![rest-client](images/usage.gif)
### Request Line
The first non-empty line of the selection (or document if nothing is selected) is the _Request Line_.
Below are some examples of _Request Line_:
```http
GET https://example.com/comments/1 HTTP/1.1
```
```http
GET https://example.com/comments/1
```
```http
https://example.com/comments/1
```
If request method is omitted, request will be treated as __GET__, so above requests are the same after parsing.

#### Query Strings
You can always write query strings in the request line, like:
```http
GET https://example.com/comments?page=2&pageSize=10
```
Sometimes there may be several query parameters in a single request, putting all the query parameters in _Request Line_ is difficult to read and modify. So we allow you to spread query parameters into multiple lines(one line one query parameter), we will parse the lines in immediately after the _Request Line_ which starts with `?` and `&`, like
```http
GET https://example.com/comments
    ?page=2
    &pageSize=10
```

### Request Headers
The lines immediately after the _request line_ to first empty line are parsed as _Request Headers_. Please provide headers with the standard `field-name: field-value` format, each line represents one header.
Below are examples of _Request Headers_:
```http
User-Agent: rest-client
Accept-Language: en-GB,en-US;q=0.8,en;q=0.6,zh-CN;q=0.4
Content-Type: application/json
```

### Request Body
If you want to provide the request body, please add a blank line after the request headers like the POST example in usage, and all content after it will be treated as _Request Body_.
Below are examples of _Request Body_:

```http
POST https://example.com/comments HTTP/1.1
Content-Type: application/xml
Authorization: token xxx

<request>
    <name>sample</name>
    <time>Wed, 21 Oct 2015 18:27:50 GMT</time>
</request>
```

You can also specify file path to use as a body, which starts with `< `, the file path can be either in absolute or relative(relative to current http file) formats:
```http
POST https://example.com/comments HTTP/1.1
content-type: application/xml
authorization: token xxx

< C:\Users\Default\Desktop\demo.xml
```
```http
POST https://example.com/comments HTTP/1.1
content-type: application/xml
authorization: token xxx

< ./demo.xml
```

### Cancel Request
Once you want to cancel a processing request, use shortcut `Ctrl+Alt+Q`(`Cmd+Alt+Q` for macOS), or press `F1` and then select/type `Rest Client: Cancel Request`.

### Rerun Last Request
Sometimes you may want to refresh the API response, now you could do it simply using shortcut `Ctrl+Alt+L`(`Cmd+Alt+L` for macOS), or press `F1` and then select/type `Rest Client: Rerun Last Request` to rerun last request.

## Making CURL Request
![CURL Request](images/curl-request.png)
We add the capability to directly run [curl request](https://curl.haxx.se/) in REST Client extension. The issuing request command is the same as raw HTTp one. REST Client will automatically parse the request with specified parser.

## Request History
![request-history](images/request-history.png)
Each time we sent a http request, the request details(method, url, headers and body) would be persisted into file. By using shortcut `Ctrl+Alt+H`(`Cmd+Alt+H` for macOS), or press `F1` and then select/type `Rest Client: Request History`, you can view the last __50__ request items(method, url and request time) in the time reversing order, you can select any request you wish to trigger again. After specified request history item is selected, the request details would be displayed in a temp file, you can view the request details or follow previous step to trigger the request again.

You can also clear request history by pressing `F1` and then selecting/typing `Rest Client: Clear Request History`.

## Save Response
![Save Response](images/response.gif)
In the upper right corner of the response preview tab, we add a new icon to save the latest response to local file system. After you click the `Save Response` icon, it will prompt the window with the saved response file path. You can click the `Open` button to open the saved response file in current workspace, or click `Copy Path` to copy the saved response path to clipboard.

## Generate Code Snippet
![Generate Code Snippet](images/code-snippet.gif)
Once you’ve finalized your request in REST Client extension, you might want to make the same request from your own source code. We allow you to generate snippets of code in various languages and libraries that will help you achieve this. Once you prepared a request as previously, use shortcut `Ctrl+Alt+C`(`Cmd+Alt+C` for macOS), or press `F1` and then select/type `Rest Client: Generate Code Snippet`, it will pop up the language pick list, as well as library list. After you selected the code snippet language/library you want, the generated code snippet will be previewed in a separate panel of Visual Studio Code, you can click the `Copy Code Snippet` icon in the tab title to copy it to clipboard.

## HTTP Language
Add language support for HTTP request, with features like __syntax highlight__, __auto completion__ and __comment support__, when writing HTTP request in Visual Studio Code. By default, the language association will be automatically activated in two cases:

1. File with extension `.http` or `.rest`
2. First line of file follows standard request line in [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec5.html), with `Method SP Request-URI SP HTTP-Version` format

If you want to enable language association in other cases, just change the language mode in the right bottom of `Visual Studio Code` to `HTTP`.

![HTTP Language](images/http.png)
### Auto Completion
Currently auto completion will be enabled for following five categories:

1. HTTP Method
2. HTTP URL from request history
3. HTTP Header
4. Global dynamic variables
5. MIME Types for `Accept` and `Content-Type` headers

## Variables
### Global Variables
Global variables provide a set of variables that can be used in every part of the request(Url/Headers/Body) in the format `{{variableName}}`. Currently, we provide a few dynamic variables which you can use in your requests. The variable names are _case-sensitive_.
* `{{$guid}}`: Add a RFC 4122 v4 UUID
* `{{$randomInt min max}}`: Returns a random integer between min (included) and max (excluded)
* `{{$timestamp}}`: Add UTC timestamp of now. You can even specify any date time based on current time in the format `{{$timestamp number option}}`, e.g., to represent 3 hours ago, simply `{{$timestamp -3 h}}`; to represent the day after tomorrow, simply `{{$timestamp 2 d}}`. The option string you can specify in timestamp are:
```
    +---------+--------------+
    | Options | Descriptions |
    +---------+--------------+
    |    y    |    Years     |
    +---------+--------------+
    |    Q    |   Quarters   |
    +---------+--------------+
    |    M    |    Months    |
    +---------+--------------+
    |    w    |    Weeks     |
    +---------+--------------+
    |    d    |     Days     |
    +---------+--------------+
    |    h    |    Hours     |
    +---------+--------------+
    |    m    |   Minutes    |
    +---------+--------------+
    |    s    |   Seconds    |
    +---------+--------------+
    |    ms   | Milliseconds |
    +---------+--------------+
```

### Variables Sample:
```http
POST https://example.com/comments HTTP/1.1
Content-Type: application/xml
X-Request-Id: {{$guid}}

{
    "updated_at": "{{$timestamp}}",
    "created_at": "{{$timestamp -1 d}}",
    "review_count": "{{$randomInt 5, 200}}"
}
```

## Settings
* `rest-client.followredirect`: Follow HTTP 3xx responses as redirects. (Default is __true__)
* `rest-client.defaultuseragent`: If User-Agent header is omitted in request header, this value will be added as user agent for each request. (Default is __vscode-restclient__)
* `rest-client.timeoutinmilliseconds`: Timeout in milliseconds. 0 for infinity. (Default is __0__)
* `rest-client.showResponseInDifferentTab`: Show response in different tab. (Default is __false__)
* `rest-client.rememberCookiesForSubsequentRequests`: Save cookies from `Set-Cookie` header in response and use for subsequent requests. (Default is __true__)
* `rest-client.enableTelemetry`: Send out anonymous usage data. (Default is __true__)
* `rest-client.excludeHostsForProxy`: Excluded hosts when using proxy settings. (Default is __[]__)

Rest Client respects the proxy settings made for Visual Studio Code (`http.proxy` and `http.proxyStrictSSL`).

## License
[MIT License](LICENSE)

## Change Log
See CHANGELOG [here](CHANGELOG.md)

## Feedback
Please provide feedback through the [GitHub Issue](https://github.com/Huachao/vscode-restclient/issues) system, or fork the repository and submit PR.
