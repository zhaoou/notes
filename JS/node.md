<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <link href='http://fonts.googleapis.com/css?family=Source+Sans+Pro:400,700,400italic,700italic|Source+Code+Pro:400,700' rel='stylesheet' type='text/css'>
    <link rel="stylesheet" href="http://www.stanford.edu/~maiyifan/introtojs/css/typography.css">
    <link rel="stylesheet" href="http://www.stanford.edu/~maiyifan/introtojs/codemirror/lib/codemirror.css">
    <link rel="stylesheet" href="http://www.stanford.edu/~maiyifan/introtojs/css/code.css">
</head>
<body>
    <div class="wrapper">
        <h1 id='nodejs'>Node.js</h1>

<h2 id='what_is_nodejs'>What is Node.js?</h2>

<p>Node.js is a software package consisting of a JavaScript engine and a number asynchronous input / output (I/O) libraries. It allows you to run JavaScript on the command line, outside of your browser.</p>

<p>The JavaScript engine used is the V8 JavaScript engine, which also powers the Chrome Browser. The I/O libraries allow Node.js to interface with files or network devices in an asynchronous manner. This allows Node.js to be used as a fast, lightweight, event-based web server.</p>

<h2 id='up_and_running'>Up and Running</h2>

<p>To get started, downloading the Node.js package for your platform on the <a href='http://www.nodejs.org/'>Node.js website</a>. You can also get Node.js through your favorite package manager on Mac OS X or Linux distro.</p>

<p>After you&#8217;ve installed Node.js, run it by opening a command prompt and typing <code>node</code>. You get dropped into a REPL. The node.js REPL works exactly the same as Chrome&#8217;s REPL, so you can run any of your JavaScript commands here.</p>

<p>Node.js can also be used to run files. Try creating a file called <code>hello.js</code> containing <code>console.log(&quot;hello world&quot;)</code>. Now run <code>node hello.js</code>. Node.js will open the file, run your program and exit.</p>

<h2 id='loading_modules'>Loading modules</h2>

<p>Node.js provides a mechanism for importing external libraries using the command <code>require</code>.</p>

<p>Earlier we saw that a JavaScript library should export a single global object, and that all functionality should be provided as properties or objects of that object. For instance, the jQuery library exports a single object that is named <code>jQuery</code> or <code>$</code>, and the d3 library exports a single object that is named <code>d3</code>.</p>

<p>Node.js uses that idea. To load a module, use the <code>require</code> command:</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>http</span> <span class='o'>=</span> <span class='nx'>require</span><span class='p'>(</span><span class='s2'>&quot;http&quot;</span><span class='p'>);</span>
</code></pre></div>
<p>This loads the http library and the single exported object available through the <code>http</code> variable. By convention, the variable name is the same as the module name, but you can also pick any variable name you like, e.g. <code>var otherName = require(&quot;http&quot;)</code>.</p>

<p>The system that Node.js uses behind the scenes to load modules is called CommonJS. If you write your own library for node, your code should conform to the CommonJS specifications.</p>

<h2 id='installing_modules'>Installing modules</h2>

<p>Where do modules come from? Some modules, such as <code>http</code>, are installed as part of the standard Node.js library. Other user-contributed modules have to be downloaded and installed. Recent versions of Node.js come with a package manager called npm, which allows you to download and install modules easily. To install a package, run <code>npm install package_name</code> on the command line.</p>

