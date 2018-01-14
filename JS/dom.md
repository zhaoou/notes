<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <title>The Document Object Model</title>
    <link href='http://fonts.googleapis.com/css?family=Source+Sans+Pro:400,700,400italic,700italic|Source+Code+Pro:400,700' rel='stylesheet' type='text/css'>
    <link rel="stylesheet" href="http://www.stanford.edu/~maiyifan/introtojs/css/typography.css">
    <link rel="stylesheet" href="http://www.stanford.edu/~maiyifan/introtojs/codemirror/lib/codemirror.css">
    <link rel="stylesheet" href="http://www.stanford.edu/~maiyifan/introtojs/css/code.css">
</head>
<body>
    <div class="wrapper">
        <h1 id='the_document_object_model'>The Document Object Model</h1>

<p>So far, we&#8217;ve been talking a lot about language and theory. You might be thinking, &#8220;When do we get to write a web application?&#8221; That&#8217;s what the next two chapters are for.</p>

<p>In order to write an application, our code needs to be able to interface with the browser and web page. It turns out that interface is not part of JavaScript itself. Instead, it is part of a (somewhat) standardized API called the Document Object Model (DOM). The DOM is how JavaScript code can interact with elements on a HTML web page.</p>

<h2 id='dom_objects'>DOM Objects</h2>

<p>As specified by the DOM API, each HTML element is represented by an object, hence the term &#8220;Object Model&#8221;. Each object has various properties and methods that corresponds to the appearence or behavior of the corresponding HTML element. The object also has parents, children and siblings as specified by the DOM tree.</p>

<p>Each node has a type, corresponding to what kind of HTML element it represents. Each node has different properties and methods available, depending on its type. There is a type hierarchy for DOM nodes, as shown below. The base type of all dom nodes is <code>Node</code>, which contains methods for traversing the tree. All HTML elements would have the type <code>Element</code> and <code>HTMLElement</code>, but an image would have type HTML and so on.</p>
<img src='../img/dom_types.png' />
<h2 id='the_dom_tree'>The DOM Tree</h2>

<p>A HTML document consists of nested HTML elements. The document can be viewed as a tree of elements. Each element is a child of the element that immediately encloses it. Similarly, each element is a ancestor of all the elements it encloses.</p>

<p>For instance, consider the following example HTML:</p>
<div class='highlight'><pre><code class='js'><span class='o'>&lt;</span><span class='nx'>div</span><span class='o'>&gt;</span>
	<span class='o'>&lt;</span><span class='nx'>h1</span><span class='o'>&gt;</span><span class='nx'>Header</span><span class='o'>&lt;</span><span class='err'>/h1&gt;</span>
	<span class='o'>&lt;</span><span class='nx'>ul</span><span class='o'>&gt;</span>
		<span class='o'>&lt;</span><span class='nx'>li</span><span class='o'>&gt;</span><span class='nx'>Point</span> <span class='mi'>1</span><span class='o'>&lt;</span><span class='err'>/li&gt;</span>
		<span class='o'>&lt;</span><span class='nx'>li</span><span class='o'>&gt;</span><span class='nx'>Point</span> <span class='mi'>2</span><span class='o'>&lt;</span><span class='err'>/li&gt;</span>
		<span class='o'>&lt;</span><span class='nx'>li</span><span class='o'>&gt;</span><span class='nx'>Point</span> <span class='mi'>3</span><span class='o'>&lt;</span><span class='err'>/li&gt;</span>
	<span class='o'>&lt;</span><span class='err'>/ul&gt;</span>
<span class='o'>&lt;</span><span class='err'>/div&gt;</span>
</code></pre></div>
<p>In this example, the <code>div</code> element is the parent of two child nodes: the <code>h1</code> and the <code>ul</code> elements. The <code>ul</code> element is a parent of three child nodes: the <code>li</code> elements.</p>

<h2 id='the__object'>The <code>document</code> Object</h2>

<p>The <code>document</code> object represents the HTML document itself. It is at the root of the DOM tree that we previously discussed. It also provides us with a number of methods for searching for other nodes in the document.</p>

