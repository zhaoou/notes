The Document Object Model

So far, we’ve been talking a lot about language and theory. You might be thinking, “When do we get to write a web application?” That’s what the next two chapters are for.

In order to write an application, our code needs to be able to interface with the browser and web page. It turns out that interface is not part of JavaScript itself. Instead, it is part of a (somewhat) standardized API called the Document Object Model (DOM). The DOM is how JavaScript code can interact with elements on a HTML web page.

DOM Objects

As specified by the DOM API, each HTML element is represented by an object, hence the term “Object Model”. Each object has various properties and methods that corresponds to the appearence or behavior of the corresponding HTML element. The object also has parents, children and siblings as specified by the DOM tree.

Each node has a type, corresponding to what kind of HTML element it represents. Each node has different properties and methods available, depending on its type. There is a type hierarchy for DOM nodes, as shown below. The base type of all dom nodes is Node, which contains methods for traversing the tree. All HTML elements would have the type Element and HTMLElement, but an image would have type HTML and so on.


The DOM Tree

A HTML document consists of nested HTML elements. The document can be viewed as a tree of elements. Each element is a child of the element that immediately encloses it. Similarly, each element is a ancestor of all the elements it encloses.

For instance, consider the following example HTML:

<div>
	<h1>Header</h1>
	<ul>
		<li>Point 1</li>
		<li>Point 2</li>
		<li>Point 3</li>
	</ul>
</div>
In this example, the div element is the parent of two child nodes: the h1 and the ul elements. The ul element is a parent of three child nodes: the li elements.

The document Object

The document object represents the HTML document itself. It is at the root of the DOM tree that we previously discussed. It also provides us with a number of methods for searching for other nodes in the document.

The most common way to select an element is the document.getElementById method. Each element in a HTML document can have an optional ID attribute, which must be unique. This method takes a ID string and returns the reference to the element with the ID, or null if there is no such element.

var elem = document.getElementById("para1");
<p class="hello">Hello world!</p>
There are other uncommon ways to select elements:

document.getElementsByClassName
Returns a list of elements with the given class name in the document. The collection is a live collection, which means that if we add or remove items from the DOM tree, the list gets updated automatically.
document.getElementsByTagName
Returns a list of elements with the given tag name in the document. The collection is a live collection, which means that if we add or remove items from the DOM tree, the list gets updated automatically.
Deprecated Element Selection

For historical reasons, the DOM interface also provides other short-hand ways to access elements by ID. For instance, the document.forms object is an object that maps id strings to the corresponding form objects. The document.images object is does the same for images, and the document.links object does the same for links.

var goldenGateImg = document.images["golden_gate"];
Traversing the DOM Tree

Once you have an element, there are a number of elements that let you get elements from elsewhere in the tree.

element.parentNode
Returns the parent node of the given element.
element.childNodes
Returns a collection of child nodes of the given element.
element.firstChild
Returns the node’s first child in the tree, or null if the node is childless.
element.lastChild
Returns the node’s last child in the tree, or null if the node is childless.
element.nextSibling
Returns the node immediately following the specified one in its parent’s childNodes list, or null if the specified node is the last node in that list.
element.previousSibling
Returns the node immediately preceding the specified one in its parent’s childNodes list, null if the specified node is the first in that list.
DOM Element Attributes

Now that we have a DOM element, we can access or modify its attributes. Most HTML attributes can be accessed through the JavaScript object property of the same name. For instance, the src property of an image DOM object represents the URL of the image.

var goldenGateImg = document.getElementById("golden_gate");
console.log("The URL of this image is: " + goldenGateImg.src);
These properties behave like public properties and c

var goldenGateImg = document.getElementById("golden_gate");
goldenGateImg.src = "../img/golden_gate.jpg";
For most attributes, the DOM name for the HTML attribute is simply the attribute name written in camelcase. For instance, tabindex in HTML would become tabIndex in JavaScript. Note that while the HTML name is case-insensitive, the JavaScript name is case-sensitive.

DOM Element Contents

Another common operation is that we would often like to retrieve or change the contents of the element. There are two properties that let us do that. You can retrieve values from these properties, or you can assign new values to them, which would cause the contents of the element to change.

element.innerHTML
This property represents the HTML within the element.
element.textContent
This property represents the plain text within the element. This is equivalent to the inner HTML, but with all the tags stripped out.
Event-based Programming

The programs discussed so far are sequential. When they are loaded, they perform a series of instructions. This is analogous to having a main function in C++ or Java: when the program is run, the main function is run until it is completed, then the program quits.

However, JavaScript is often used for programming client-side interfaces. We want a client-side interface to perform actions in response to user actions. Enter event-based programming.

