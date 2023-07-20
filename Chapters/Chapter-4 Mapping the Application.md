 # Chapter-4 Mapping the Application

This chapter provides practical steps, techniques, tricks, and tools for effective application mapping.

# 4.1 Enumerating Content and Functionality  

## 4.1.1 Web Spidering  

- spidering involves automated tools that request web pages, parse for links, recursively explore new content until none is found.
- It goes beyond basic spidering by parsing HTML forms, submitting them with random values, following forms based navigation.
- **Tools:** `Burp Suite`, `WebScarab`, `Zed Attack Proxy`, and `CAT`.
  
1. Check the **robots.txt** file: Some web servers have a `robots.txt` file that lists URLs to exclude from spidering.
2. **REST style URLs**: If an application uses REST style URLs, spidering specific paths can provide unique links to items within those paths.
3. Limitations of automated spidering:
   - `Unusual navigation mechanisms` and `compiled client side objects` may be `missed`.
   - Fine grained `input validation` checks can `prevent automated tools` from proceeding beyond certain forms.
   - Forms based navigation with `dynamic URLs may require multiple requests` to fully explore the application.
   - `Volatile data within URLs` can cause the spider to `continue running indefinitely`.
   - Handling `authentication` is essential for accessing protected functionality, but the spider's operation may `break authenticated sessions`
   >     requests links can be extremely dangerous. It can delete users, shut down a database, restart the server, and the like.

## 4.1.2 User Directed Spidering 

- User spidering is a `controlled technique` where the user manually navigates through an application using a standard browser.
- The user's traffic is passed through an intercepting `proxy` and `spider` tool, which builds a map of the application by monitoring requests and responses.
- This technique allows the user to `follow complex navigation mechanisms`, `control submitted data`, `maintain an authenticated session`, and `enumerate dangerous functionality`.
- User directed spidering is effective in discovering content that may be missed by automated spiders, such as links accessed through JavaScript functions.
- The spider tool can identify hidden links, even if they are not visible on the screen.

1. In addition to proxy/spider tools, `browser extensions` can be useful for HTTP and HTML analysis within the browser interface.
2. Tools like `IEWatch` provide detailed information about requests, responses, headers, parameters, cookies, links, scripts, forms, and thick client components.

   >     Try browsing with JavaScript enabled and disabled, and with cookies enabled and disabled.

## 4.1.3 Discovering Hidden Content

>     Effective discovery of hidden content requires a combination of automated and manual techniques and often relies on a degree of luck.
- Applications often have hidden content and functionality that is not directly accessible or linked from the main visible content.
- Hidden content can include `testing`/`debugging` functionality, functionality available to specific `user categories`, `backup` `copies` of files, new/deployed but `unlinked functionality`, default functionality in off the shelf applications, `old versions` of files, `configuration`/`include` files, source files, `comments` in source code, and `log` files.

### 4.1.3.1 Brute Force Techniques:

- Common directory names or identifiers can be brute forced by iterating through a list of names and capturing server responses.
- Bruteforcing for directories can be followed by searching for additional pages within specific directories.
  ```
   http://eis/About/
   http://eis/abstract/
   http://eis/academics/
   http://eis/accessibility/
   http://eis/accounts/
   http://eis/action/
  ```
- Tools used: `Burp Intruder`
  
- Responses from the server should be analyzed manually, as different response codes and messages provide insights into the existence and accessibility of resources.

>     Many applications handle requests for nonexistent resources in a customized way, often returning a bespoke error message and a 200 response code. 

| Response Code                    | Meaning                                                                |
|----------------------------------|------------------------------------------------------------------------|
| 302 Found                        | Indicates redirect to a login page or error message                    |
| 400 Bad Request                  | May indicate invalid syntax in the request or wordlist                 |
| 401 Unauthorized / 403 Forbidden | Suggests the existence of the requested resource but restricted access |
| 500 Internal Server Error        | Indicates missing parameters or server-side issues                     |

### 4.1.3.2 Inference from Published Content

- Inferring from identified resources, naming schemes can help discover hidden content. 
```(if `ForgotPassword`, then `ResetPassword`)```
- Naming patterns, such as `capitalization` or specific words, can be used to fine-tune automated enumeration.
  ```
   http://eis/auth/AddPassword
   http://eis/auth/ForgotPassword
   http://eis/auth/GetPassword
   http://eis/auth/ResetPassword
   http://eis/auth/RetrievePassword
   http://eis/auth/UpdatePassword
  ```
