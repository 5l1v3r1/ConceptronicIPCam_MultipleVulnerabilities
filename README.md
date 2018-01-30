  # Multiple vulnerabilities at Conceptronic IP Cameras with 0.61.X. web firmware.
A CSRF and Denial of Service vulnerabilities have been found at Conceptronic IP Cameras.

#### Index:
  0. Introduction
  1. Cross-Site Request Forgery on users.cgi
  2. Remote Unauthenticated Denial of Service
  3. Exploits
  4. Researcher

#### 0. Introduction

Conceptronic cameras are able to record sound and vídeo and stream it trhoug the network. Currently this kind of IP Cameras are used on domestic and proffesional environments, mostly for control and security reasons. Two vulnerabilities where found on two different software firmwares, and possibly they are present on every firmware currently on the market (not confirmed yet by the vendor). The first vulnerability affects the privacy and integrity of the sound and video and audio recorded by the camera, the second, affects de disponibility of the video and audio recorded by the camera, leading to physical security issues. An attacker could remotely take down this cameras just by knowing their IP.

Tested models:

| Camera Model| System firmware|Web firmware|
| -------------|-------------| -------------|
| CIPCAMPTIWL V3|00.30.01.0047P3|0.61.30.21|
|CIPCAM1080PTIWL|00.10.01.0039P2|-|


#### 1. Cross-Site Request Forgery on users.cgi
Every request is vulnerable to Cross-Site Request Forgery due to lack of CSRF token or any other CSRF protection. Specially sensitive GET request are:
``` 
/hy-cgi/user.cgi?cmd=edituser&at_username=admin&at_newpassword=new_password&at_newrolename=admin&at_userid=10001
``` 
Which allows to change administrator IP without providing the old password. An attacker can exploit this vulnerability by tricking the victim to visit a page which forces its browser to make a request to this URL.

```
/hy-cgi/user.cgi?cmd=adduser&at_username=admin2&at_password=admin2&at_rolename=admin
```

Allows to create a new administrator user.

Also, in special circustances, an attacker could exploit the vulnerability on 
```
/hy-cgi/user.cgi?cmd=checkuserinfo
```
to view victims current user password in plaintext.

| CVSS | Score | CVSS Details |
| -------------|-------------| -------------|
| 3|7.2|AV:N/AC:L/PR:N/UI:R/S:C/C:H/I:N/A:N/E:F/RL:U/RC:C|



#### 2. Unauthenticated Denial of Service.
A POST request with huge body to
```
/hy-cgi/device.cgi?cmd=searchlandevice
```
causes a crash on the IP Camera which completley freeze the system, making it unnaccesible.

In fact, any POST request is handled by an unknown middleware which is the crash responsible.


| CVSS | Score | CVSS Details |
| -------------|-------------| -------------|
| 3|8.6|AV:N/AC:L/PR:N/UI:N/S:C/C:N/I:H/A:N|

#### 3. Exploits.
Two exploits are provided in this repo for those vulnerabilities, just for testing purposes.

#### 4. Researcher.
Gonzalo Garcia Leon