<p>The most common way to select an element is the <code>document.getElementById</code> method. Each element in a HTML document can have an optional ID attribute, which must be unique. This method takes a ID string and returns the reference to the element with the ID, or null if there is no such element.</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>elem</span> <span class='o'>=</span> <span class='nb'>document</span><span class='p'>.</span><span class='nx'>getElementById</span><span class='p'>(</span><span class='s2'>&quot;para1&quot;</span><span class='p'>);</span>
</code></pre></div><div class='highlight'><pre><code class='html'><span class='nt'>&lt;p</span> <span class='na'>class=</span><span class='s'>&quot;hello&quot;</span><span class='nt'>&gt;</span>Hello world!<span class='nt'>&lt;/p&gt;</span>
</code></pre></div>
<p>There are other uncommon ways to select elements:</p>

<dl>
<dt><code>document.getElementsByClassName</code></dt>

<dd>Returns a list of elements with the given class name in the document. The collection is a live collection, which means that if we add or remove items from the DOM tree, the list gets updated automatically.</dd>

<dt><code>document.getElementsByTagName</code></dt>

<dd>Returns a list of elements with the given tag name in the document. The collection is a live collection, which means that if we add or remove items from the DOM tree, the list gets updated automatically.</dd>
</dl>

<h2 id='deprecated_element_selection'>Deprecated Element Selection</h2>

<p>For historical reasons, the DOM interface also provides other short-hand ways to access elements by ID. For instance, the <code>document.forms</code> object is an object that maps <code>id</code> strings to the corresponding form objects. The <code>document.images</code> object is does the same for images, and the <code>document.links</code> object does the same for links.</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>goldenGateImg</span> <span class='o'>=</span> <span class='nb'>document</span><span class='p'>.</span><span class='nx'>images</span><span class='p'>[</span><span class='s2'>&quot;golden_gate&quot;</span><span class='p'>];</span>
</code></pre></div>
<h2 id='traversing_the_dom_tree'>Traversing the DOM Tree</h2>

<p>Once you have an element, there are a number of elements that let you get elements from elsewhere in the tree.</p>

<dl>
<dt>element.parentNode</dt>

<dd>Returns the parent node of the given element.</dd>

<dt>element.childNodes</dt>

<dd>Returns a collection of child nodes of the given element.</dd>

<dt>element.firstChild</dt>

<dd>Returns the node&#8217;s first child in the tree, or null if the node is childless.</dd>

<dt>element.lastChild</dt>

<dd>Returns the node&#8217;s last child in the tree, or null if the node is childless.</dd>

<dt>element.nextSibling</dt>

<dd>Returns the node immediately following the specified one in its parent&#8217;s <code>childNodes</code> list, or <code>null</code> if the specified node is the last node in that list.</dd>

<dt>element.previousSibling</dt>

<dd>Returns the node immediately preceding the specified one in its parent&#8217;s <code>childNodes</code> list, <code>null</code> if the specified node is the first in that list.</dd>
</dl>

<h2 id='dom_element_attributes'>DOM Element Attributes</h2>

<p>Now that we have a DOM element, we can access or modify its attributes. Most HTML attributes can be accessed through the JavaScript object property of the same name. For instance, the <code>src</code> property of an <code>image</code> DOM object represents the URL of the image.</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>goldenGateImg</span> <span class='o'>=</span> <span class='nb'>document</span><span class='p'>.</span><span class='nx'>getElementById</span><span class='p'>(</span><span class='s2'>&quot;golden_gate&quot;</span><span class='p'>);</span>
<span class='nx'>console</span><span class='p'>.</span><span class='nx'>log</span><span class='p'>(</span><span class='s2'>&quot;The URL of this image is: &quot;</span> <span class='o'>+</span> <span class='nx'>goldenGateImg</span><span class='p'>.</span><span class='nx'>src</span><span class='p'>);</span>
</code></pre></div>
<p>These properties behave like public properties and c</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>goldenGateImg</span> <span class='o'>=</span> <span class='nb'>document</span><span class='p'>.</span><span class='nx'>getElementById</span><span class='p'>(</span><span class='s2'>&quot;golden_gate&quot;</span><span class='p'>);</span>
<span class='nx'>goldenGateImg</span><span class='p'>.</span><span class='nx'>src</span> <span class='o'>=</span> <span class='s2'>&quot;../img/golden_gate.jpg&quot;</span><span class='p'>;</span>
</code></pre></div>
<p>For most attributes, the DOM name for the HTML attribute is simply the attribute name written in camelcase. For instance, <code>tabindex</code> in HTML would become <code>tabIndex</code> in JavaScript. Note that while the HTML name is case-insensitive, the JavaScript name is case-sensitive.</p>

