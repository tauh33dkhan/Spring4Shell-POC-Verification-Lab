# Spring4Shell-POC-Verification-Lab

This lab is created to test poc exploits on vulnerable, partially vulnerable and fixed version of spring boot deployment.

## Lab 1: Vulnerable deployment

```js
Tomcat: 8.5.73
Spring boot: 2.6.3
JDK: 11
```
<a href="https://github.com/tauh33dkhan/Spring4Shell-POC-Verification-Lab/tree/main/tomcat_8.5.73-jdk11-spb.2.6.3">Read how to deploy lab & test results</a>

## Lab 2: Partially vulnerable deployment

Here I used vulnerable spring boot and JDK version but fixed tomcat version

```js
Tomcat: 8.5.78
Spring boot: 2.6.3
JDK: 11
```
<a href="https://github.com/tauh33dkhan/Spring4Shell-POC-Verification-Lab/blob/main/tomcat_8.5.78-jdk11-spb-2.6.3">Read how to deploy lab & test results</a>

## Lab 3: Fixed Deployment

Here I used fixed spring boot, tomcat and not vulnerable JDK version 

```js
Tomcat: 8.5.78
Spring boot: 2.6.6
JDK: 8
```
<a href="https://github.com/tauh33dkhan/Spring4Shell-POC-Verification-Lab/tree/main/tomcat_8.5.78-jdk8-spb-2.6.6">Read how to deploy lab & test results</a>

### Working POC

After testing many poc on lab I found this poc is properly able to detect the vulnerable deployment which gives 

- `400` error response on vulnerable lab.
- `500` error response on fixed tomcat but vulnerable spring boot lab and 
- `200 OK` response on fixed tomcat as well as spring boot lab.

source: https://twitter.com/hiaray115/status/1512147033309786119

```
host:port/path?class.module.classLoader.resources.baseUrls%5B0%5D=0
```



## Credits

All labs use POC application shared by <a href="https://github.com/reznok/Spring4Shell-POC">@reznok</a>