Event-based programming (also known as event-driven programming) is a programming paradigm in which we write programs that respond to events. An event can be anything interesting that happens to the program. For instance, a page load, keyboard button press, or mouse click can all be events. When these events occur, the program will run a given function. This function is known as a the event listener or event handler.

Here’s a first exapmle of event-based programming:

var helloWorldFn = function() { console.log("hello world!")};
document.getElementById("hello_world_btn").addEventListener("click", helloWorldFn, false)
In this example, the element with the ID “hello_world_btn” is the event target. The name of hte event is “click”. helloWorldFn is the event listener.

Registering and Unregistering Events

The best way to register an event listener is to use the addEventListener method. The receiver of the method is the target of the event. The first parameter should be the name of the event, and the second parameter should be callback function that is fired upon receiving the event. The third event specifies whether the listener has a property called “capture”, which causes the listener to fire before the normal event bubbling chain. (In theory, the third parameter should be optional and default to false. In practice, many browsers consider a missing third parameter to be an error, so you should always include it and set it to false.)

Multiple event listeners can be bound to the same function. The event listeners will be fired in the order that they were bound.

var helloWorldFn = function() { console.log("hello world!")};
document.getElementById("hello_world_btn").addEventListener("click", helloWorldFn, false)
There is a corresponding method called removeElementListener, which removes the event listener from the element.

var helloWorldFn = function() { console.log("hello world!")};
document.getElementById("hello_world_btn").addEventListener("click", helloWorldFn, false)
document.getElementById("hello_world_btn").removeEventListener("click", helloWorldFn, false)
The other way registering events was to set the listener as the property of the target element. For instance, the onclick property of a button object would represent the listener for the click event for that button. To set the listener for the event, we simply assign the listener to the property. To remove the listener, we set the listener to be equal to null.

var helloWorldFn = function() { console.log("hello world!")};
document.getElementById("hello_world_btn").onclick =  helloWorldFn;
This use has become uncommon, because it only allows one event handler per event per element. It also doesn’t play well with addEventListener or removeEventListener.

The event object

When the event listener is fired, the browser passes it a single parameter. This parameter is the event object:

var helloWorldFn = function(e) { console.log(e)};
document.getElementById("hello_world_btn").onclick =  helloWorldFn;
The event object is filled with properties that tell you useful information about the event. Whenever an event fires, an object is created to represent the event, and is automatically given to your event handler as the first parameter to your event listener.

All events have an event type. This tells the event handler what type of event it is, whether it is a keyboard key press, or a mouse click, or a page load. This can be retrieved via the type property of the event object.

Most events also have a target. The target is the element upon which the event was fired. For instance, if the event was triggered by a click on a link, the link is the target of the event. If the event was a keyboard button press while the user was typing in a textbox, the textbox is the target of the event. This can be retrieved via the type property of the event object.

Default Actions

Many events have a default action. For instance, if you click on a hyperlink, it will navigate to the target page by default. If you click on a form submission button, it will submit the form to the server by default.

If there is an event listener for that event, the event listener will fire first before the default action is run. The event listener can choose to prevent the action from being run. This can be useful in various ways. If you’re writing a AJAX-y application, you can make it such that when a user clicks a “next page” link, instead of navigating the next page, . If you’re writing a form validation script, you can prevent the form from submitting if errors were detected.

There are two ways to to prevent the default action from happening. First, the event object that is passed as a parameter has a method called preventDefault. If the event listener calls the method, the default action will be cancelled.

var preventDefaultFn = function(e) { e.preventDefault() };
document.getElementById("disabled_link").addEventListener("click", preventDefaultFn, false);
Second, if the listener returns the value false, the default action will be cancelled.

var preventDefaultFn = function() { return false };
document.getElementById("disabled_link").addEventListener("click", preventDefaultFn, false);
Event Propagation

Suppose a page contained input button element contained within a p element, contained within a div element. If you clicked on the button element, which click handler(s) should fire: the click handler for the button, the p element or the div element? The answer is all of them.

When you click on the button, the event handler on the button fires first. Next, the event handler on the parent p element is invoked. Finally, the event handlers on the parent div element is invoked.

This behavior is known as event propagation or event bubbling. When an event is fired, the element’s event handlers are first fired. The element’s parent’s event handlers are fired. That element’s parent’s event handlers are fired, and so on, until the root Document object is reached. Most DOM events exhibit event propagation.

When an event handler is fired, it has an opportunity to stop the event from propogating to its parent. To do so, the handler can call event.stopPropagation() method on the event object.


http://web.stanford.edu/class/cs98si/