<p>Another way of installing modules is by installing them as part of the application. If you place a file called package.json in the same folder as your application, and then run <code>npm install</code> in that folder, npm will extract the list of dependencies from package.json, and then install them locally in the application. Here&#8217;s what a sample package.json file will look like:</p>
<div class='highlight'><pre><code class='js'><span class='p'>{</span>
	<span class='s2'>&quot;name&quot;</span><span class='o'>:</span> <span class='s2'>&quot;chirps&quot;</span><span class='p'>,</span>
	<span class='s2'>&quot;description&quot;</span><span class='o'>:</span> <span class='s2'>&quot;A social media website for sharing short messages&quot;</span><span class='p'>,</span>
	<span class='s2'>&quot;version&quot;</span><span class='o'>:</span> <span class='s2'>&quot;0.0.1&quot;</span><span class='p'>,</span>
	<span class='s2'>&quot;engines&quot;</span><span class='o'>:</span> <span class='p'>{</span>
		<span class='s2'>&quot;node&quot;</span><span class='o'>:</span> <span class='s2'>&quot;0.8.x&quot;</span><span class='p'>,</span>
		<span class='s2'>&quot;npm&quot;</span><span class='o'>:</span> <span class='s2'>&quot;1.2.x&quot;</span>
	<span class='p'>},</span>
	<span class='s2'>&quot;dependencies&quot;</span><span class='o'>:</span> <span class='p'>{</span>
		<span class='s2'>&quot;express&quot;</span><span class='o'>:</span> <span class='s2'>&quot;3.2.x&quot;</span><span class='p'>,</span>
		<span class='s2'>&quot;ejs&quot;</span><span class='o'>:</span> <span class='s2'>&quot;0.8.x&quot;</span>
	<span class='p'>}</span>
<span class='p'>}</span>
</code></pre></div>
<h2 id='your_first_http_server'>Your First HTTP Server</h2>

