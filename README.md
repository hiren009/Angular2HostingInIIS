Hosting Angular 2 application in IIS (Internet Information Server)
===================

Hey! Below are the steps i have followed in order to host angular 2 application in iis. 

----------

**Step 1**:  Publish angular application (ng build --prod), get generated code from dist folder. Create project in iis, and reference this code.

**Step 2**:  It should work. If it does not then check whether our application is hosted in sub-directory like "localhost:xxxx/AngularApp/" then we need to modify baseurl which is in index.html
```
<base href="/AngularApp/">
```

**Step 3**: It should be working by now. One problem you may find is when you manually type the url in browser navigation bar and hit submit then it might not go to desired page and instead give error. 

For Example if you have typed "localhost:xxxx/AngularApp/ProductList" and press enter in browser navigation bar, it might give error.

**Step 4**: If these navigation errors persists, then we need to install Microsoft URL Rewrite Module 2.0 for IIS, and also need to add web.config file containing rewrite. 

To install Microsoft URL Rewrite Module 2.0 for IIS, below link can be useful.
https://www.microsoft.com/en-us/download/details.aspx?id=47337

Also add web.config file in published directory with content like below.
```
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <system.webServer>
    <rewrite>
	  <rules>
		<rule name="Angular" stopProcessing="true">
		  <match url=".*" />
		  <conditions logicalGrouping="MatchAll">
			<add input="{REQUEST_FILENAME}" matchType="IsFile" negate="true" />
			<add input="{REQUEST_FILENAME}" matchType="IsDirectory" negate="true" />
		  </conditions>
		  <action type="Rewrite" url="/" />
		</rule>
	  </rules>
	</rewrite>
  </system.webServer>
</configuration>
```

That's all. I have followed above steps, and my angular application was running perfectly fine.