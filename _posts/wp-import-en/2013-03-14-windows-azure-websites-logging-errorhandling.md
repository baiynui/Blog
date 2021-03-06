---
layout: post
title: "Windows Azure Websites – Logging & ErrorHandling"
date: 2013-03-14 16:40
author: CI Team
comments: true
categories: [Uncategorized]
tags: []
language: en
---
{% include JB/setup %}
The <a href="{{BASE_PATH}}/2013/03/02/windows-azure-websites-git-hosting-deployment-leicht-gemacht/">Azure Websites</a> are easy to handle but still it doesn’t take much effort to add new instances. But how should I react if an error appears?

<img style="background-image: none; padding-left: 0px; padding-right: 0px; padding-top: 0px; border: 0px;" title="image" src="{{BASE_PATH}}/assets/wp-images-de/image_thumb937.png" border="0" alt="image" width="575" height="239" />

<strong>Azure Website Configuration </strong>



At the adjustments of Azure Websites you will find three diagnostic-tools:

· <strong>Detailed Error Logging</strong> – Turn on detailed error logging to capture all errors generated by your web site.

· <strong>Failed Request Tracing</strong> – Turn on failed request tracing to capture information for failed client requests.

· <strong>Web Server Logging</strong> – Turn on Web Server logging to save web site logs using the W3C extended log file format.

<a href="http://www.windowsazure.com/en-us/manage/services/web-sites/how-to-monitor-websites/">Source: How to monitor web sites</a>

<img style="background-image: none; padding-left: 0px; padding-right: 0px; padding-top: 0px; border: 0px;" title="image" src="{{BASE_PATH}}/assets/wp-images-de/image_thumb938.png" border="0" alt="image" width="578" height="390" />

<strong>Where are these Logs saved?</strong>



The logs are saved in a directory on the machine which is accessible via FTP. Therefore you have to change into the “Dashboard” and search for “FTP DIAGNOSTIC LOGS”. You will be asked for the Deployment-User (which is also responsible for the Git-Deployment) as Login!

&nbsp;

<img style="background-image: none; padding-left: 0px; padding-right: 0px; padding-top: 0px; border: 0px;" title="image" src="{{BASE_PATH}}/assets/wp-images-de/image_thumb939.png" border="0" alt="image" width="398" height="164" />

<img style="background-image: none; padding-left: 0px; padding-right: 0px; padding-top: 0px; border: 0px;" title="image" src="{{BASE_PATH}}/assets/wp-images-de/image_thumb940.png" border="0" alt="image" width="395" height="333" />



<strong>I require more Logging- / Exception information’s </strong>

<strong></strong>

Unfortunately the Error-Pages are not always constructive therefore you will quickly reach the point where you need the Logging framework.

ELMAH to the rescue!

I quickly decided for ELMAH including the additional <a href="https://github.com/alexanderbeletsky/elmah.mvc">ASP.NET MVC Integration</a>:

<img style="background-image: none; padding-left: 0px; padding-right: 0px; padding-top: 0px; border: 0px;" title="image" src="{{BASE_PATH}}/assets/wp-images-de/image_thumb941.png" border="0" alt="image" width="458" height="313" />

Now integrate the XmlFile-Provider into the web.config:
<pre class="csharpcode">&lt;elmah&gt;
  &lt;errorLog type=”Elmah.XmlFileErrorLog, Elmah” logPath=”~/App_Data” /&gt;
&lt;/elmah&gt;</pre>
<!-- .csharpcode, .csharpcode pre { 	font-size: small; 	color: black; 	font-family: consolas, "Courier New", courier, monospace; 	background-color: #ffffff; 	/*white-space: pre;*/ } .csharpcode pre { margin: 0em; } .csharpcode .rem { color: #008000; } .csharpcode .kwrd { color: #0000ff; } .csharpcode .str { color: #006080; } .csharpcode .op { color: #0000c0; } .csharpcode .preproc { color: #cc6633; } .csharpcode .asp { background-color: #ffff00; } .csharpcode .html { color: #800000; } .csharpcode .attr { color: #ff0000; } .csharpcode .alt  { 	background-color: #f4f4f4; 	width: 100%; 	margin: 0em; } .csharpcode .lnum { color: #606060; } -->Now you are able to navigate to the page and the App_Data directory via FTP:

<img style="background-image: none; padding-left: 0px; padding-right: 0px; padding-top: 0px; border: 0px;" title="image" src="{{BASE_PATH}}/assets/wp-images-de/image_thumb942.png" border="0" alt="image" width="457" height="146" />

Of course Log4Net or NLog would work as well.

<strong></strong>

<strong>Result:</strong>

The Azure websites act basically like a normal web application on an ordinary IIS and therefore they have almost the same rules. You don’t need a totally different repertoire like with the Cloud Services (with the Table Storage and the Diagnostics).
