# Chapter-1 Web Application (In)security

## 1.0 The Evolution of Web Applications

* Early Internet sites lacked user authentication & security threats were mainly related to web server vulnerabilities.
* Modern web applications require secure two-way communication between servers and browsers to process sensitive information.
* Security -- major concern due to the private and sensitive nature of the data processed.
* In-house developers with limited security knowledge commonly create web applications.
* An attacker who gains access to a web application can steal personal information, commit financial fraud, & harm other users.

## 1.1 Common Web Application Functions

* Web apps have various purposes such as 
    * e-commerce, 
    * social networking, 
    * online banking, 
    * search engines, 
    * auctions, 
    * gaming, 
    * email,
    * interactive content.
* Mobile apps can work like web apps, using a browser or customized client with HTTP-based APIs.
* Web apps are crucial for critical business functions such as 
    * HR, 
    * collaboration, 
    * administrative interfaces, 
    * business applications, 
    * software services, 
    * office applications.
* Cloud solutions save money but increase exposure to potential attacks.
* Web browsers as primary client software for most users inherit common security vulnerabilities due to shared protocols.

## 1.2 Benefits of Web Applications

* HTTP -- lightweight, connectionless protocol used for web applications and can be proxied and tunneled for secure communication.
* Browsers eliminate the need for separate client software distribution and are highly functional with rich user interfaces
* Technologies and Languages used to develop web apps are simple, and there are many platforms and development tools available for beginners, including open-source resources.

## 2.0 Web-App Security: “This Site Is Secure”

* Many web applications claim to be secure due to their use of SSL and compliance with PCI standards. However, the majority of web applications are actually insecure, despite these measures.
* Broken authentication, broken access controls, SQL injection, cross-site scripting, information leakage, and cross-site request forgery are some of the most common vulnerabilities.
* SSL technology protects data in transit but does not prevent attacks that directly target server or client components of an application.
* Most successful attacks exploit vulnerabilities in web applications rather than targeting data in transit.
* Regardless of SSL usage, web applications still need to be secured against vulnerabilities to ensure data safety.

## 2.1 The Core Security Problem: Users Can Submit Arbitrary Input

* Users can interfere with any data transmitted between client and server.
* Users can send requests in any sequence and submit parameters at a different stage than the application expects.
* Attackers can use widely available tools to operate alongside or independently of a browser to attack web applications.
* Majority of attacks against web applications involve submitting crafted input to the server.
* SSL does not stop an attacker from submitting crafted input to the server.
* If any of the attacks are successful, the application is vulnerable regardless of using SSL.

## 3.0 Key Problem Factors

* **Underdeveloped Security Awareness** 
* **Custom development** 
* **Deceptive Simplicity** 
* **Rapidly Evolving Threat Profile**
* **Resource and Time Constraints**
* **Overextended Technologies** 
* **Increasing Demands on Functionality** 

## 4.0 The New Security Perimeter

* Web applications expand the security perimeter to include them and their connection to back-end systems, making them potential gateways for attacks.
* One defective line of code can compromise core back-end systems, requiring defenses to be implemented within the applications themselves.
* Server-side security extends beyond organizations with trust in external applications and services, and accessing the application layer can enable attackers to steal personal information or transfer funds.
* Vulnerable web applications are as threatening as malicious sites, and using email for extended authentication increases attack surface and escalates attacks.
* App owners must protect users from attacks, and there are various open-source resources available for beginners to develop web applications.