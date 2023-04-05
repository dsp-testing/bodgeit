The BodgeIt Store is a vulnerable web application which is currently aimed at people who are new to pen testing.

> ### Please note that The BodgeIt Store is no longer being worked on
> #### You are strongly recommended to use [OWASP Juice Shop](https://www.owasp.org/index.php/OWASP_Juice_Shop_Project) instead!

Note that the BodgeIt Store is now available as a Docker image: https://hub.docker.com/r/psiinon/bodgeit/ 

Some of its features and characteristics:
* Easy to install - just requires java and a servlet engine, e.g. Tomcat
* Self contained (no additional dependencies other than to 2 in the above line)
* Easy to change on the fly - all the functionality is implemented in JSPs, so no IDE required
* Cross platform
* Open source
* No separate db to install and configure - it uses an 'in memory' db that is automatically (re)initialized on start up

All you need to do is download and open the zip file, and then extract the war file into the webapps directory of your favorite servlet engine.

Then point your browser at (for example) http://localhost:8080/bodgeit

You may find it easier to find vulnerabilities using a pen test tool.

If you dont have a favourite one, I'd recommend the [Zed Attack Proxy](https://www.owasp.org/index.php/ZAP) (for which I'm the project lead).

The Bodge It Store include the following significant vulnerabilities:
* Cross Site Scripting
* SQL injection
* Hidden (but unprotected) content
* Cross Site Request Forgery
* Debug code
* Insecure Object References
* Application logic vulnerabilities If you spot any others then let me know ;)

There is also a 'scoring' page (linked from the 'About Us' page) where you can see various hacking challenges and whether you have completed them or not.

In the relatively near future I'm hoping to add things like:
* Ajax requests
* More vulnerabilities (of course)

You can now also perform automated security regression tests on the Bodge It Store - see the wiki.

Any feedback (or offers of help to develop it further;) would be appreciated.

## Build

# BUILD FAILED /bodgeit/build.xml:28: destination directory "/bodgeit/build/WEB-INF/classes" does not exist or is not a directory
mkdir build/WEB-INF/classes

#    [javac] error: Source option 5 is no longer supported. Use 7 or later.
#    [javac] error: Target option 5 is no longer supported. Use 7 or later.
```diff
- <javac target="1.5" ...
+ <javac target="1.7" ...
```
ant -noinput -f "build.xml" "compile"

## Maven Build
Added to inject jspc compiler step easily. Copied over java and jsp files to `thebodgeitstore` project.  Added jspc plugin to pom.xml, when running codeql seeing it compile the JSP files (if i make a syntax error in a tag, i see compiler error)

```
cd bodgeitstore
codeql database create codeql_db --language=java --command="mvn clean package" --overwrite
```

yields:

```[2023-04-05 00:42:40] [build-stdout] [INFO] --- jspc:3.2.0:compile (jspc) @ thebodgeitstore ---
[2023-04-05 00:42:40] [build-stderr] Apr 05, 2023 12:42:40 AM org.apache.jasper.servlet.TldScanner scanJars
[2023-04-05 00:42:40] [build-stderr] INFO: At least one JAR was scanned for TLDs yet contained no TLDs. Enable debug logging for this logger for a complete list of JARs that were scanned but no TLDs were found in them. Skipping unneeded JARs during scanning can improve startup time and JSP compilation time.
[2023-04-05 00:42:40] [build-stdout] [INFO] Includes=**\/*.jsp, **\/*.jspx,  **\/*.jspf
[2023-04-05 00:42:40] [build-stdout] [INFO] Excludes=**\/.svn\/**
[2023-04-05 00:42:40] [build-stdout] [INFO] Number of jsps for thread 1 : 18
[2023-04-05 00:42:41] [build-stderr] Apr 05, 2023 12:42:41 AM org.apache.jasper.JspC execute
[2023-04-05 00:42:41] [build-stderr] INFO: Generation completed with [0] errors in [675] milliseconds
[2023-04-05 00:42:41] [build-stdout] [INFO] Number total of jsps : 18
[2023-04-05 00:42:41] [build-stdout] [INFO] Compilation completed in 0 min, 0 sec
```

CURRENT STATE: not seeing any JSP files in the CodeQL db :humph: