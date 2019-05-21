---
title: How to Build a Facebook Messenger Bot in Glitch
date: 2019-03-07 17:59:31
tags: [YouTube,API]
music-id: 521492516

---  
> source from http://baiaman.com/facebook-chatbot-tutorial
> https://developers.facebook.com/docs/messenger-platform/getting-started/quick-start?locale=zh_TW
![Messenger Bot](/images/Plugin/Messengerbot/messenger_bot.gif)

## Introduce
The [Glitch](https://glitch.com) is free register and build a project website.Provide a free public service and many interested projects,you could test your project online,let's becutiful feature I love it so much.

## This short Facebook chatbot tutorial 

will help you to create your own messenger chatbot in just 9 minutes. Your chatbot will be hosted in Glitch (free) and we will use Facebook’s Messenger Platform quick start page as a template.

### Step 1. Go to Glitch and click on “Remix This”  
Go here and click on the “Remix This” button to create a new Glitch project for you.
![loading...](https://i0.wp.com/i.imgur.com/blSv6gB.gif?zoom=2&resize=1000%2C476 "remix this")

### Step 2. Register as a Facebook Developer  
Click here to open the Facebook for developers homepage, and follow their instructions to register your developer account.
![loading...](https://i2.wp.com/i.imgur.com/GYfGPD2.gif?zoom=2&resize=700%2C361 "register fb dev")

### Step 3. Create a New Facebook App  
Once you register your developer account, you should be able to create an app.
![loading...](https://i1.wp.com/i.imgur.com/s9FTwd7.gif?zoom=2&w=1000 "Create app")

### Step 4. Add Messenger to your Facebook App
Inside of your Facebook app dashboard click on “+ Add Product” button and add Messenger, and link it to your Facebook page (click here to create a new page if you don’t have an existing page).
![loading...](https://i1.wp.com/i.imgur.com/xH5ZORP.gif?zoom=2&w=1000 "Add messenger")

### Step 5. Generate Your Page Access Token
We will use it in the next step.
![loading...](https://i1.wp.com/i.imgur.com/pOv0LjB.gif?zoom=2&w=1000 "Generate page_access_token")

### Step 6. Add Your Page Access Token to your Glitch script
Copy your “Page Access Token” from your facebook app and paste it into the .env file in your Glitch project that we created in Step 1.
![loading...](https://i1.wp.com/i.imgur.com/YvXn47L.gif?zoom=2&w=1000 "add token to glitch")

### Step 7. Make your Glitch Project Private
By default your Glitch project will be visible to everyone. To make it private, click on the “Make Private” button from the “Advanced Options”.
 ![loading...](https://i2.wp.com/i.imgur.com/0ZcxjfK.gif?w=1000 "make private")
 
### Step 8. Verify and Save your Webhooks
Get your URL from your Clitch dashboard, add /webhook in the end, and paste it in `Callback URL` input field. For the `Verify Token` input field, you can use any token (just make sure the New Page Subscription Verify Token matches Glitch VERIFY_TOKEN).
![loading...](https://i1.wp.com/i.imgur.com/2SqBLaX.gif?zoom=2&w=1000 "verify webhook")

### Step 9. Subscribe your App to your Page
![loading...](https://i0.wp.com/i.imgur.com/nd7bMyi.gif?zoom=2&w=1000 "subscribe")

### Step 10. Connect Your Chatbot to a Google Sheet
Copy this Google Sheet and add your Google Sheet ID to your env. file in your Glitch project.
But After I check ,I found it can't generate anything ,because it need your google sheet api and credientals
But his code shows nothing about the feature of googlesheet api
![loading...](https://i1.wp.com/i.imgur.com/SXLoxou.gif?zoom=2&w=1000 "Connect to googlesheet")

### Step 11. Test your Facebook ChatBot
Go to your Facebook page and chat with your bot. To customize your chatbot’s replies edit your Google Sheet from the Step 10.
![loading...](https://i2.wp.com/i.imgur.com/t41vM2q.gif?zoom=2&resize=700%2C391 "test")

If you have any questions related to this post, let me know in the comments below.And you could also do a simple messenger bot by Facebook offical developers's tutorial.

The next step is developing a messenger bot with NLP



