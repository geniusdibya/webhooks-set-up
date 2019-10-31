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

## How to set up a webhook?

1) Make sure your port 80 and 443 (default it changes as per your configuration) are open for your webhook provider ie. it should be publicly accessible.

   Check using telnet 

   `telnet webhook.mydomain.com 80{443 or any other port}`

   If the URL is open it will give below results

   ```bash
   Connected to webhook.mydomain.com.
   Escape character is '^]'.
   ^]
   telnet> quit
   Connection closed.
   ```

2) Prepare a URL that you need to share with your webhook provider.
ie. deploying a piece of code (webhook receiving module) for consuming data which will be posted by the provider. 

## Writing Webhooks in PHP

If PHP is installed on your web server you can simply copy & paste the below code snippet in file getwebhooks.php at your web server location

**Default locations**

Apache web server:  `/var/www/html/getwebhooks.php`

Nginx web server :  `/usr/share/nginx/html/getwebhooks.php`

If you have the custom path : `/{path to web server}/getwebhooks.php`

**getwebhooks.php**

```php
<?php
$webhook_data = file_get_contents('php://input');
file_put_contents('/tmp/consumewebhook.log', $webhook_data)
?>
```

## Writing Webhooks in Python

Similarly for python install/update web, six packages using below command:
    
`sudo pip install web.py==0.40`{whichever is latest}

`sudo pip install six --upgrade`

copy paste the below code snippet in file **getwebhooks.py**

```python
import web

urls = ('/.*', 'getwebhooks')

app = web.application(urls, globals())

class getwebhooks:
    def POST(self):
        data = web.data()
        print
        print 'DATA RECEIVED:'
        print data
        print
        return 'OK'

if __name__ == '__main__':
    app.run()
```

Run using 

`python getwebhooks.py 6565` (if Port not specified default it will take 8080)

Test by hitting below command from the terminal. 

```bash
curl -v --request POST --url http://mydomain.webhooks.example:6565/webhook --header 'Content-Type: application/json' --data '[{“hello”:”world”}]’
```

## What kind of data will I receive in a webhook POST request (consuming webhooks)?

Data posted on your webhook URL will be JSON(Mostly). 

Most of the webhooks, POST data to you in JSON format besides JSON there will be 2 more ways XML or form-data (application/x-www-form-urlencoded or multipart/form-data). No matter what content-type is posted to you, almost all web library will support or help you in interpreting. 

If in case, it doesn’t have, you will be able to write one or two functions of your own.  

## How to test webhook URL (sending POST request with data)?

You can send data using an API / REST Development tools but posting data can be tedious at times. Since the webhook calls are asynchronous you need to wait for the response after triggering an event.

But there are already available tools and with the help of those you can always check the response without any setup  
2 popular HTTP events logging sites are 

1. [Webhook Site](https://webhook.site/)

  * Generates the random URL and help you test, inspect the action for each events on your webhook URL.
  
  ![webhook-site](https://i.imgur.com/Qo5mHlV.png)
  
2. [Request bin](https://requestbin.com/)

  ![Imgur](https://i.imgur.com/SFgi3NW.png)
  
  * All you need is the URL they provide and hit any curl request to particular URL they will help you render the posted data on their UI.

**Some of the famous REST development tools can be used to post or receive data.**

* [Postman](https://www.getpostman.com/) (popular)
* [Advanced REST Client](https://chrome.google.com/webstore/detail/advanced-rest-client/hgmloofddffdnphfgcellkdfbfbjeloo)(ARC)
* [Paw](https://paw.cloud/)
* [HttpRequester](https://sourceforge.net/projects/httprequester/)
* [Curl](https://curl.haxx.se/) through terminal 

You can always try with your existing web-server on localhost or create one with [ngrok](https://ngrok.com)

Simply hit the below command on your terminal 

```bash
curl -v --request POST --url http://yourwebhook.domain.com/getwebhooks --header 'Content-Type: application/json' --data '[{"SIZE":null,"SUBJECT":"Thank you for your interest in account","TRANSID":"15086970014684020","RESPONSE":"74.xxx.xx.27 - smtp;250 2.0.0 OK 1508714170 x69si1901723otb.460 -gsmtp","RCPTID":"0","EMAIL":"c.xxxxxx@gmail.com","TAGS":"Content","TIMESTAMP":"1508714170","CLIENTID":"xxxx","FROMADDRESS":"invest@xxxx.com","MSIZE":"9697","X-APIHEADER":"Content","EVENT":"sent"}]'
```

*note :: Above is pepipost sample webhook data whose content-type is  application/JSON it may vary with respect to your webhook provider

## How would I secure my webhook URL (security)?

Since webhook URL is publicly open, there will be always a security concern of someone accessing URL and posting improper data. There are many ways to implement security on the URL but few most important are as follows :

* Allow only TLS connections (HTTPS).
  
  TLS ensures that data is encrypted over the internet with the help of some secure algorithm and it should be not viewable by any third party while transmission. By default every web servers support TLS. 
    
* Basic security among all is to keep an Auth token eg. ?mysecure=PEPI
  
  A token is meant to validate whether the source is authentic or not. These tokens are used to check whether the source has access for reading/modifying application. Restricting unknown source will help to prevent useless data which automatically increase the availability of your URL. 

* Implement all the basic access authentication which is required by any HTTP user agent.

  This is the simplest technique of enforcing access controls to your web resources because it does not require any cookies, sessions or login pages rather it uses simple HTTP-header authentication method. Basic authentication usually takes action with HTTPS to provide confidentiality.

## Conclusion

