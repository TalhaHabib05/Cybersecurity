# Web Hacking & Vulnerability Research

This document serves as a technical log of my progress in offensive web security, detailing the underlying mechanics of web applications and the exploitation of standard vulnerabilities.

---

## 1. Web Application Architecture
A web application functions through the interaction of client-side and server-side technologies.

* **Front End (Client-Side):** The interface the user interacts with, primarily built using:
    * **HTML:** The structural foundation and data of the page.
    * **CSS:** The visual styling and layout.
    * **JavaScript (JS):** The logic and dynamic behavior executed within the browser.
* **Back End (Server-Side):** The underlying infrastructure, including:
    * **Web Servers:** Host and deliver the application content.
    * **Databases:** Store, modify, and retrieve persistent data.
    * **Web Application Firewall (WAF):** A protective layer filtering malicious incoming traffic before it reaches the web server.

---

## 2. Uniform Resource Locator (URL) Anatomy


A URL directs the browser to specific resources. Understanding its structure is critical for identifying injection points and access control flaws.

* **Scheme:** The protocol used (e.g., `http://` or `https://`). HTTPS ensures encrypted data transmission.
* **Domain/Host:** The unique address of the server. *Security risk: Typosquatting (malicious domains resembling legitimate ones).*
* **Port:** Directs traffic to the correct service (e.g., Port 80 for HTTP, 443 for HTTPS).
* **Path:** The specific file or endpoint on the server.
* **Query String:** Data appended after a `?`, often used for search terms (e.g., `?search=admin`). *Security risk: Prime target for injection attacks.*
* **Fragment:** Appended after a `#`, directing the browser to a specific section of the page.

---

## 3. The HTTP Protocol


HTTP messages are the packets of data exchanged between the client and server. 

### HTTP Requests
Sent by the client to trigger an action. The Request Line defines the interaction: `[METHOD] [PATH] [VERSION]`

* **GET:** Retrieves data. *Security note: Never pass sensitive data in GET requests as it is stored in plaintext logs.*
* **POST:** Submits data to the server. *Security note: Requires strict input validation to prevent XSS and SQLi.*
* **PUT/PATCH:** Updates existing resources.
* **DELETE:** Removes a resource.
* **OPTIONS:** Reveals which HTTP methods are supported by the server.

### HTTP Responses
Returned by the server. The Status Line includes the HTTP version and a Status Code indicating the result:
* **1xx (Informational):** Request received, continuing process.
* **2xx (Success):** The action was successfully received and accepted (e.g., `200 OK`).
* **3xx (Redirection):** Further action must be taken to complete the request (e.g., `301 Moved Permanently`).
* **4xx (Client Error):** The request contains bad syntax or cannot be fulfilled (e.g., `404 Not Found`, `403 Forbidden`).
* **5xx (Server Error):** The server failed to fulfill an apparently valid request (e.g., `500 Internal Server Error`).

---

## 4. HTTP Headers & Security
Headers are key-value pairs that dictate how clients and servers communicate and handle data.

### Standard Headers
* **Host:** Specifies the target domain.
* **User-Agent:** Identifies the client's browser and operating system.
* **Content-Type:** Defines the data format (e.g., `application/json` or `application/x-www-form-urlencoded`).
* **Set-Cookie:** Sent by the server to issue a session token.

### Security Headers
Modern web defense relies on properly configured response headers to mitigate client-side attacks:

* **Content-Security-Policy (CSP):** Mitigates XSS by dictating exactly which domains the browser is allowed to load executable scripts from (e.g., `script-src`).
* **Strict-Transport-Security (HSTS):** Forces the browser to communicate exclusively over HTTPS, preventing downgrade attacks. The `includeSubDomains` directive ensures complete coverage.
* **X-Content-Type-Options:** Using the `nosniff` directive prevents the browser from guessing MIME types, mitigating malicious file execution.
* **Cookie Security Flags:**
    * `Secure`: Ensures the cookie is only transmitted over encrypted HTTPS connections.
    * `HttpOnly`: Prevents JavaScript from accessing the cookie, heavily mitigating the impact of XSS attacks.
