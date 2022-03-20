# IDOR


IDOR is a flaw in access control that allows unauthorized access to program functionalities by using invalidated user input. IDOR can lead to the leaking of sensitive information, data manipulation, and other issues. They're a form of access control flaw when a program utilizes user-supplied input to get direct access to objects.  

  

Examples of IDOR  

There are several examples of access control flaws in which user-controlled parameter values are utilized to access resources or services directly.  

  

Consider a website that accesses the customer account page by getting information from the back-end database using the following URL:  

 https://insecure-website.com/customer_account?customer_number=132355  

  

In this case, the customer number is directly utilized as a record index in queries run on the back-end database. If no additional safeguards are in place, an attacker can change the customer number value to access other customers' information by circumventing access constraints. This is an example of a horizontal privilege escalation caused by an IDOR vulnerability.  

  

Understanding IDOR Vulnerability:  

  

Although IDOR is not a direct security issue, it does provide a favorable atmosphere for hackers to gain unauthorized access to data.  

1. The hacker uses direct object references to identify the web application and asks for verified information.  

2. A valid HTTP request reveals the direct object reference entity.  

3. The direct object reference entity is changed, and a new HTTP request is made.  

 4. An HTTP request is made without user authentication, and the hacker gains access to sensitive data.  

  

IDOR attacks are of two types:  

Manipulation of the body: The value of a checkbox, radio button, or form field can be changed by an attacker. This allows them to quickly obtain information from other users.  

Tampering with the URL: The URL is changed by altering the HTTP request parameters at the client's end. A URL-altering IDOR attack usually targets the HTTP verbs GET and POST.  

  

IDORs at Work  

When the following three requirements are satisfied, an unsafe direct object reference vulnerability occurs:  

 1. A direct reference to an internal resource or action is revealed in the program.  

2. The user can change the direct reference by manipulating a URL or form parameter.  

3. The program allows access to the internal object without first determining whether or not the user is permitted.