- `Numeric values` and `dates` in resource names can reveal additional content. (`AnnualReport2009.pdf`)
- Reviewing client-side code, like HTML and JavaScript, can provide clues about hidden server-side content.
- `Comments`, `include` files, and `thick-client components` may contain sensitive information.
- Adding potential names and `common file extensions (txt, bak, src, inc, and old)` to lists can uncover more resources.
- Search for `temporary files (.DS_Store)` inadvertently created by development tools.
- Perform automated exercises combining directories, file stems, and extensions.
- Consider focused brute-force exercises based on consistent naming schemes. <br>(if `AddDocument.jsp` and `ViewDocument.jsp` then, `XxxDocument.jsp`)
- Perform exercises recursively, using new content and patterns for further discovery.

### 4.1.3.3 Use of Public Information

- Search engines index and cache web content, including hidden or removed content.
- WayBack Machine store snapshots of websites, providing access to past versions.
- Apply advanced Google Dork search techniques to web-apps
- Search for information in Groups and News for diverse results.
- Check the omitted search results in Google
- Developers, programmers' technical questions in internet forums reveal details about the application's `technologies`, `functionality`, development `challenges`, `security bugs`, `configuration files`, `log files`, and even `source code snippets`.
- Get the list of their emails and search the internet forum

> Note: Old content and functionality discovered through public sources may still be usable and may contain vulnerabilities that don't exist in the current application. 

### 4.1.3.4 Leveraging the Web Server

- Bugs in web server software can lead to directory listing or obtaining raw source for dynamic server-executable pages.
- Leveraging default content or third-party components can aid in attacking the web application.
- `Wikto` is a free tool for scanning web applications, offering configurable brute-force lists for content.
- It can identify directories and URLs that are not discovered through automated or user-driven spidering

## 4.1.4 Application Pages Versus Functional Paths

- Traditional web application structure involves accessing individual functions through unique URLs.
   ```
   POST /bank.jsp HTTP/1.1
   Host: wahh-bank.com
   Content-Length: 106
   servlet=TransferFundsmethod=confirmTransferfromAccount=10372918&toAccount=3910852&amount=291.23&Submit=Ok
   ```
- `Functional paths` focus on the application's core functionality and logical relationships between functions.
- functionality through functional paths provides a more informative catalog.<br>
  <img src="https://lh3.googleusercontent.com/pw/AIL4fc_-igcK90kB-Q6zhy7chRp8w2ftjJWcGQcY8EIvuuny-AZDT89FqYDca9lOg4GywbIINnsBnF8YxltZzUZVPPw1vtrVojmf0mo9dWyiamSHcfJIYDc7giEqckoYP-_UwPWPmQdcGQ9WnFvrSiVXL6ez=w968-h750-s-no" width="550" height="350">
- Identifying logical relationships helps understand developers' expectations and formulate potential attacks.

## 4.1.5 Discovering Hidden Parameters

- Some applications use parameters to control logic or enable specific features.
  
  ```
  application may behave differently if the parameter `debug=true` is added to the query string. It might turn off certain input validation checks, allow the user to bypass certain access controls, or display verbose debug informa- tion in its response.
  ```

- The application may not explicitly indicate the handling of certain parameters.
- The effect of a parameter can only be detected by guessing and submitting different values.

Hack Steps:

1. Generate numerous requests using common parameter names and values.
2. Monitor responses for anomalies indicating the impact of the added parameter on the application's processing.
3. Focus on specific pages or functions that are more likely to have implemented debug logic for uncovering hidden parameters.

# 4.2 Analyzing the Application

- Investigate the application's core functionality and peripheral behavior.
- Identify the application's `security` mechanisms, `session state` management, `access controls`, and `authentication` mechanisms.
- Identify all user input processing locations, including URLs, query string parameters, POST data, and cookies.
- Analyze client-side technologies such as forms, scripts, and cookies.
- Analyze server-side technologies such as static and dynamic pages, request parameters, SSL usage, web server software, and database interactions.
- Gather information about the internal structure and functionality of the server-side application.

## 4.2.1 Identifying Entry Points for User Input

- Pay attention to user input captured in `URL strings`, `query string` parameters, `POST request` bodies, `cookies`, and other `HTTP headers`.
- HTTP headers like `User-Agent`, `Referer`, `Accept`, `Accept-Language`, and `Host` should be considered as possible entry points for attacks.

**4.2.1.1 URL File Paths:**

      http://eis/shop/browse/electronics/iPhone3G/

 - URLs preceding the query string can function as data parameters and should be treated as entry points for user input
 - `electronics` and `iPhone3G` should be treated as parameters to store a search function.

**4.2.1.2 Request Parameters:**

- Some applications may employ nonstandard parameter formats, requiring special handling during testing.

