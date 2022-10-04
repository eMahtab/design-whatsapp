# Design WhatsApp

## Functional requirements
1. Text messaging (1 to 1)
2. Sent, Delivered and Read reciept
3. User A should be able to send message to user B, even if user B is currently offline, and the message should get delivered to user B when he comes online
4. Group Messages

## Non functional requirements
1. Highly available
2. Low latency

# Billions of users use Whatsapp daily and Whatsapp delivers billions of messages everyday.

## Where does Whatsapp stores the messages :
WhatsApp does not store your chat history on its servers. When you send a message, WhatsApp **only keeps that message on its servers until the recipient receives the message or for a max of 30 days**. Even then, the end-to-end encryption prevents WhatsApp from reading your message. 

The only time a message is stored on WhatsApp’s servers is if the recipient cannot receive it (maybe they’re offline or don’t have a WhatsApp account). In cases like those, the message(s) you sent are automatically deleted off of WhatsApp’s server in 30 days’ time. 

**Once Whatsapp recieves a delivery acknowledgement for a message, the message gets deleted from Whatsapp's database.**

!["Whatsapp"](whatsapp.PNG?raw=true)


## Questions :
1. Should users periodically poll the chat server and check for any new messages

