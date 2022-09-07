<properties
   pageTitle="JavaScript API"
   description="JavaScript API"
   services="app-service\logic"
   documentationCenter=".net,nodejs,java"
   authors="stepsic-microsoft-com"
   manager="dwrede"
   editor=""/>

<tags
   ms.service="app-service-logic"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration"
   ms.date="07/16/2015"
   ms.author="stepsic"/>

#JavaScript API App
The JavaScript API App gives you an easy way to run simple JavaScript expressions *while your Logic App executes*. 

##When should you use this API app?
The key scenario for this API app is when you want the lifecycle of the code that you write to be the same as the Logic App, and you do *not* want the code to be called in any other scenarios.

On the other hand, if you want a reusable snippet of code that has a lifecycle independent of the Logic app, then you should use the WebJobs API app to create simple code expressions and call them from your Logic app.

Finally, if you want to include any additional packages, you will also need to use the WebJobs API app, as you can not add any libraries using the JavaScript API App. 

Use the [C# API App](app-service-logic-cs-api.md) if you would prefer to write your expressions in C#.

##Creating a JavaScript API App
To use the JavaScript API App, you need to first create an instance of the JavaScript API app. This can be done either inline while creating a logic app or by selecting the JavaScript API app from the Azure Marketplace.

##Using JavaScript API App in Logic Apps designer surface
###Trigger
You can create a trigger that the Logic App service will poll (on an interval you define), and, if it returns any content, the Logic App will run, otherwise, it will wait until the next polling interval to check again.

The inputs to the trigger are:
- **JavaScript expression**  - An expression that will be evaluated. It will be invoked inside a function and must return `false` when you do not want the Logic app to run, and can return anything else when you want the Logic app to run. You will be able to use the content of the response in the actions of the Logic app.
- **Context object** - An optional object that can be passed into the trigger. You can define as many properties as you want, but the top-level entity must be an object, e.g. `{ "bar" : 0}`.

For example, you could have a simple trigger that will only run your Logic app between the :15 and :30 of the hour:

```
var d = new Date(); return (d.getMinutes() > 15) && (d.getMinutes() < 30);
```

###Action

Likewise, you can provide an action to run. 

The inputs to the action are:
- **JavaScript expression**  - An expression that will be evaluated. You must include the `return` statement to get any content. 
- **Context object** - An optional object that can be passed into the trigger. You can define as many properties as you want, but the top-level entity must be an object, e.g. `{ "bar" : 0}`.

For example, imagine you are using the Office 365 trigger **New Email**. That returns the following object:
```
{
	...
	"Attachments" : [
		{
			"name" : "Picture.png",
			"content" : {
				"ContentData" : "iVBORw0KGgoAAAANSUhEUgAAAAUAAAAFAQMAAAC3obSmAAAABGdBTUEAALGPC/xhBQAAAAFzUkdCAK7OHOkAAAAGUExURf///wAAAFXC034AAAASSURBVAjXY2BgCGBgYOhgKAAABEIBSWDJEbYAAAAASUVORK5CYII=",
				"ContentType" : "image/png",
				"ContentTransferEncoding" : "Base64"
			}
		},	
		{
			"name" : "File.txt",
			"content" : {
				"ContentData" : "Don't worry, be happy!",
				"ContentType" : "text/plain",
				"ContentTransferEncoding" : "None"
			}
		}	
	]
}
```

But, you want to upload these attachments to a Yammer post. Unfortunately, the schema for Yammer attachments is slightly different. Now, you can now parse this inside your Logic app. For the context object just pass: `@triggerBody()`, and for the expression, pass:

```
return Attachments.map(function(obj){var a = obj.Content; a.FileName = obj.Name; return a;})
```

The action returns the JSON that you returned from your function. Thus, in the Yammer API app you can reference `@body('javascriptapi')` for the **Attachments** property.

## Do more with your Connector
Now that the connector is created, you can add it to a business flow using a Logic App. See [What are Logic Apps?](app-service-logic-what-are-logic-apps.md).

You can also review performance statistics and control security to the connector. See [Manage  and Monitor API apps and connector](../app-service-api/app-service-api-manage-in-portal.md).

<!--References -->

<!--Links -->
[Creating a Logic App]: app-service-logic-create-a-logic-app.md

test
