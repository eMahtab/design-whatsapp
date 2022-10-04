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

**Sent, Delivered and Read Notification :**
For each message, there can be up to three callbacks from the WhatsApp's chat servers. These callbacks are sent, delivered and read notifications.


Suppose User1 sends a message to User2 and immediately gets offline, now in this case , the WhatsApp chat server won't be able to send the above notification to User1, when User2 recieves the message or reads the message. So we need to store this data in some persistent storage at least for time till the notifications are not sent to User1 successfully, and once the notifications are sent to User1 successfully and chat server gets the acknowledgement, we can delete the data from persistent storage.

!["Whatsapp"](whatsapp.PNG?raw=true)


## Questions :
1. Should users periodically poll the chat server and check for any new messages

