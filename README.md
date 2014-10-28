Mandrill_Node.js
================

[Mandrill](http://mandrill.com) allows you to send emails from your website or application, and track the results!

###Getting Started
------

First you'll need to create an account. Once you're logged into your accont, click the options to Get API keys. Generate a new key, and copy it so you can use it for the exercises. For future refrence, you can accesss API keys at Settings > SMTP & API Credentials.

[Mandrill API](https://www.npmjs.org/package/mandrill-api)is a npm module for Node.js that makes it so you can use Mandrill's API.

I've created a basic Node application using express.  Fork and clone the repo - open the terminal and cd into the directory - and install the dependencies:
```
npm install
```  

###Sending and email with pure HTML
Let's jump right into it! Head over to your app/mandrill-controller.js file.  At the top we want to require mandrill's API:
```javascript
var mandrill = require('mandrill-api/mandrill');
var mandrill_client = new mandrill.Mandrill('YOUR_API_KEY');
```

Great, let's construct a message that we will use to email: 
```javascript
var params = {
	"message": {
	    to: [{email: '<Send_To_Email>', name: '<Your_Name_Here>'}],
	    from_email: '<Sent_From_Email>',
	    subject: "Thanks for Registering",
	    html: '<h1>Guess what?...Chicken Butt!</h1>'
	}
};
```
There are a lot of options here for the message.  Check out the [Mandrill API](https://mandrillapp.com/api/docs/messages.nodejs.html#method=send).

Not let's add in a sendEmail method.
```javascript
module.exports.sendEmail = function(req, res){
	mandrill_client.messages.send(params, function(result){
		return res.status(200).send(result);
	}, function(err){
		return res.send(err);
	});
}
```

Now for the fun part.  Go to server.js and require the mandrill-conotoller file under required files.
```javascript
var mandrillCtrl = require('./app/mandrill-controller');
```

Head down the the routes and lets make the magic happen:
```javascript
app.get('/mandrill', mandrillCtrl.sendEmail);
```

Go to the terminal - cd into your directory - type $ node server.js - open Postman and type in http://localhost:8080/mandrill and select GET from the dropdown - press the send button...

**BOOM!** Now as long as your weren't a dumbosaurus with syntax errors, you should see and email in your box.  Not there you say? Well check your spam folder.  Voila! 

Down the road, when you are sending if from a real website with a real domain.  Head over to the Mandrill [SPF and DKIM Docs](http://help.mandrill.com/entries/21751322-What-are-SPF-and-DKIM-and-do-I-need-to-set-them-up-) to find out more.