<h2 id='dom_element_contents'>DOM Element Contents</h2>

<p>Another common operation is that we would often like to retrieve or change the contents of the element. There are two properties that let us do that. You can retrieve values from these properties, or you can assign new values to them, which would cause the contents of the element to change.</p>

<dl>
<dt>element.innerHTML</dt>

<dd>This property represents the HTML within the element.</dd>

<dt>element.textContent</dt>

<dd>This property represents the plain text within the element. This is equivalent to the inner HTML, but with all the tags stripped out.</dd>
</dl>

<h2 id='eventbased_programming'>Event-based Programming</h2>

<p>The programs discussed so far are sequential. When they are loaded, they perform a series of instructions. This is analogous to having a <code>main</code> function in C++ or Java: when the program is run, the <code>main</code> function is run until it is completed, then the program quits.</p>

<p>However, JavaScript is often used for programming client-side interfaces. We want a client-side interface to perform actions in response to user actions. Enter event-based programming.</p>

<p>Event-based programming (also known as event-driven programming) is a programming paradigm in which we write programs that respond to events. An event can be anything interesting that happens to the program. For instance, a page load, keyboard button press, or mouse click can all be events. When these events occur, the program will run a given function. This function is known as a the <em>event listener</em> or <em>event handler</em>.</p>

<p>Here&#8217;s a first exapmle of event-based programming:</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>helloWorldFn</span> <span class='o'>=</span> <span class='kd'>function</span><span class='p'>()</span> <span class='p'>{</span> <span class='nx'>console</span><span class='p'>.</span><span class='nx'>log</span><span class='p'>(</span><span class='s2'>&quot;hello world!&quot;</span><span class='p'>)};</span>
<span class='nb'>document</span><span class='p'>.</span><span class='nx'>getElementById</span><span class='p'>(</span><span class='s2'>&quot;hello_world_btn&quot;</span><span class='p'>).</span><span class='nx'>addEventListener</span><span class='p'>(</span><span class='s2'>&quot;click&quot;</span><span class='p'>,</span> <span class='nx'>helloWorldFn</span><span class='p'>,</span> <span class='kc'>false</span><span class='p'>)</span>
</code></pre></div>
<p>In this example, the element with the ID &#8220;hello_world_btn&#8221; is the <em>event target</em>. The name of hte event is &#8220;click&#8221;. <code>helloWorldFn</code> is the <em>event listener</em>.</p>

<h2 id='registering_and_unregistering_events'>Registering and Unregistering Events</h2>

<p>The best way to register an event listener is to use the <code>addEventListener</code> method. The receiver of the method is the target of the event. The first parameter should be the name of the event, and the second parameter should be callback function that is fired upon receiving the event. The third event specifies whether the listener has a property called &#8220;capture&#8221;, which causes the listener to fire before the normal event bubbling chain. (In theory, the third parameter should be optional and default to <code>false</code>. In practice, many browsers consider a missing third parameter to be an error, so you should always include it and set it to <code>false</code>.)</p>

