---
layout: post
title: "Avoid „Duplicate Content“ with IIS7 – Domain with or without www"
date: 2011-12-02 21:02
author: CI Team
comments: true
categories: [Uncategorized]
tags: [Domain; IIS7]
language: en
---
{% include JB/setup %}
<p>If you are the owner of a domain like “foobar.de” you need to make a decision: with or without www? </p>
<p><b>With or without www?</b></p>
<p>It’s not so easy to find an answer to this question. I prefer the www option because it’s possible to set a <a href="{{BASE_PATH}}/2010/12/16/howto-eigene-domains-auf-windows-azure-applikationen-mappen-cloudapp-net/">C-Name</a> for a “www” Subdomain. The C-Name is important if you want to run your own Web application on you own Domain on Windows Azure. </p>
<p>I found <a href="http://www.sitepoint.com/www-or-no-www/">two</a> <a href="http://www.codinghorror.com/blog/2008/04/the-great-dub-dub-dub-debate.html">more</a> articles about this subject but they are a little bit older. </p>
<p><b>Important: you need to decide! </b></p>  

<p>If the website is addressable with and without www then Google will evaluate the <a href="http://www.google.com/support/webmasters/bin/answer.py?answer=66359">Content as “double”</a> – the result will be negative for the <s>search engine</s> Google optimization. </p>
<p><b>What’s the easiest way to reach this wit IIS</b></p>
<p><a href="{{BASE_PATH}}/assets/wp-images-en/image1394.png"><img style="background-image: none; border-bottom: 0px; border-left: 0px; margin: 0px 10px 0px 0px; padding-left: 0px; padding-right: 0px; display: inline; float: left; border-top: 0px; border-right: 0px; padding-top: 0px" title="image1394" border="0" alt="image1394" align="left" src="{{BASE_PATH}}/assets/wp-images-en/image1394_thumb.png" width="158" height="240" /></a>Our blog for example is meant to be addressable with the non-www option so I’ve created the “Code Inside” side. Code-inside.de is the only address the side will react on. </p>
<p>The other side <a href="http://www.code-inside.de">www.code-inside.de</a> will only react on the www-option. </p>  
  
  

<p><b>Redirect in IIS</b></p>
<p>There is a very simple way to create a redirect in IIS:</p>
<p><img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; border-top: 0px; border-right: 0px; padding-top: 0px" title="image" border="0" alt="image" src="{{BASE_PATH}}/assets/wp-images-de/image_thumb577.png" width="430" height="376" /></p>
<p>Enter the destination address and set the state on “Permanent (301)”.</p>
<p><img style="background-image: none; border-bottom: 0px; border-left: 0px; padding-left: 0px; padding-right: 0px; border-top: 0px; border-right: 0px; padding-top: 0px" title="image" border="0" alt="image" src="{{BASE_PATH}}/assets/wp-images-de/image_thumb578.png" width="244" height="197" /></p>
<p>This way will work but maybe you will have many “Dummy” sides in IIS only for the Redirects. But at least it’s very easy and it works – for me <img style="border-bottom-style: none; border-left-style: none; border-top-style: none; border-right-style: none" class="wlEmoticon wlEmoticon-winkingsmile" alt="Zwinkerndes Smiley" src="{{BASE_PATH}}/assets/wp-images-en/wlEmoticon-winkingsmile29.png" /></p>
<p><b>Other options: with web.config</b></p>  

<p>Of course there are some other possibilities like for example there are some interesting ways in IIS with <a href="http://learn.iis.net/page.aspx/460/using-the-url-rewrite-module/">Rewrite Rules</a>. To say the true: I didn’t work with this so far. Maybe someone knows the suitable Snippet for the transmission from www to non-www? <img style="border-bottom-style: none; border-left-style: none; border-top-style: none; border-right-style: none" class="wlEmoticon wlEmoticon-openmouthedsmile" alt="Smiley mit geöffnetem Mund" src="{{BASE_PATH}}/assets/wp-images-en/wlEmoticon-openmouthedsmile2.png" /></p>