<p>Let&#8217;s create a HTTP server! Copy the following code (taken from the Node.js website) into a file, and then run it using Node.js.</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>http</span> <span class='o'>=</span> <span class='nx'>require</span><span class='p'>(</span><span class='s1'>&#39;http&#39;</span><span class='p'>);</span>
<span class='nx'>http</span><span class='p'>.</span><span class='nx'>createServer</span><span class='p'>(</span><span class='kd'>function</span> <span class='p'>(</span><span class='nx'>req</span><span class='p'>,</span> <span class='nx'>res</span><span class='p'>)</span> <span class='p'>{</span>
	<span class='nx'>res</span><span class='p'>.</span><span class='nx'>writeHead</span><span class='p'>(</span><span class='mi'>200</span><span class='p'>,</span> <span class='p'>{</span><span class='s1'>&#39;Content-Type&#39;</span><span class='o'>:</span> <span class='s1'>&#39;text/plain&#39;</span><span class='p'>});</span>
	<span class='nx'>res</span><span class='p'>.</span><span class='nx'>end</span><span class='p'>(</span><span class='s1'>&#39;Hello World\n&#39;</span><span class='p'>);</span>
<span class='p'>}).</span><span class='nx'>listen</span><span class='p'>(</span><span class='mi'>1337</span><span class='p'>,</span> <span class='s1'>&#39;127.0.0.1&#39;</span><span class='p'>);</span>
<span class='nx'>console</span><span class='p'>.</span><span class='nx'>log</span><span class='p'>(</span><span class='s1'>&#39;Server running at http://127.0.0.1:1337/&#39;</span><span class='p'>);</span>
</code></pre></div>
<p>The code first loads the HTTP library. It then creates a new server, and sets a callback function that will be called whenever a browser connects to the server. The server is then set to listen to incoming connections at <code>127.0.0.1:1337</code> using the <code>listen</code> command.</p>

<p>Here&#8217;s how the callback function works. The server will call the provided callback function whenever it receives a request, and it will pass in two useful objects as parameters. The callback function is passed <code>req</code>, the request object, and <code>res</code>, the response object. <code>req</code> contains information about the browser&#8217;s request, such as the path requested. <code>res</code> is an object that should be used to send the server&#8217;s response using the <code>writeHead</code> and <code>end</code> methods. In the example above, the callback method simply prints the words &#8220;Hello World&#8221; back to the browser.</p>

<p>The problem with the HTTP library is that it is very low level. It allows you to operate directly on the HTTP request and response objects, but it doesn&#8217;t give you any higher-level abstrations that would help you build a web application.</p>

<p>Here are some drawbacks. You only get to specify a single callback function. You have to retrieve the URL from the <code>req</code> parameter and match it with your list of available URLs manually. You need to set response headers manually.</p>

<h2 id='web_frameworks'>Web Frameworks</h2>

<p>A web framework is a collection of tools and libraries designed for web applications. A web framework can make our life easier by handling common functionality that is used in web applications such as routing, templating, sessions, etc. Examples of frameworks are Rails and Django.</p>

<p>A web microframework is like a web framework, but provides a more minimalistic set of tools. Examples include Sinatra and Flask. Node.js has its own popular microframework called Express, which will see next.</p>

<h2 id='express'>Express</h2>

<p>Here&#8217;s what the same application looks like in Express.</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>express</span> <span class='o'>=</span> <span class='nx'>require</span><span class='p'>(</span><span class='s2'>&quot;express&quot;</span><span class='p'>);</span>
<span class='kd'>var</span> <span class='nx'>app</span> <span class='o'>=</span> <span class='nx'>express</span><span class='p'>();</span>
<span class='nx'>app</span><span class='p'>.</span><span class='nx'>get</span><span class='p'>(</span><span class='s2'>&quot;/&quot;</span><span class='p'>,</span> <span class='kd'>function</span><span class='p'>(</span><span class='nx'>req</span><span class='p'>,</span> <span class='nx'>res</span><span class='p'>)</span> <span class='p'>{</span>
	<span class='nx'>res</span><span class='p'>.</span><span class='nx'>send</span><span class='p'>(</span><span class='s2'>&quot;Hello world!&quot;</span><span class='p'>);</span>
	<span class='nx'>res</span><span class='p'>.</span><span class='nx'>end</span><span class='p'>();</span>
<span class='p'>}).</span><span class='nx'>listen</span><span class='p'>(</span><span class='mi'>1337</span><span class='p'>);</span>
<span class='nx'>console</span><span class='p'>.</span><span class='nx'>log</span><span class='p'>(</span><span class='s1'>&#39;Server running at http://127.0.0.1:1337/&#39;</span><span class='p'>);</span>
</code></pre></div>
<p>In this example, we loaded the Express module, created an application, defined a new route, and then made the server listen for new connections.</p>

<p>The application is very similar to the old server, but notice the improvements. We can now specify that our handler should handle only the GET method on the &#8221;/&#8221; url. We also got rid of having to fiddle around with the headers on the response.</p>

<h2 id='routes'>Routes</h2>

<p>We can create more routes for the application using the <code>get</code> and <code>post</code> methods. <code>get</code> creates routes for the HTTP GET method, while <code>post</code> creates routes for the HTTP POST method. Both <code>get</code> and <code>post</code> take the URL as the first parameter and a callback function as the second. The callback function is passed <code>req</code>, the request object, and <code>res</code>, the response object. <code>req</code> contains information about the browser&#8217;s request, such as the path requested. <code>res</code> is an object that should be used to send the server&#8217;s response using the <code>send</code> and <code>end</code> methods.</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>express</span> <span class='o'>=</span> <span class='nx'>require</span><span class='p'>(</span><span class='s2'>&quot;express&quot;</span><span class='p'>);</span>
<span class='kd'>var</span> <span class='nx'>app</span> <span class='o'>=</span> <span class='nx'>express</span><span class='p'>();</span>

<span class='nx'>app</span><span class='p'>.</span><span class='nx'>get</span><span class='p'>(</span><span class='s2'>&quot;/hello&quot;</span><span class='p'>,</span> <span class='kd'>function</span><span class='p'>(</span><span class='nx'>req</span><span class='p'>,</span> <span class='nx'>res</span><span class='p'>)</span> <span class='p'>{</span>
	<span class='nx'>res</span><span class='p'>.</span><span class='nx'>send</span><span class='p'>(</span><span class='s2'>&quot;Hello!&quot;</span><span class='p'>);</span>
	<span class='nx'>res</span><span class='p'>.</span><span class='nx'>end</span><span class='p'>();</span>
<span class='p'>});</span>

<span class='nx'>app</span><span class='p'>.</span><span class='nx'>get</span><span class='p'>(</span><span class='s2'>&quot;/world&quot;</span><span class='p'>,</span> <span class='kd'>function</span><span class='p'>(</span><span class='nx'>req</span><span class='p'>,</span> <span class='nx'>res</span><span class='p'>)</span> <span class='p'>{</span>
	<span class='nx'>res</span><span class='p'>.</span><span class='nx'>send</span><span class='p'>(</span><span class='s2'>&quot;World!&quot;</span><span class='p'>);</span>
	<span class='nx'>res</span><span class='p'>.</span><span class='nx'>end</span><span class='p'>();</span>
<span class='p'>});</span>

<span class='nx'>app</span><span class='p'>.</span><span class='nx'>listen</span><span class='p'>(</span><span class='mi'>1337</span><span class='p'>);</span>
<span class='nx'>console</span><span class='p'>.</span><span class='nx'>log</span><span class='p'>(</span><span class='s1'>&#39;Server running at http://127.0.0.1:1337/&#39;</span><span class='p'>);</span>
</code></pre></div>
<h2 id='parameters'>Parameters</h2>

<p>You can get query parameters in the callback functions. For instance, you can access GET parameters through the <code>req.query</code> object. Consider the following server code:</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>express</span> <span class='o'>=</span> <span class='nx'>require</span><span class='p'>(</span><span class='s2'>&quot;express&quot;</span><span class='p'>);</span>
<span class='kd'>var</span> <span class='nx'>app</span> <span class='o'>=</span> <span class='nx'>express</span><span class='p'>();</span>
<span class='nx'>app</span><span class='p'>.</span><span class='nx'>get</span><span class='p'>(</span><span class='s2'>&quot;/&quot;</span><span class='p'>,</span> <span class='kd'>function</span><span class='p'>(</span><span class='nx'>req</span><span class='p'>,</span> <span class='nx'>res</span><span class='p'>)</span> <span class='p'>{</span>
	<span class='nx'>res</span><span class='p'>.</span><span class='nx'>send</span><span class='p'>(</span><span class='s2'>&quot;Hello, &quot;</span> <span class='o'>+</span> <span class='nx'>req</span><span class='p'>.</span><span class='nx'>query</span><span class='p'>.</span><span class='nx'>name</span> <span class='o'>+</span> <span class='s2'>&quot;!&quot;</span><span class='p'>);</span>
	<span class='nx'>res</span><span class='p'>.</span><span class='nx'>end</span><span class='p'>();</span>
<span class='p'>}).</span><span class='nx'>listen</span><span class='p'>(</span><span class='mi'>1337</span><span class='p'>);</span>
<span class='nx'>console</span><span class='p'>.</span><span class='nx'>log</span><span class='p'>(</span><span class='s1'>&#39;Server running at http://127.0.0.1:1337/&#39;</span><span class='p'>);</span>
</code></pre></div>
<p>If you run the server and visit <code>http://127.0.0.1:1337/?name=John</code>, you will see that the page changes to reflect the value of the name parameter.</p>

<h2 id='static_files'>Static Files</h2>

<p>We can use Express as a static file server as follows:</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>express</span> <span class='o'>=</span> <span class='nx'>require</span><span class='p'>(</span><span class='s2'>&quot;express&quot;</span><span class='p'>);</span>
<span class='kd'>var</span> <span class='nx'>ejs</span> <span class='o'>=</span> <span class='nx'>require</span><span class='p'>(</span><span class='s2'>&quot;ejs&quot;</span><span class='p'>);</span>
<span class='nx'>app</span><span class='p'>.</span><span class='nx'>use</span><span class='p'>(</span><span class='nx'>express</span><span class='p'>.</span><span class='kr'>static</span><span class='p'>(</span><span class='nx'>__dirname</span> <span class='o'>+</span> <span class='s2'>&quot;/public&quot;</span><span class='p'>));</span>
<span class='nx'>app</span><span class='p'>.</span><span class='nx'>listen</span><span class='p'>(</span><span class='mi'>1337</span><span class='p'>);</span>
</code></pre></div>
<p>This tells Express to use <code>/public</code> as the directory from which files are served. If the server receives a request that does not match any of the routes, but matches a file in that directory, Express will respond by sending that file. For instance, if we create a file at <code>public/hello.html</code>, then we can view it by going to <code>http://127.0.0.1:1337/hello.html</code>.</p>

<h2 id='views'>Views</h2>

<p>Views allow us to specify a template for a page, and then dynamically fill in information to the page.</p>
<div class='highlight'><pre><code class='js'><span class='kd'>var</span> <span class='nx'>express</span> <span class='o'>=</span> <span class='nx'>require</span><span class='p'>(</span><span class='s2'>&quot;express&quot;</span><span class='p'>);</span>
<span class='kd'>var</span> <span class='nx'>app</span> <span class='o'>=</span> <span class='nx'>express</span><span class='p'>();</span>

<span class='nx'>app</span><span class='p'>.</span><span class='nx'>set</span><span class='p'>(</span><span class='s2'>&quot;view engine&quot;</span><span class='p'>,</span> <span class='s2'>&quot;ejs&quot;</span><span class='p'>);</span>
<span class='nx'>app</span><span class='p'>.</span><span class='nx'>set</span><span class='p'>(</span><span class='s2'>&quot;views&quot;</span><span class='p'>,</span> <span class='nx'>__dirname</span> <span class='o'>+</span> <span class='s2'>&quot;/views&quot;</span><span class='p'>);</span>

<span class='nx'>app</span><span class='p'>.</span><span class='nx'>get</span><span class='p'>(</span><span class='s2'>&quot;/&quot;</span><span class='p'>,</span> <span class='kd'>function</span><span class='p'>(</span><span class='nx'>req</span><span class='p'>,</span> <span class='nx'>res</span><span class='p'>)</span> <span class='p'>{</span>
	<span class='nx'>res</span><span class='p'>.</span><span class='nx'>render</span><span class='p'>(</span><span class='s2'>&quot;hello&quot;</span><span class='p'>,</span> <span class='p'>{</span><span class='s2'>&quot;greeting&quot;</span><span class='o'>:</span> <span class='nx'>Hello</span> <span class='nx'>world</span><span class='o'>!</span><span class='p'>});</span>
<span class='p'>}).</span><span class='nx'>listen</span><span class='p'>(</span><span class='mi'>1337</span><span class='p'>);</span>

<span class='nx'>console</span><span class='p'>.</span><span class='nx'>log</span><span class='p'>(</span><span class='s1'>&#39;Server running at http://127.0.0.1:1337/&#39;</span><span class='p'>);</span>
</code></pre></div>
<p>We use the <code>set</code> command to select <a href='http://embeddedjs.com/'>EJS</a> as the templating engine. there are other templating engines engines available, such as Jade, but we pick EJS because it is the simplest. We then set the folder to the <code>/views</code> directory.</p>

<p>In the handler for <code>/</code>, we simply call the render method, specifying the name of the template to use, and the data that should be passed to the template. Any data that is passed to the template will become local variables within the variables.</p>

<p>The contents of <code>/views/hello.ejs</code> are as follows. Notice the use of the <code>&lt;%= greet %&gt;</code> tag, which causes the value of <code>greet</code> to be printed at the appropriate position:</p>
<div class='highlight'><pre><code class='html'><span class='cp'>&lt;!DOCTYPE html&gt;</span>
<span class='nt'>&lt;html</span> <span class='na'>lang=</span><span class='s'>&quot;en&quot;</span><span class='nt'>&gt;</span>
<span class='nt'>&lt;head&gt;</span>
	<span class='nt'>&lt;meta</span> <span class='na'>charset=</span><span class='s'>&quot;utf-8&quot;</span><span class='nt'>&gt;</span>
	<span class='nt'>&lt;title&gt;</span>Hello World<span class='nt'>&lt;/title&gt;</span>
<span class='nt'>&lt;/head&gt;</span>
<span class='nt'>&lt;body&gt;</span>
	<span class='nt'>&lt;h1&gt;</span><span class='err'>&lt;</span>%= greet %&gt;<span class='nt'>&lt;/h1&gt;</span>
<span class='nt'>&lt;/body&gt;</span>
<span class='nt'>&lt;/html&gt;</span>
</code></pre></div>
    </div>

</body>
</html>