<p>Multiple event listeners can be bound to the same function. The event listeners will be fired in the order that they were bound.</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>helloWorldFn</span> <span class='o'>=</span> <span class='kd'>function</span><span class='p'>()</span> <span class='p'>{</span> <span class='nx'>console</span><span class='p'>.</span><span class='nx'>log</span><span class='p'>(</span><span class='s2'>&quot;hello world!&quot;</span><span class='p'>)};</span>
<span class='nb'>document</span><span class='p'>.</span><span class='nx'>getElementById</span><span class='p'>(</span><span class='s2'>&quot;hello_world_btn&quot;</span><span class='p'>).</span><span class='nx'>addEventListener</span><span class='p'>(</span><span class='s2'>&quot;click&quot;</span><span class='p'>,</span> <span class='nx'>helloWorldFn</span><span class='p'>,</span> <span class='kc'>false</span><span class='p'>)</span>
</code></pre></div>
<p>There is a corresponding method called <code>removeElementListener</code>, which removes the event listener from the element.</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>helloWorldFn</span> <span class='o'>=</span> <span class='kd'>function</span><span class='p'>()</span> <span class='p'>{</span> <span class='nx'>console</span><span class='p'>.</span><span class='nx'>log</span><span class='p'>(</span><span class='s2'>&quot;hello world!&quot;</span><span class='p'>)};</span>
<span class='nb'>document</span><span class='p'>.</span><span class='nx'>getElementById</span><span class='p'>(</span><span class='s2'>&quot;hello_world_btn&quot;</span><span class='p'>).</span><span class='nx'>addEventListener</span><span class='p'>(</span><span class='s2'>&quot;click&quot;</span><span class='p'>,</span> <span class='nx'>helloWorldFn</span><span class='p'>,</span> <span class='kc'>false</span><span class='p'>)</span>
<span class='nb'>document</span><span class='p'>.</span><span class='nx'>getElementById</span><span class='p'>(</span><span class='s2'>&quot;hello_world_btn&quot;</span><span class='p'>).</span><span class='nx'>removeEventListener</span><span class='p'>(</span><span class='s2'>&quot;click&quot;</span><span class='p'>,</span> <span class='nx'>helloWorldFn</span><span class='p'>,</span> <span class='kc'>false</span><span class='p'>)</span>
</code></pre></div>
<p>The other way registering events was to set the listener as the property of the target element. For instance, the <code>onclick</code> property of a button object would represent the listener for the click event for that button. To set the listener for the event, we simply assign the listener to the property. To remove the listener, we set the listener to be equal to <code>null</code>.</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>helloWorldFn</span> <span class='o'>=</span> <span class='kd'>function</span><span class='p'>()</span> <span class='p'>{</span> <span class='nx'>console</span><span class='p'>.</span><span class='nx'>log</span><span class='p'>(</span><span class='s2'>&quot;hello world!&quot;</span><span class='p'>)};</span>
<span class='nb'>document</span><span class='p'>.</span><span class='nx'>getElementById</span><span class='p'>(</span><span class='s2'>&quot;hello_world_btn&quot;</span><span class='p'>).</span><span class='nx'>onclick</span> <span class='o'>=</span>  <span class='nx'>helloWorldFn</span><span class='p'>;</span>
</code></pre></div>
<p>This use has become uncommon, because it only allows one event handler per event per element. It also doesn&#8217;t play well with <code>addEventListener</code> or <code>removeEventListener</code>.</p>

<h2 id='the_event_object'>The event object</h2>

<p>When the event listener is fired, the browser passes it a single parameter. This parameter is the event object:</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>helloWorldFn</span> <span class='o'>=</span> <span class='kd'>function</span><span class='p'>(</span><span class='nx'>e</span><span class='p'>)</span> <span class='p'>{</span> <span class='nx'>console</span><span class='p'>.</span><span class='nx'>log</span><span class='p'>(</span><span class='nx'>e</span><span class='p'>)};</span>
<span class='nb'>document</span><span class='p'>.</span><span class='nx'>getElementById</span><span class='p'>(</span><span class='s2'>&quot;hello_world_btn&quot;</span><span class='p'>).</span><span class='nx'>onclick</span> <span class='o'>=</span>  <span class='nx'>helloWorldFn</span><span class='p'>;</span>
</code></pre></div>
<p>The event object is filled with properties that tell you useful information about the event. Whenever an event fires, an object is created to represent the event, and is automatically given to your event handler as the first parameter to your event listener.</p>