```
/dir/file;foo=bar&foo2=bar2
/dir/file?foo=bar$foo2=bar2
/dir/file/foo%3dbar%26foo2%3dbar2
/dir/foo.bar/file
/dir/foo=bar/file
/dir/file?param=foo:bar
/dir/file?data=%3cfoo%3ebar%3c%2ffoo%3e%3cfoo2%3ebar2%3c%2ffoo2%3e
```

**4.2.1.3 HTTP Headers:**

- HTTP headers like `Referer` and `User-Agent` may be entry points for attacks.
- Spoofing User-Agent headers can reveal additional attack surfaces or vulnerabilities.
- By adding a suitably crafted `X-Forwarded-For` header, you may be able to deliver attacks such as SQL injection or persistent
cross-site scripting.

**4.2.1.4 Out-of-Band Channels:**

- Applications that receive user input through out-of-band channels may have hidden entry points.
- Examples include `web mail applications`, `content retrieval functions`, `intrusion detection applications`, and `API interfaces`.

## 4.2.2 Identifying Server-Side Technologies

- Server technologies can be fingerprinted using various clues and indicators.
- `Banner grabbing` reveals detailed information about the web server and installed components.
- `HTTP fingerprinting` examines the behavior of the web server to identify software in use.
- `File extensions` in URLs often disclose the platform or programming language used.
  
   | asp        | Microsoft Active Server Pages           |
   |------------|-----------------------------------------|
   | aspx       | Microsoft ASP.NET                       |
   | jsp        | Java Server Pages                       |
   | cfm        | Cold Fusion                             |
   | php        | The PHP language                        |
   | d2w        | WebSphere                               |
   | pl         | The Perl language                       |
   | py         | The Python language                     |
   | dll        | Usually compiled native code (C or C++) |
   | nsf or ntf | Lotus Domino                            |
- `Directory names` and `session tokens` can provide information about the technology in use.
  | servlet                      | Java servlets                            |
  |------------------------------|------------------------------------------|
  | pls                          | Oracle Application Server PL/SQL gateway |
  | cfdocs or cfide              | Cold Fusion                              |
  | SilverStream                 | The SilverStream web server              |
  | WebObjects or {function}.woa | Apple WebObjects                         |
  | rails                        | Ruby on Rails                            |

   | JSESSIONID        | The Java Platform    |
   |-------------------|----------------------|
   | ASPSESSIONID      | Microsoft IIS server |
   | ASP.NET_SessionId | Microsoft ASP.NET    |
   | CFID/CFTOKEN      | Cold Fusion          |
   | PHPSESSID         | PHP                  |
   | rails             | Ruby on Rails        |

- `Third-party code components` may introduce known vulnerabilities or provide insights from similar applications.

## 4.2.3 Identifying Server-Side Functionality

- Observing clues disclosed to the client can reveal server-side functionality and structure.

- It is often necessary to consider the whole URL and application context to guess the function of different parts of a request.

**4.2.3.1 Extrapolating Application Behavior:**
- Consistent application behavior across functions allows extrapolation of server-side functionality.
- Examples: Common input validation logic, shared obfuscation schemes, inconsistent error handling.

**4.2.3.2 Isolating Unique Application Behavior:**
- In secure applications, specific areas may have vulnerabilities due to being retrospectively added or not integrated with the general security framework.
- Identifiable through GUI differences, parameter naming conventions, or comments in source code.

## 4.2.4 Mapping the Attack Surface

- Identifying attack surfaces and associated vulnerabilities in different areas of the application.

|           Clientside validation           |             Checks may not be replicated on the server             |
|:-----------------------------------------:|:------------------------------------------------------------------:|
| Database interaction                      | SQL injection                                                      |
| File uploading and downloading            | Path traversal vulnerabilities, stored cross-site scripting        |
| Display of user-supplied data             | Cross-site scripting                                               |
| Dynamic redirects                         | Redirection and header injection attacks                           |
| Social networking features                | username enumeration, stored cross-site scripting                  |
| Login                                     | Username enumeration, weak passwords, ability to use brute force   |
| Multistage login                          | Logic flaws                                                        |
| Session state                             | Predictable tokens, insecure handling of tokens                    |
| Access controls                           | Horizontal and vertical privilege escalation                       |
| User impersonation functions              | Privilege escalation                                               |
| Use of cleartext communications           | Session hijacking, capture of credentials and other sensitive data |
| Off-site links                            | Leakage of query string parameters in the Referer header           |
| Interfaces to external systems            | Shortcuts in the handling of sessions and/or access controls       |
| Error messages                            | Information leakage                                                |
| E-mail interaction                        | E-mail and/or command injection                                    |
| Native code components or interaction     | Buffer overflows                                                   |
| Use of third-party application components | Known vulnerabilities                                              |
| Identifiable web server software          | Common configuration weaknesses, known software bugs               |
