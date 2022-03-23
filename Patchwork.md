Patchwork for IDOR vulnerability

Detection in logs:

1. Same IP addresses accessing various portions of accounts, e.g. acc_ID=1 (attacker can alter account ID to 2), with continual changes in ID numbers from the same IP addresses within a particular period, indicating the Insecure Direct Object being accessed. 

2. If the source IP address has a successful HTTP status code of 300, a failure code of 5xx, or any other code, it means that efforts were made but the code was not successfully exploited. 

3. Rule of Simple Correlation Reasoning: WAF/RASP logs contain acc_ID=1 to 999 AND Status Codes 300 AND SAME Source IP addresses: x.x.x.x 

4. Examine web application logs, WAF, and RASP for information on what is accessed from a third party. 

5. Examine the results of the vulnerability scanners and see whether the application has any new vulnerabilities.

Preventative Actions:

We ensure that access is restricted and that the resources are within our control. Like resources mapped to the owner using a non-sequence key or a random non-guessable integer. 

Before issuing the resource when the user wants it, check the user's authorisation. In the application, a powerful User role function should be implemented. Before releasing the resource, make sure the user role and hash are correct. 

Instead of mapping the item with a straight ID, we can use a hash in this situation. It is advisable to add salt with hash to make it more challenging.


Burpsuite:

Attackers commonly employ the Burp Suite Tool to carry out such attacks. The measures to be taken are as follows: 

1. Capture the Request: First and foremost, an attacker must choose a target website on which to launch an IDOR assault. The website is then added to the scope, and a spider is used to collect all URLs with certain specifications. 

2. Filter the variables. Request: After the first step, we'll use the parameter filters to filter our collected request. Only that parameter or Injection points will be chosen by an attacker as targets for their assaults.

3. Forward request to Repeater: If an attacker discovers an injection point where they may run IDOR, the request will be forwarded to the repeater. www.xyz.com/myaccount/uid=19 is an example of a potentially vulnerable URL. The "UID" appears to be susceptible in this case. 

4. Parameter tampering: Now that the attacker obtains the susceptible injection point, they will attempt to carry out the IDOR attack using social engineering or the pattern specified in the injection point. For example, an attacker may alter uid from 19 to 20, resulting in the account of another user with id number 20 being opened.

As discussed earlier, we employ the Session Storage method to overcome IDOR to an extent. Session Storage variable stores the user's data for only one session and is deleted as soon as the user exits or closes the browser.

Syntax: sessionStorage
To store data: sessionStorage.setItem("key", "value");
To remove data: sessionStorage.clear();

For example, when the following snippet is incorporated in our code, every time a user information is requested, the server makes sure that the information fetched belongs to the session of the user. Essentially, when an "attacker" tries to barge, the page simply fails to load or the corresponding fallback is executed. 

sessionStorage.setItem("lastname", "2151425");
sessionStorage.getItem("lastname");

Another work-around is REST API. A RESTful API sends a representation of the resource's state to the requester or endpoint when a client request is made. JSON (Javascript Object Notation), HTML, XLT, Python, PHP, or plain text are some of the forms in which this information, or representation, is sent through HTTP.

A major advantage of using session storage is that they are compatible with all browers in all versions.

Steps:

1. Create Project. Then a new folder on the desktop. In this, we create index.html and app.js files. The app.js file will include all our JavaScript methods related to the sessionStorage object. This file will then be imported into index.html.
2. We then create a view as per our requirement: 

This stage will include creating a basic web page that will be used to interact with the session storage features using HTML. The index.html file must include the app.js file. 

This gives us access to the session storage functions we need.

<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+abc/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">

3. We import the app.js file using the code snippet below:
  <script type="text/javascript" src="main.js"></script>

A sample code before incorporating our session storage:

```
<!DOCTYPE html>
<html>

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width">
  <title>sessionStorage</title>
  <script type="text/javascript" src="main.js"></script>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
</head>

<body>
  <div class="container" id="formDiv">
        <form class="form-signin" id="carForm">
            <h1>Enter details</h1>
            <label for="carBrand" class="sr-only">Car</label>
            <input type="text" id="carBrand" class="form-control" placeholder="Car brand" required autofocus><br>
            <label for="carPrice" class="sr-only">Price</label>
            <input type="text" id="carPrice" class="form-control" placeholder="Price" required><br>
            <button class="btn btn-lg btn-primary btn-block" type="submit">Submit</button>
        </form>

        <button class="btn btn-lg btn-primary btn-block" id="retrieveButton">Retrieve records</button>
        <div id="retrieve"></div>

        <button class="btn btn-lg btn-danger btn-block" id="removeButton">Remove record</button>
        <button class="btn btn-lg btn-danger btn-block" id="clearButton">Clear all records</button>
  </div>

</body>
</html>


The app.js file contains all of these functions. The store() function will accept user input and give the data as arguments to the setItem() method. These values or objects will be saved in session storage as a consequence. The store() method's code is as follows:

function store(){ //stores items in sessionStorage
    var brand = document.getElementById('carBrand').value;
    var price = document.getElementById('carPrice').value;

    const car = {
        brand: brand,
        price: price,
    }

    window.sessionStorage.setItem('car',JSON.stringify(car));  
    //converting object to string
}
```

The reason behind this patchwork is that the user's authentication state may be checked using session storage. The homepage can be forwarded to logged-in users. Users who have not yet registered are forwarded to the authentication page. This patch is not 100% successful and can still be bypassed when the salted hashes are brute-forced. Even though it takes time, it is still possible. Our main takeaway from this is that it certainly makes it harder for the attacker to gain access to potentially confidential information.

Reference: https://www.section.io/engineering-education/how-and-when-to-apply-session-storage-with-javascript/
