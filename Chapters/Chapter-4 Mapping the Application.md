# Chapter-4 Mapping the Application

This chapter provides practical steps, techniques, tricks, and tools for effective application mapping.

# 4.1 Enumerating Content and Functionality  

## 4.1.1 Web Spidering  

- spidering involves automated tools that request web pages, parse for links, recursively explore new content until none is found.
- It goes beyond basic spidering by parsing HTML forms, submitting them with random values, following forms-based navigation.
- **Tools:** `Burp Suite`, `WebScarab`, `Zed Attack Proxy`, and `CAT`.
  
1. Check the **robots.txt** file: Some web servers have a `robots.txt` file that lists URLs to exclude from spidering.
2. **REST-style URLs**: If an application uses REST-style URLs, spidering specific paths can provide unique links to items within those paths.
3. Limitations of automated spidering:
   - `Unusual navigation mechanisms` and `compiled client-side objects` may be missed.
   - Fine-grained `input validation` checks can `prevent automated tools` from proceeding beyond certain forms.
   - Forms-based navigation with `dynamic URLs may require multiple requests` to fully explore the application.
   - `Volatile data within URLs` can cause the spider to `continue running indefinitely`.
   - Handling `authentication` is essential for accessing protected functionality, but the spider's operation may `break authenticated sessions`

## 4.1.2 User-Directed Spidering 

- User spidering is a `controlled technique` where the user manually navigates through an application using a standard browser.
- The user's traffic is passed through an intercepting `proxy` and `spider` tool, which builds a map of the application by monitoring requests and responses.
- This technique allows the user to `follow complex navigation mechanisms`, `control submitted data`, `maintain an authenticated session`, and `enumerate dangerous functionality`.
- User-directed spidering is effective in discovering content that may be missed by automated spiders, such as links accessed through JavaScript functions.
- The spider tool can identify hidden links, even if they are not visible on the screen.

1. In addition to proxy/spider tools, `browser extensions` can be useful for HTTP and HTML analysis within the browser interface.
2. Tools like `IEWatch` provide detailed information about requests, responses, headers, parameters, cookies, links, scripts, forms, and thick-client components.

## 4.1.3 Discovering Hidden Content

- Applications often have hidden content and functionality that is not directly accessible or linked from the main visible content.
- Hidden content can include `testing`/`debugging` functionality, functionality available to specific `user categories`, `backup` `copies` of files, new/deployed but `unlinked functionality`, default functionality in off-the-shelf applications, `old versions` of files, `configuration`/`include` files, source files, `comments` in source code, and `log` files.
- Discovering hidden content requires a combination of automated and manual techniques and sometimes relies on luck.

### Brute-Force Techniques:

- Automation can be used to make a large number of requests to the web server to guess hidden functionality.
- Common directory names or identifiers can be brute-forced by iterating through a list of names and capturing server responses.
- Tools like `Burp Intruder` can be used to automate the brute-forcing process.
- Brute-forcing for directories can be followed by searching for additional pages within specific directories.
- Responses from the server should be analyzed manually, as different response codes and messages provide insights into the existence and accessibility of resources.
- Response codes can indicate the presence of interesting content.

| Response Code                    | Meaning                                                                |
|----------------------------------|------------------------------------------------------------------------|
| 302 Found                        | Indicates redirect to a login page or error message                    |
| 400 Bad Request                  | May indicate invalid syntax in the request or wordlist                 |
| 401 Unauthorized / 403 Forbidden | Suggests the existence of the requested resource but restricted access |
| 500 Internal Server Error        | Indicates missing parameters or server-side issues                     |

### Inference from Published Content

- Inferring from identified resources, naming schemes can help discover hidden content. (if `ForgotPassword`, then `ResetPassword`)
- Naming patterns, such as `capitalization` or specific words, can be used to fine-tune automated enumeration.
- `Numeric values` and `dates` in resource names can reveal additional content. (`AnnualReport2009.pdf`)
- Reviewing client-side code, like HTML and JavaScript, can provide clues about hidden server-side content.
- `Comments`, `include` files, and `thick-client components` may contain sensitive information.
- Adding potential names and `common file extensions` (txt, bak, src, inc, and old) to lists can uncover more resources.
- Search for `temporary files` (.DS_Store) inadvertently created by development tools.
- Perform automated exercises combining directories, file stems, and extensions.
- Consider focused brute-force exercises based on consistent naming schemes. (if `AddDocument.jsp` and `ViewDocument.jsp` then, `XxxDocument.jsp`)
- Perform exercises recursively, using new content and patterns for further discovery.

## 4.1.4 Application Pages Versus Functional Paths


## 4.1.5 Discovering Hidden Parameters