---
layout: concept
title: Train your bot
permalink: /concepts/train-your-bot
---

## Create an intent

Everything your chatbot understands is in the intents. Each intent corresponds to an action that your user wants to perform. For example, the intent **greetings** enables your bot to understand when a user says \`Hello\`.

Explore each intent by clicking the name of the intent (for example, **greetings**) and you'll see the expressions inside that train your bot to understand the user's intent.

Let's add a new capability to our chatbot to book a meeting room. Add a new intent called **booking** to understand when users ask your chatbot to book a room. In the **SEARCH** field, type \`booking\` and click **SEARCH** to search and fork this intent from the community. Since the platform is collaborative, many intents have already been created. Select one of the first results and click **FORK**.

![SAP Conversational AI fork intents](https://cdn.cai.tools.sap/man/introduction/search-booking.png)

## Add expressions

Click the intent **booking** that you've just added to your bot. The optimal setting for an intent is to contain around twenty paraphrased expressions. Identify some expressions that your users are likely to say and add each expression to the intent by by entering it in the **Add an expression** field and pressing **Enter**.

![SAP Conversational AI expressions](https://cdn.cai.tools.sap/man/introduction/booking-intent.png)

## Use entities

Go to the intent **booking**. If you click one of the expressions, you'll see highlighted words with tags. These are entities. Entities are keywords detected in expressions that are important to you in order to automate a task.

![SAP Conversational AI intents](https://cdn.cai.tools.sap/man/recast-ai-entitiesb.png)

We automatically detect <a href="https://cai.tools.sap/docs/concepts/gold-entities" target="_blank" >28 different entities</a> such as *Datetime*, *Location*, *Person*, and so on.
We call them gold entities. If you need another entity – for example, a custom entity like a meal for a cooking bot – just select what you want to tag as your new entity and type a name. The more examples you provide, the better the detection will be.

![SAP Conversational AI tag entities](https://cdn.cai.tools.sap/man/recast-ai-tag-entitiesb.png)

## Training mode

Unless your bot is a big bot, the default setting for the training mode is **Automatic**. This means that training is automatically triggered by any change to the bot. If you wish, you can change the training mode to **Manual** in your bot settings. This lets you decide for yourself when you want to update the bot's training.

For big bots (that is, bots with more than 10,000 expressions or more than 15 custom entities), the training mode is always set to **Manual**. It cannot be changed to **Automatic**.

## Test with the NLP console

Once you’ve created new intents, you can test them with the console. To display the console, click **TEST** at the top right of the page. To test if your bot is well-trained, try typing \`I want meeting room 2 for tomorrow\`.

![SAP Conversational AI NLP analyze](https://cdn.cai.tools.sap/man/introduction/console-view.png)

You can see which intent is detected and which entities are extracted. To switch the view to the JSON mode, click the **Smart view** toggle.
The JSON contains a lot of useful information about the message you’ve sent, such as all the enrichments we can provide for the gold entities.

![SAP Conversational AI NLP JSON](https://cdn.cai.tools.sap/man/introduction/console-json.png)

## Train your bot

If your message doesn’t match an intent, you need to train your bot. On the **MONITOR** tab, click the **Log Feed** option to show the logs for your bot. Select the expressions that didn't match an intent and redirect them to the correct intent. Then check that your custom entities have been automatically tagged. If not, tag them and remember *Bot trained, mommy approved!*

![SAP Conversational AI bot monitor](https://cdn.cai.tools.sap/man/introduction/monitor-log-feed.png)
