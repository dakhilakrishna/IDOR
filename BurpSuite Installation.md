BurpKit

## Introduction

BurpSuite is the next generation of web application penetration testing - using WebKit to own the web.
BurpKit is a BurpSuite plugin which helps in assessing complex web apps that render the contents of
their pages dynamically. It also provides a bi-directional JavaScript bridge API which allows users
to create quick one-off BurpSuite plugin prototypes which can interact directly with the DOM and
Burp's extender API.


System Requirements:

BurpKit has the following system requirements:
- Oracle JDK &gt;=8u50 and &lt;9 [download](https://www.oracle.com/java/technologies/downloads/)
- At least 4GB of RAM


Installation:

1. Download the latest prebuilt release from: (https://github.com/allfro/BurpKit/releases).
2. Open BurpSuite and navigate to the `Extender` tab.
3. Under `Burp Extensions` click the `Add` button.
4. In the `Load Burp Extension` dialog, make sure that `Extension Type` is set to `Java` and click the `Select file ...` button under `Extension Details`.
5. Select the `BurpKit-<version>.jar` file and click `Next` when done.

Reference: https://github.com/allfro/BurpKit.git

