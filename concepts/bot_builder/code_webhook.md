---
layout: concept
title: Custom code and webhooks
permalink: /concepts/code-and-webhook
---


At many points in your conversation, you most likely want to retrieve business information or connect to an external system to perform actions. You can do this through webhooks.
A webhook is a simple HTTP call to your backend. To configure your HTTP call, click **CALL WEBHOOK** in the Bot Builder.

![SAP Conversational AI - Webhook](//cdn.cai.tools.sap/man/webhook/webhook.png)

You can provide the full URL or route (starting with a '/') to be called by the Bot Builder. 
If you provide a route, the **Bot webhook base URL** (configurable in your bot's settings) will be prepended to it.

You can specify the HTTP method to use in your webhook call (GET, POST, PUT, or PATCH).

![SAP Conversational AI - Webhook](//cdn.cai.tools.sap/man/webhook/header.png)

## Authentication configuration

You have the following options:

•	No authentication: No authentication/authorization is passed with the request.

•	Basic authentication: A username/password pair is passed with the request. 

•	OAuth 2 authentication: A client ID, client secret, and authorization URL is passed with the request.

![SAP Conversational AI - Webhook](//cdn.cai.tools.sap/man/webhook/authentication.png)

## Header configuration

HTTP headers are accommodated by configuring a key-value pair, where you can name keys and set a value to be passed along in the header.

![SAP Conversational AI - Webhook](//cdn.cai.tools.sap/man/webhook/webhook-header.png)

## Body configuration

The HTTP request body must be formatted as a standard JSON object. You can either receive the default body that we provide with all conversation states or create your own custom body.

The default body format is as follows:

~~~ json
{
  "conversation": {
    "id": "A_CONVERSATION_ID",
    "language": "en",
    "memory": {
      "person": {
        "fullname": "Francois",
        "raw": "Francois",
        "confidence": 0.95
      }
    },
    "skill_stack": ["get-weather"],
    "skill": "small-talk",
    "skill_occurences": 1
  },
  "nlp": {
    "source": "hi",
    "intents": [
      {
        "slug": "greetings",
        "confidence": 0.99
      }
    ],
    "sentiment": "vpositive",
    "entities": {},
    "act": "assert",
    "type": null,
    "version": "2.10.1",
    "processing_language": "en",
    "language": "en",
    "uuid": "96597974-3ee1-4743-8a5d-341b67effb9a"
    "status": 200,
    "timestamp": "2017-10-25T21:36:02.071243+00:00",
  }
}
~~~

<br><br>
In custom HTTP request bodies, you can reference conversation variables (like memory variables, NLP information, etc.) in place of hard-coded values, for example, `{{memory.person.raw}}`. Check all the [variables](https://cai.tools.sap/docs/concepts/builder_messages#variables) you can use.

![SAP Conversational AI - Webhook](//cdn.cai.tools.sap/man/webhook/body.png)


## Templates

You can reuse specific configurations of authorizations, headers, and bodies in other skills by choosing **Templates** in the skill view.

![SAP Conversational AI - Webhook](//cdn.cai.tools.sap/man/webhook/templates-skill.png)

 You can manage these templates on the **Templates** tab.
 
![SAP Conversational AI - Webhook](//cdn.cai.tools.sap/man/webhook/templates-builder.png)

## Formatting the response of the webhook call

The body format of your response should be a valid JSON and can contain two keys: `replies` and `conversation`.
<br><br>

| Key                   | Required | Value
|-----------------------|----------|-------------------------------------------|
| replies               | Optional | Array of object                           |
| conversation          | Optional | Object with a key `memory` and `language` |
| conversation.memory   | Optional | Object filled as you want                 |
| conversation.language | Optional | String with a language ISO format         |

<br>
Here is an example:

~~~ json
{
  "replies": [
    {
      "type": "text",
      "content": "Hello world!"
    }
  ],
  "conversation": {
    "language": "en",
    "memory": {
      "user": "Bob"
    }
  }
}
~~~

<br>
The `conversation` data you send back will update the state of your conversation:

* `memory` will replace the actual memory of your bot (so be careful that you don't lose everything if you just want to change one of your memory keys to add all your other keys).
* `language` will update the language of the conversation. Each new sentence sent by the user will be processed in this language, and the bot will reply in this language.

The `replies` are sent in the body of the result of the main Bot Builder and will appear in the `messages` key:

POST `https://api.cai.tools.sap/build/v1/dialog`
<br>

~~~ json
{
    "messages": [
        {
             "type": "text",
             "content": "Hello world!"
        }
    ],
    "conversation": {
        "id": "CONVERSATION_ID",
        "language": "en",
        "memory": {},
        "skill": "default",
        "skill_occurences": 1
    },
    "nlp": {
        "uuid": "b96bc782-6aba-4fac-aeaa-2326936b08bf",
        "source": "Hello SAP Conversational AI",
        "intents": [
            {
                "slug": "greetings",
                "confidence": 0.99
            }
        ],
        "act": "assert",
        "type": null,
        "sentiment": "neutral",
        "entities": {},
        "language": "en",
        "processing_language": "en",
        "version": "2.10.1",
        "timestamp": "2017-10-19T13:24:12.984856+00:00",
        "status": 200
    }
}
~~~


## Formatting the array of replies

You can format objects in the array of reply as desired, depending on your needs when you request the Bot Builder API.
If you are using the Bot Connector (that is, you have connected a channel on the SAP Conversational AI platform like Facebook Messenger, Slack, or a webchat), you need to follow the Bot Connector format:  **<a href="https://cai.tools.sap/docs/concepts/structured-messages">Check the format to send rich messages</a>**

~~~ json
{
  "replies": [
    {
      "type": "text",
      "content": "Hello world!"
    }
  ]
}
~~~


## Running your basic API

Copy and paste this snippet in a file, install the dependencies, and run the file.

### JavaScript (JS)

We recommend using version 6.1.0 of Node.js.

1. Create a directory `mkdir my-bot` and `cd my-bot && npm init`  

2. Create your main file `touch index.js`  

3. Install the dependencies `npm install express body-parser --save`  

4. Open your `index.js` and copy and paste the code snippet.

5. Run your code `node index.js`

### Ruby

We recommend using version 2.3+ of Ruby.

1. Create a directory `mkdir my-bot` and `cd my-bot && touch bot.rb`  

2. Install the dependencies `gem install sinatra`  

3. Open your `bot.rb` and copy and paste the code snippet.

4. Run your code `ruby bot.rb`  

### Python

We recommend using version 2.7+ of Python.

1. Create a directory `mkdir my-bot` and `cd my-bot && touch bot.py`  

2. Install the dependencies `pip install flask`  

3. Open your `bot.py` and copy and paste the code snippet.  

4. Run your code `python bot.py`  

### PHP

We recommend using version 7.x of PHP.

1. Create a directory `my-bot` and `cd my-bot && touch index.php`  

2. Install the dependencies `composer require slim/slim`  

3. Copy and paste the code snippet in the `index.php`  

4. Run your server with `php -S localhost:5000 index.php`  

<br/>

<script>
  function activateWebhookSnippet(language) {
    document.querySelectorAll(".snippet-tab")
      .forEach(elem => elem.classList.remove("snippet-tab-active"));
    document.getElementById(`webhook-snippet-${language}`).classList.add("snippet-tab-active")

    for (const lng of ["javascript", "ruby", "python", "php"]) {
      if (lng === language) {
        document.querySelector(`.language-${language}`).classList.add("snippet-active")
      } else {
        document.querySelector(`.language-${lng}`).classList.remove("snippet-active")
      }
    }
  };

  document.addEventListener("DOMContentLoaded", () => {
    activateWebhookSnippet("javascript")
  });
</script>

<style>
  .language-javascript,
  .language-python,
  .language-ruby,
  .language-php {
    display: none;
  }

  .highlight {
    padding: 0.5rem;
  }

  .snippet-tab {
    padding: 0.2rem 0.5rem;
    border-top-left-radius: 3px;
    border-top-right-radius: 3px;
    background-color: grey;
    color: white;
    cursor: pointer;
  }

  .snippet-tab-active {
    background-color: #002b36;
  }

  .snippet-active {
    display: block !important;
  }

  .code {
    background-color: lightgrey;
    padding: 0.2rem;
    border-radius: 3px;
  }
</style>

<div id="webhook-snippet-container">
  <div class="snippet-tabs">
    <button onclick="activateWebhookSnippet('javascript')" id="webhook-snippet-javascript" class="snippet-tab">JS</button>
    <button onclick="activateWebhookSnippet('ruby')" id="webhook-snippet-ruby" class="snippet-tab">Ruby</button>
    <button onclick="activateWebhookSnippet('python')" id="webhook-snippet-python" class="snippet-tab">Python</button>
    <button onclick="activateWebhookSnippet('php')" id="webhook-snippet-php" class="snippet-tab">PHP</button>
  </div>
</div>

~~~ javascript
const express = require('express')
const bodyParser = require('body-parser')

const app = express()
const port = 5000
app.use(bodyParser.json())

app.post('/', (req, res) => {
  console.log(req.body)

  res.send({
    replies: [{
      type: 'text',
      content: 'Roger that',
    }],
    conversation: {
      memory: { key: 'value' }
    }
  })
})

app.post('/errors', (req, res) => {
  console.log(req.body)
  res.send()
})

app.listen(port, () => {
  console.log('Server is running on port 5000')
})
~~~

~~~ ruby
require 'sinatra'
require 'json'

set :port, 5000

before do
  @params = JSON.parse(request.body.read)
end

post '/' do
  content_type :json
  {
    replies: [{ type: 'text', content: 'Roger that' }],
    conversation: {
      memory: {
        key: 'value'
      }
    }
  }.to_json
end

post '/errors' do
  puts @params

  200
end
~~~

~~~ python
from flask import Flask, request, jsonify
import json

app = Flask(__name__)
port = '5000'

@app.route('/', methods=['POST'])
def index():
  print(json.loads(request.get_data()))
  return jsonify(
    status=200,
    replies=[{
      'type': 'text',
      'content': 'Roger that',
    }],
    conversation={
      'memory': { 'key': 'value' }
    }
  )

@app.route('/errors', methods=['POST'])
def errors():
  print(json.loads(request.get_data()))
  return jsonify(status=200)

app.run(port=port)
~~~

~~~ php
<?php
require 'vendor/autoload.php';
use \Slim\App;

$app = new App();

$app->post('/', function ($request, $response) {
  error_log($request->getBody()->getContents());
  return $response->withJson(array(
    'replies' => [
      array('type' => 'text', 'content' => 'Roger that')
    ],
    'conversation' => array(
      'memory' => array('key' => 'value')
    )
  ));
});

$app->post('/errors', function ($request, $response) {
  error_log($request->getBody()->getContents());
  return $response;
});

$app->run();
~~~

## Exposing the opened port to connect it to a webhook

If you don’t have a public server, or if you want to test your webhook during development, ngrok is a very handy tool. It creates a public URL for you and forwards requests to your computer.

Once you’ve installed it, run the following command:  
`ngrok http 5000`

Then copy the forwarding URL in HTTPS (https://XXX.ngrok.io) to the **Bot webhook base URL** field in your bot settings. All requests made to this URL will be forwarded to port 5000 of your computer, so that your local webserver listening on this port can respond.

