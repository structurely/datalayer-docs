# Getting Started

## Conversation API

### Setting Up the Environment

To start put your API key somewhere in you private configurations. This key is specific to your application/company so do not share with any end users either through support channels or exposing it on a end user application like the browser or an app. You will be using this API key for all your users' accounts to exchange data, create conversations, and set up/receive webhooks.

If you want to receive data from us such as sending messages or updating conversations slots and stages you will need to set up one or more webhooks with the /v1/conversationWebhooks endpoints. Create a webhook or multiple webhooks with the triggers 'response:created' and 'conversation:updated'. These webhooks will allow us to push new responses to you to send to the leads and to update lead information in your system with our slot and stage updates. Be sure to keep the webhook secret returned from the API because that will be used to authenticate the webhook on your side.

### Create a Conversation

After the environment is set up you can begin creating conversations using the /v1/conversations endpoints. When a lead responds to a message, create a conversation with the entire message history and relevant data as messages and slots. Be sure to set the context of each response object for your outbound messages so that we can properly interpret the message that the lead sent. After a conversation is created, use the create message method to add any new messages a lead might send to the conversation so that we can generate updates and responses for those messages. If any lead profile fields update from your application, use the conversation update method to change or add corresponding slot values.

### Sending Responses and Webhooks

If you have webhooks set up, conversation updates and responses will be pushed when generated. When a response:created webhook is triggered, you can send the message to the lead with your own messaging service like Twilio or Smooch webchat. When you send the response make sure you add the response with the conversation API create response method. This is used by us to update the internals so we know what was sent as part of the conversation.  Stage updates are also sent as part of the conversation:updated trigger. Stage updates can include a reason which can be used to determine what to do with your leads in your system.

