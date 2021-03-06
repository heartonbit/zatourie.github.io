---
layout: post
title: Secure Coding 교육
excerpt: Secure Coding 교육 정리노트
date:   2015-11-24 09:00:00
categories:
tags: [securecoding]
author: mango
share: true
comments: true  
---

# What is vulnerability

> In computer security, a vulnerability is a weakness which allows an attacker to reduce a system's information assurance.

from https://en.wikipedia.org/wiki/Vulnerability_(computing)

In short, a weakness is in theory, a vulnerability is in practice.


# Refs

* 행자부 (Secure Coding Guide)
* CWE (Mitre's Common Weakness Enumeration) cf)CVE(Common Vulnerability Exposure)
* OWASP
* CERT (Secure Coding Standard)

# Minimum Requirement

* CWE SANS Top 25
* OWASP Top 10

# Mitigate Vulnerability

* Network Layer : Traffic, Packet monitoring
* App Op Layer : Request, pattern monitoring
* App Dev Layer : Secure Coding

# Cases

* Authentication flaw + non-authorized request exploitation
* Webshell[1] attack using Paros[2]
* Library vulneribility exploitation


----
# etc

Secure Coding 진단원 

OAuth 2.0 https://developers.daum.net/services/apis/docs/oauth2_0/reference

# Mitre corporation

* http://www.mitre.org/
* https://en.wikipedia.org/wiki/Mitre_Corporation
* http://www.oxforddictionaries.com/definition/english/mitre 

# Qualys

https://www.ssllabs.com/ssltest/analyze.html?d=skbiok.com

https://www.ssllabs.com/projects/documentation/index.html

# MS SDL (Security Development Lifecycle)

* https://www.microsoft.com/en-us/sdl/

![](http://i.msdn.microsoft.com/cc448177.SDL-Lifecycle-gradient_0609(en-us,MSDN.10).jpg)

![MS-SDL openeg](http://cfile22.uf.tistory.com/R750x0/227AA74355630C1A13AFA8)

* DREAD : http://www.openeg.co.kr/541

![](http://image.slidesharecdn.com/praetorianthreatmodelingpresentation-110117095334-phpapp02/95/threat-modeling-improve-security-drive-testing-reduce-costs-25-728.jpg?cb=1332160190) 

* STRIDE 

![](http://image.slidesharecdn.com/praetorianthreatmodelingpresentation-110117095334-phpapp02/95/threat-modeling-improve-security-drive-testing-reduce-costs-20-728.jpg?cb=1332160190)

![](https://i-technet.sec.s-msft.com/en-us/security/hh855044.STRIDE-definitions(en-us,MSDN.10).jpg)

![](https://i-technet.sec.s-msft.com/en-us/security/hh855044.STRIDE-mitigation-categories(en-us,MSDN.10).jpg)

# OWASP Webgoat Project

* WebGoat is a deliberately insecure web application maintained by OWASP designed to teach web application security lessons. You can install and practice with WebGoat in either J2EE (this page) or WebGoat for .Net in ASP.NET. In each lesson, users must demonstrate their understanding of a security issue by exploiting a real vulnerability in the WebGoat applications. For example, in one of the lessons the user must use SQL injection to steal fake credit card numbers. The application is a realistic teaching environment, providing users with hints and code to further explain the lesson.
* https://www.owasp.org/index.php/Category:OWASP_WebGoat_Project

# HSQLDB: http://hsqldb.org/

* HSQLDB (HyperSQL DataBase) is the leading SQL relational database software written in Java. It offers a small, fast multithreaded and transactional database engine with in-memory and disk-based tables and supports embedded and server modes. It includes a powerful command line SQL tool and simple GUI query tools.
* Benchmark: http://hsqldb.org/PolePosition.pdf

# web.xml 웹서버 설정 폴더 리스팅 / 파일쓰기 권한 등 

# 보안관련 Encoding

* URL Encoding : URL에 사용할 수 있는 형태로 encoding
* HTML Encoding : 브라우저 렌더링 escape

# MySQL SQL Injection

## SQL Injection Sample

    ' union select 1,2,3,4,5,6 #
    ' union select schema_name,2,3,4,5,6 from information_schema.schemata#
    ' union select table_name,2,3,4,5,6 from information_schema.tables where table_schema = database()#
    ' union select group_concat(column_name),2,3,4,5,6 from information_schema.columns where table_name = '테이블명'#

## Mitigate SQL Injection

* Use static query 
* Use PreparedStatement properly when using JDBC API
* Use # in My/IBatis query
* In case of dynamic query, use a proper filter to remove/replace special characters which enable attacks

# Access Control 경로조작 및 자원 삽입

## 주어진 권한을 넘어서는 리소스 탈취/수정

## Mitigate Access Control Attack

* Application에게 과도한 권한을 주지 말 것: root, administrator 사용금지
* Application내에서 whitelist, input string validation 등 수행


# Cross Site Script (XSS)

클라이언트 대상 해킹기법으로 악성코드를 클라이언트에서 실행시켜 원하는 데이터를 탈취함.

## Type (regarding where malicious script originated)

* Stored XSS
![](http://excess-xss.com/persistent-xss.png)

* Reflected XSS
![](http://excess-xss.com/reflected-xss.png)

* DOM XSS
![](http://excess-xss.com/dom-based-xss.png)

## Mitigate

* Using JSTL

```
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
....
<c:out value="${board.content}" />
....
````

or

````Java
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn" %>
....
<${fn:escapeXml(board.content)}
....
```

* Using Lucy XssPreventer/XssFilter : https://github.com/naver/lucy-xss-filter

```Java
String dirty = "\"><script>alert('xss');</script>";
String clean = XssPreventer.escape(dirty); //&quot;&gt;&lt;script&gt;alert(&#39xss&#39);&lt;/script&gt;

String dirty = "<img src=\"<img src=1\\ onerror=alert(1234)>\" onerror=\"alert('XSS')\">";
String clean = filter.doFilter(dirty); //<img src=\"\"><!-- Not Allowed Attribute Filtered ( onerror=alert(1234)) --><img src=1\\>\" onerror=\"alert('XSS')\"&gt;
```
Example above is too basic. Consult link below for smarter attacks 

OWASP Cheat Sheet : https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet

And here's a first hand experience of "Mass SQL Injection" http://using.tistory.com/11, Korean


# Secure Coding Conclusion : *DO NOT BELIEVE ANYONE*

----

# Footnotes

[1] Wehshell : 웹상에서 서버 CLI를 사용하도록 해주는 툴

http://darksoulstory.tistory.com/324 
https://www.trustwave.com/Resources/SpiderLabs-Blog/Hiding-Webshell-Backdoor-Code-in-Image-Files/

[2] Paros : http://www.parosproxy.org/

[3] Burp Suite : https://portswigger.net/burp/

[4] Pangolin : http://www.toolswatch.org/2011/04/pangolin-automated-sql-injection-test-tool-free-edition-v3-2-3-released/

[5] Cooxie Toolbar : http://download.cnet.com/Cooxie-Toolbar-for-Microsoft-Internet-Explorer/3000-2144_4-10268044.html

[6] Zap : https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project

[7] Apache Commons Collection Vulnerability http://www.kb.cert.org/vuls/id/576313

InvokeTransformer

[8] OWASP Enterprise Security API : https://www.owasp.org/index.php/Category:OWASP_Enterprise_Security_API 닷넷도 있음

[9] exploit-db.com : 공격코드
