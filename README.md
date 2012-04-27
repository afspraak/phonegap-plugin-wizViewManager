


# PLUGIN: 

phonegap-plugin-wizViewManager<br />
version : 1.7<br />
last update : 27/04/2012<br />


# CHANGELOG: 
<br />
- First pull request from [maxogden](https://github.com/maxogden) - Thanks Max!


# KNOWN ISSUES:
<br />
- an outstanding issue where animation options do not work as they throw EXC_BAD_ACCESS errors


# DESCRIPTION :

PhoneGap plugin for;

creating,<br />
removing,<br />
showing,<br />
hiding,<br />
messaging (cross communication with JSON objects to each and every view),<br />
animating,<br />
resizing,<br />
loading source into views.





# INSTALL (iOS): #

Project tree<br />

<pre><code>
project
	/ www
		-index.html
		-newview.html
		-json.js
		/ phonegap
			/ plugin
				/ wizViewManager
					/ wizViewManager.js	
	/ Classes
		AppDelegate.m
	/ Plugins
		/ wizViewManager
			/ wizViewManager.h
			/ wizViewManager.m
	-project.xcodeproj
</code></pre>



1 ) Arrange files to structure seen above.

2 ) Add to phonegap.plist in the plugins array;<br />
Key : wizViewManager<br />
Type : String<br />
Value : wizViewManager<br />


3 ) Configure your AppDelegate.m

See the example AppDelegate.m
(just add/copy \#import and shouldStartLoadWithRequest function)


4 ) Add \<script\> tag to your index.html<br />
\<script type="text/javascript" charset="utf-8" src="phonegap/plugin/wizViewManager/wizViewManager.js"\>\</script\><br />
(assuming your index.html is setup like tree above)
You will also need the json.js (for getting JSON objects)


5 ) Follow example code below.






<br />
<br />
<br />
# EXAMPLE CODE : #

<br />
<br />
Creating a view<br />
<pre><code>
wizViewManager.create(String viewName, JSONObject options, Function success, Function fail);

    * Example options object; 

{

    "height": "300", [pixels - default : fills height] 
    "width": "300", [pixels - default : fills width] 
    "x": "0", [pixels - default : 0] 
    "y": "0", [pixels - default : 0] 
    "src": "http://google.com" [URL/URI to load] 

}; 
</code></pre>


Load source into view<br />
<pre><code>
wizViewManager.load(String viewName, String URI or URL, Function success, Function fail);
</code></pre>


Change or set the Layout of a view<br />
<pre><code>
wizViewManager.setLayout(String viewName, JSONObject options, Function success, Function fail);
    * Height overrides top and bottom. Width overrides left and right.  
    * Top, bottom,left,right,width,height all take floats (0.25) or string "25%" as percents, int (25) as pixcels.
    * Example options object; 

{

    "height": "300", [pixels - default : fills height] 
    "width": "300", [pixels - default : fills width] 
    "x": "0", [pixels - default : 0] 
    "y": "0", [pixels - default : 0] 
    "top": "0", [pixels or percent - default : 0]
    "bottom": "0", [pixels or percent - default : 0] 
    "left": "0", [pixels or percent - default : 0] 
    "right": "0", [pixels or percent - default : 0] 

}; 
</code></pre>



Remove a view<br />
```
wizViewManager.remove(String viewName, Function success, Function fail); 
```


Show a view<br />
<pre><code>
wizViewManager.show(String viewName, JSONObject animOptions, Function success, Function fail);

    * Animation Types; 

slideInFromLeft - iOS
slideInFromRight - iOS
slideInFromTop - iOS
slideInFromBottom - iOS
fadeIn - iOS/Android
zoomIn - iOS/Android

    * Example animOptions object; 

animation : {

    "type": "fadeIn", [string - default : fadeIn] 
    "duration": "300", [int - default : 500] 

}; 
</code></pre>



Hide a view<br />
<pre><code>
wizViewManager.hide(String viewName, JSONObject animOptions, Function success, Function fail);

    * Animation Types; 

slideOutFromLeft - iOS
slideOutFromRight - iOS
slideOutFromTop - iOS
slideOutFromBottom - iOS
fadeOut - iOS/Android
zoomOut - iOS/Android

    * Example animOptions object; 

animation : {

    "type": "fadeOut", [string - default : fadeOut] 
    "duration": "300", [int - default : 500] 

}; 
</code></pre>

Message a view<br />
add a receiver to your html that gets the message...
<pre><code>
function wizMessageReceiver(data) 
{
                        
    // your data comes in here
    console.log('i got :' + data);
                        
}
</code></pre>

to send a messsage, add this to the html you wish to send from use...
<pre><code>
function sendExample()
{
    
    var targetView = 'newView';
    var action = 'message';
    var params = { 'x' : 500 };
    
    
    postEvent(targetView, action, params );
    
}

function postEvent(targetView, action, params) 
{
    var msg = { 'event' : action , 'params' : params };
    
    var iframe = document.createElement('IFRAME');
    iframe.setAttribute('src', 'wizMessageView://'+ window.encodeURIComponent(targetView)+ '?'+ window.encodeURIComponent(JSON.stringify(msg)) );
    document.documentElement.appendChild(iframe);
    iframe.parentNode.removeChild(iframe);
    iframe = null;
}
</code></pre>