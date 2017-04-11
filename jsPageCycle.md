# JS page cycle


2017 is officially a year of the JavaScript, in my calendar.

Once again, I will be posting my notes here.

To understand JS, we need to understand the environment it is operating in: browser.

Browsers perform the following two tasks for each page:

* Build DOM and parse JS
* Manage the event queue

##Build DOM and parse JS
As a browser receives content(HTML, CSS, & JS) from the webserver, it starts building DOM from the html and parses JS as it is encountered. Browser switches between these two task: parsing JS, building DOM, depending on the content from the server.

All JS encountered by the browser can be divided into:

* **Global.** Executed as encountered.

* **Function defining.** Defines functions to be used by the following code or at a later time.

When DOM is rendered and JS is parsed, browsers switch to the event queue management.

##Manage the event queue
As a mouse is moved, server responses received, and timers expire, events are generated and placed on a single event queue.

Many events are generated at any given second, but nothing happens because not all of the events are of interest. To indicate that some event is of interest to us, we register an event handler - a simple, quick, function to be invoked on a given event.

Some events only require a single handler, while others need an array of handlers, out of necessity or otherwise.

The former can be done by **assigning** an event handler:
```
window.onload = somefunction;
```
This will replace or assign `somefunction` to the `onload` event.

The latter is done by **adding** an event handler:
```
element.addEventListener("click", somefunction);
```
This will add new handler to the existing 0-many handlers assigned to that event of that element already.

Now, everytime a `click` or `onload` events are removed from the event queue, corresponding functions will be invoked.


Stay tuned for more JS goodies!
