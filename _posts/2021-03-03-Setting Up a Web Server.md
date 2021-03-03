---
title: Setting Up a Web Server
author: lijiabao
date: 2020-12-06 12:43:11.461380700 +0800
category: Deployment
categories: Deployment
tags: Deployment
---

# Setting Up a Web Server

You might need to configure your web server to handle Java Network Launch Protocol (JNLP) files. If the web server is not set up properly, the Java Web Start application will not launch when you click on the link to the JNLP file.

Configure the web server so that files with the `.jnlp` extension are set to the `application/x-java-jnlp-file` MIME type.

The specific steps to set up the JNLP MIME type will vary depending on the web server. As an example, to configure an Apache web server, you should add the following line to the `mime.types` file.

```

application/x-java-jnlp-file JNLP

```

For other web servers, check the documentation for instructions on setting MIME types.
