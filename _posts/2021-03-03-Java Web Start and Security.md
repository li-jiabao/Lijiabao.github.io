---
title: Java Web Start and Security
author: lijiabao
date: 2020-12-06 12:43:17.791677500 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Java Web Start and Security

This section describes the basics of security for applications deployed through Java Web Start and includes:

Applications launched with Java Web Start are, by default, run in a restricted environment, known as a **sandbox**. In this sandbox, Java Web Start:

- Protects users against malicious code that could affect local files
- Protects enterprises against code that could attempt to access or destroy data on networks

Sandbox applications that are launched by Java Web Start remain in this sandbox, meaning they cannot access local files or the network. See 
[Security in Rich Internet Applications ](../doingMoreWithRIA/security.html) for information.

<a name="https" id="https"></a>

## Dynamic Downloading of HTTPS Certificates

Java Web Start dynamically imports certificates as browsers typically do. To do this, Java Web Start sets its own `https` handler, using the `java.protocol.handler.pkgs` system properties, to initialize defaults for the 
[`SSLSocketFactory`](https://docs.oracle.com/javase/8/docs/api/javax/net/ssl/SSLSocketFactory.html) and 
[`HostnameVerifier`](https://docs.oracle.com/javase/8/docs/api/javax/net/ssl/HostnameVerifier.html). It sets the defaults with the methods 
[`HttpsURLConnection.setDefaultSSLSocketFactory`](https://docs.oracle.com/javase/8/docs/api/javax/net/ssl/HttpsURLConnection.html#setDefaultSSLSocketFactory-javax.net.ssl.SSLSocketFactory-) and 
[`HttpsURLConnection.setDefaultHostnameVerifier`](https://docs.oracle.com/javase/8/docs/api/javax/net/ssl/HttpsURLConnection.html#setDefaultHostnameVerifier-javax.net.ssl.HostnameVerifier-).

If your application uses these two methods, ensure that they are invoked after the Java Web Start initializes the `https` handler, otherwise your custom handler will be replaced by the Java Web Start default handler.

You can ensure that your own customized `SSLSocketFactory` and `HostnameVerifiter` are used by doing one of the following:

- Install your own `https` handler, to replace the Java Web Start `https` handler.
- In your application, invoke `HttpsURLConnection.setDefaultSSLSocketFactory` or `HttpsURLConnection.setDefaultHostnameVerifier` only after the first `https URL` object is created, which executes the Java Web Start `https` handler initialization code first.