<p>All events have an event type. This tells the event handler what type of event it is, whether it is a keyboard key press, or a mouse click, or a page load. This can be retrieved via the <code>type</code> property of the event object.</p>

<p>Most events also have a target. The target is the element upon which the event was fired. For instance, if the event was triggered by a click on a link, the link is the target of the event. If the event was a keyboard button press while the user was typing in a textbox, the textbox is the target of the event. This can be retrieved via the <code>type</code> property of the event object.</p>

<h2 id='default_actions'>Default Actions</h2>

<p>Many events have a default action. For instance, if you click on a hyperlink, it will navigate to the target page by default. If you click on a form submission button, it will submit the form to the server by default.</p>

<p>If there is an event listener for that event, the event listener will fire first before the default action is run. The event listener can choose to prevent the action from being run. This can be useful in various ways. If you&#8217;re writing a AJAX-y application, you can make it such that when a user clicks a &#8220;next page&#8221; link, instead of navigating the next page, . If you&#8217;re writing a form validation script, you can prevent the form from submitting if errors were detected.</p>

<p>There are two ways to to prevent the default action from happening. First, the event object that is passed as a parameter has a method called <code>preventDefault</code>. If the event listener calls the method, the default action will be cancelled.</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>preventDefaultFn</span> <span class='o'>=</span> <span class='kd'>function</span><span class='p'>(</span><span class='nx'>e</span><span class='p'>)</span> <span class='p'>{</span> <span class='nx'>e</span><span class='p'>.</span><span class='nx'>preventDefault</span><span class='p'>()</span> <span class='p'>};</span>
<span class='nb'>document</span><span class='p'>.</span><span class='nx'>getElementById</span><span class='p'>(</span><span class='s2'>&quot;disabled_link&quot;</span><span class='p'>).</span><span class='nx'>addEventListener</span><span class='p'>(</span><span class='s2'>&quot;click&quot;</span><span class='p'>,</span> <span class='nx'>preventDefaultFn</span><span class='p'>,</span> <span class='kc'>false</span><span class='p'>);</span>
</code></pre></div>
<p>Second, if the listener returns the value <code>false</code>, the default action will be cancelled.</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>preventDefaultFn</span> <span class='o'>=</span> <span class='kd'>function</span><span class='p'>()</span> <span class='p'>{</span> <span class='k'>return</span> <span class='kc'>false</span> <span class='p'>};</span>
<span class='nb'>document</span><span class='p'>.</span><span class='nx'>getElementById</span><span class='p'>(</span><span class='s2'>&quot;disabled_link&quot;</span><span class='p'>).</span><span class='nx'>addEventListener</span><span class='p'>(</span><span class='s2'>&quot;click&quot;</span><span class='p'>,</span> <span class='nx'>preventDefaultFn</span><span class='p'>,</span> <span class='kc'>false</span><span class='p'>);</span>
</code></pre></div>
<h2 id='event_propagation'>Event Propagation</h2>

<p>Suppose a page contained input button element contained within a <code>p</code> element, contained within a <code>div</code> element. If you clicked on the button element, which click handler(s) should fire: the click handler for the button, the <code>p</code> element or the <code>div</code> element? The answer is all of them.</p>

<p>When you click on the button, the event handler on the button fires first. Next, the event handler on the parent <code>p</code> element is invoked. Finally, the event handlers on the parent <code>div</code> element is invoked.</p>

<p>This behavior is known as <em>event propagation</em> or <em>event bubbling</em>. When an event is fired, the element&#8217;s event handlers are first fired. The element&#8217;s parent&#8217;s event handlers are fired. That element&#8217;s parent&#8217;s event handlers are fired, and so on, until the root Document object is reached. Most DOM events exhibit event propagation.</p>

<p>When an event handler is fired, it has an opportunity to stop the event from propogating to its parent. To do so, the handler can call <code>event.stopPropagation()</code> method on the event object.</p>
    </div>

</body>
</html>

