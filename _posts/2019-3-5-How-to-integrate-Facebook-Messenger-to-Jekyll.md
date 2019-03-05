---
title: How To Add Facebook Messenger to Jekyll
layout: post
date: 2019-03-05 10:30:31
tags: [Plugins,Facebook,jekyll]
music-id: 35201383
---

# Introduce
Facebook messenger is very awesome for us to contact live,just  like wechat,and Facebook launched the Messenger customer chat plugin in 2017  
With the Messenger ,it looks like a lightweight CRM,you could make a greater businesses with your customers,so many websites(wordpress ,shoppify ,etc) have this plugin.
Now I want to show you how to integrate Facebook Messenger as a live chat to Jekyll
[Toryun.com's Messenger Plugin](/images/Plugin/Messenger.jpg)
## Bot Support
Messenger has a feature which is bot suppoort ,I will develop this one in next step and introduce it  

# How to set it up  
## Prerequisites
1. Most obviously, you need to create a Facebook Page for your website or business. Click Here to view instructions on Facebook.com.  
2. You’ll need you Facebook Page ID. Follow the instructions below to retrieve that.
3. You’ll need to whitelist your domain name on your facebook page. We’ll show you how in the guide below.
4. You’ll need to embed a snippet of code on the pages where you wish to display your widget.  
5. You should consider setting up notifications on your mobile & in email for when customers message you, as well as downloading the Messenger App for your phone.  
Detailed documentation & setup instructions on [Facebook](https://developers.facebook.com/docs/messenger-platform/discovery/customer-chat-plugin)

## 1. Retrieve your Facebook Page ID  
First, you’ll need to retrieve your *Facebook page ID*. You can do this by navigating to your page, clicking on the *About tab*, and scrolling to the bottom of the page. There, you will see your *page ID (It should be a number).
[Page ID](/images/Plugin/page_id.jpg)

## 2. Whitelist your Domain
Next, you’ll need to navigate to the “MESSENGER PLATFORM” tab and whitelist your domain. Type in both the HTTP:// and HTTPS:// variants of your domain to ensure it works for 100% of users. Click the SAVE button.
[whitelist](/images/Plugin/whitelist.jpg)

3. Embed a snippet of code in your website
Now, our final step, is to embed a small snippet of code on our website/jekyll. You should place it just before the closing **</body>** tag of your webpage, or use a tag manager, like Google Tag Manager.

```javascript

<!-- Load Facebook SDK for JavaScript -->
<div id="fb-root"></div>
<script>
  window.fbAsyncInit = function() {
    FB.init({
      xfbml            : true,
      version          : 'v3.2'
    });
  };

  (function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = 'https://connect.facebook.net/en_US/sdk/xfbml.customerchat.js';
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>

<!-- Your customer chat code -->
<div class="fb-customerchat"
  attribution=setup_tool
  page_id="1964757066968313">
</div>

```

You could change the `page_id` to yours  
Put the script code to `_includes/_partials/head.html`
then put the `<!-- Your customer chat code --><div>...</div>` to `_includes/_macro/post.html`


## Additional Setting: Messenger widget open by default
Facebook also gives you the ability to open your chat widget by default on page load. It looks like this:  
[Widget](/images/Plugin/widget.jpg)  
Just change **minimized=”true”** to **minimized=”false”** in your code snippet.

```html

<div class="fb-customerchat"
 page_id="<ENTER-YOUR-FACEBOOK-ID-HERE>"
 minimized="false">
</div>

```	

## Aligning the widget left instead of right
You can add this snippet of code, along with your other snippet of code, to align the widget left instead of right:

```javascript

<style>
  #fb-root > div.fb_dialog.fb_dialog_advanced.fb_shrink_active {
     right: initial !important;
     left: 18pt;
     z-index: 9999999 !important;
  }
  .fb-customerchat.fb_invisible_flow.fb_iframe_widget iframe {
     right: initial !important;
     left: 18pt !important;
  }
</style>

```  
[widget_left](/images/Plugin/widget_left.jpg)

Ok,That'all

