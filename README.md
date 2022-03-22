# IDOR - Insecure Direct Object Reference. 

GITHUB Link: https://github.com/dakhilakrishna/IDOR.git

The vulnerability that we have chosen is Insecure Direct Object Reference and the demonstration for the same has been done on Portswigger. The initial aim was to create a dummy website with a database linked to it. This had a .html, .css, .js and .json file in it. Unfortunately, that was not possible as we were unable to host the website. So a work-around that we have decided to go ahead with is implementing the vulnerability and exploiting it on Portswigger. We have also proposed a possible patchwork for our vulnerability and details as to why the patch is a failure is explained in the Patchwork file. Sample codes of the website have been extracted, uploaded and our understanding as to what is happening where has been documented. 

You will be able to find the source code in the <>code section outside the README file. Before jumping into the technicalities of the topic, let us discuss a little about what IDOR is, how they can be detected and prevented. 

IDOR is a flaw in access control that allows unauthorized access to program functionalities by using invalidated user input. This can lead to leakage of the user's sensitive information. IDOR can be found in URLs, body requests, GET or POST methods and sometimes, in cookies too. 

For example, there are several access control flaws in which user-controlled parameter values are utilized to access resources or services directly.  

Consider a website that accesses the customer account page by getting information from the back-end database using the following URL:  

https://vulnerable-website.com/customer_account?customer_number=12345  

Here, it is seen that the customer number is directly referenced which means that if an attacker has access to this link, they can easily tamper the information gained. Failure of additional safeguards may lead to the attacker manipulating, deleting or even publishing confidential information.

Understanding IDOR Vulnerability:  

Although IDOR is not a direct security issue, it does provide a favorable atmosphere for hackers to gain unauthorized access to data.  

1. The hacker uses direct object references to identify the web application and asks for verified information.  

2. A valid HTTP request reveals the direct object reference entity.  

3. The direct object reference entity is changed, and a new HTTP request is made.  

4. An HTTP request is made without user authentication, and the hacker gains access to sensitive data.  

IDOR attacks are of two types:  

Manipulation of the body: The value of a checkbox, radio button, or form field can be changed by an attacker. This allows them to quickly obtain information from other users.  

Tampering with the URL: The URL is changed by altering the HTTP request parameters at the client's end. A URL-altering IDOR attack usually targets the HTTP verbs GET and POST.  


When the following three requirements are satisfied, an unsafe direct object reference vulnerability occurs:  

 1. A direct reference to an internal resource or action is revealed in the program.  

2. The user can change the direct reference by manipulating a URL or form parameter.  

3. The program allows access to the internal object without first determining whether or not the user is permitted.

So what exactly is the issue with our code that leads to IDOR vulnerability?


In simple terms, when using SQL, the following happens:

String query = "SELECT * FROM accts WHERE account = ?";
PreparedStatement pstmt = connection.prepareStatement(query, ... );
pstmt.setString(1, request.getParameter("acct"));
ResultSet results = pstmt.executeQuery( );

In the case of Javascript, which is what we have demonstrated GET method file. 

INSTRUCTIONS:

1. Instructions on the installation of BurpSuite are listed in "BurpSuite Installation".
2. Using BURPSUITE, we capture the requests made from the client-side and make the necessary changes. This is explained in the file "IDOR - Exploitation Demo"
3. The reason behind this vulnerability is explained under "GET method"
4. The exploitation is patched using certain proposed methods of which one of them is explained under "Patchwork"
