**Explain the code Webdriver driver = new Chromedriver(); / Internal working when this line is executed / How does the Selenium WebDriver work?**

**Ans.**

Once we trigger our script for execution, the code is converted to an URL via the JSON Wire Protocol over HTTP. 
The URL will then be fed to the browser driver. 
The browser driver takes the help of HTTP server to get the HTTP request.
The browser driver then passes on the request to the actual browser via HTTP. 
Finally, the test script gets executed. If there is a POST request there will be action on the browser.
If there is a GET request, a response gets generated at the browser. 
It shall be passed via HTTP to the browser driver. 
Then the browser driver passes it to the IDE with the JSON Wire Protocol.

