---
title: How to Build a Facebook Messenger Bot in Glitch
date: 2018-11-23 17:59:31
tags: [YouTube,API]
music-id: 521492516
---  
> source from http://baiaman.com/facebook-chatbot-tutorial
![Messenger Bot](/images/Plugin/Messengerbot/messenger_bot.gif)
## Introduce
The [Glitch](https://glitch.com)


## This short Facebook chatbot tutorial 

will help you to create your own messenger chatbot in just 9 minutes. Your chatbot will be hosted in Glitch (free) and we will use Facebook’s Messenger Platform quick start page as a template.

### Step 1. Go to Glitch and click on “Remix This”  
Go here and click on the “Remix This” button to create a new Glitch project.
###  

### Step 2. Register as a Facebook Developer  
Click here to open the Facebook for developers homepage, and follow their instructions to register your developer account.
 
###Step 3. Create a New Facebook App  
Once you register your developer account, you should be able to create an app.


### Step 4. Add Messenger to your Facebook App
Inside of your Facebook app dashboard click on “+ Add Product” button and add Messenger, and link it to your Facebook page (click here to create a new page if you don’t have an existing page).

### Step 5. Generate Your Page Access Token
We will use it in the next step.

### Step 6. Add Your Page Access Token to your Glitch script
Copy your “Page Access Token” from your facebook app and paste it into the .env file in your Glitch project that we created in Step 1.

### Step 7. Make your Glitch Project Private
By default your Glitch project will be visible to everyone. To make it private, click on the “Make Private” button from the “Advanced Options”.

### Step 8. Verify and Save your Webhooks
Get your URL from your Clitch dashboard, add /webhook in the end, and paste it in `Callback URL` input field. For the `Verify Token` input field, you can use any token (just make sure the New Page Subscription Verify Token matches Glitch VERIFY_TOKEN).

### Step 9. Subscribe your App to your Page
### Step 10. Connect Your Chatbot to a Google Sheet
Copy this Google Sheet and add your Google Sheet ID to your env. file in your Glitch project.
But After I check ,I found it can't generate anything ,because it need your google sheet api and credientals
### Step 11. Test your Facebook ChatBot
Go to your Facebook page and chat with your bot. To customize your chatbot’s replies edit your Google Sheet from the Step 10.


If you have any questions related to this post, let me know in the comments below.


