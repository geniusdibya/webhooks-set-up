# webhooks-set-up

## What is Webhook?
 
Webhook is very simple and understanding the webhook’s working mechanism is much simpler too. Ideally, it sends notification (data) on a particular URL mostly via POST method request on every activity/action of the user.

Technically, you need to share a publicly accessible URL with the provider and whenever an action or activity is performed by the user, data will be posted on the client-side application server on which your webhook URL is configured to consume that data.

Webhook has many different names such as “Reverse API”, “web callback” or “HTTP push API”. 

Many applications allow you to set up webhooks for tracking your realtime activities. Here are a sample list of products who over webhooks integration:

1) [Github](github.com) (extensively used among developers, [notifies for every activity/action](https://developer.github.com/webhooks/) on your repository).

2) [Slack](slack.com) (Slack webhook [notifies you on a particular channel on a particular activity](https://api.slack.com/messaging/webhooks) - any integration related to development or third party)

3) [Facebook](https://developers.facebook.com/docs/messenger-platform/getting-started/webhook-setup/) (Receive real-time notifications of changes to your Facebook Page)

4) [Pepipost](https://docs.pepipost.com/docs/webhooks) (notifies you on every activity on email)* 
