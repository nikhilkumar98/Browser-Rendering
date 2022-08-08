# Browser-Rendering
                                    An overview of how browsers render websites

Critical Rendering path is the sequence of actions or steps performed by the browser in order to render a web page on the screen. These steps include fetching resources (HTML, CSS, JavaScript) from the server, parsing received HTML to create DOM, parsing CSS to create CSSOM, creating Render Tree which is the combination of DOM and CSSOM, calculating positioning and dimensions (Layout) of all elements, and finally Painting actual pixels on the screen.

How exactly do browsers render websites? I’ll deconstruct the process shortly, but first, it’s important to recap some basics.

A web browser is a piece of software that loads files from a remote server (or perhaps a local disk) and displays them to you — allowing for user interaction. 

However, within a browser, there’s a piece of software that figures out what to display to you based on the files it receives. This is called the browser engine.

The browser engine is a core software component of every major browser, and different browser manufacturers call their engines by different names. The browser engine for Firefox is called Gecko, and Chrome’s is called Blink, which happens to be a fork of WebKit.

You can have a look at a comparison of the various browser engines if that interests you. Don’t let the names confuse you — they are just names.

The point I’m trying to make is that when you write some HTML, CSS, and JS, and attempt to open the HTML file in your browser, the browser reads the raw bytes of HTML from your hard disk (or network).

<!DOCTYPE html>
<html>

<head>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <title>Medium Article Demo</title>
    <link rel="stylesheet" href="style.css">
</head>

<body>
    <p id="header">How Browser Rendering Works</p>
    <div><img src="https://i.imgur.com/jDq3k3r.jpg">
</body>

</html>

A simple text and image are rendered on the screen. From previous explanations, the browser reads raw bytes of the HTML file from the disk (or network) and transforms that into characters.


 # How JavaScript works:
 
 
The rendering engine and tips to optimize its performance
When you’re building web apps, however, you don’t just write isolated JavaScript code that runs on its own. The JavaScript you write is interacting with the environment. 
Understanding this environment, how it works and what it is composed of will allow you to build better apps and be well-prepared for potential issues that might arise once your apps are released into the wild.


                                          So, let’s see what the browser main components are:
                                          
 * User interface: 
This includes the address bar, the back and forward buttons, bookmarking menu, etc. In essence, this is every part of the browser display except for the window where you see the web page itself.

 * Browser engine: 
it handles the interactions between the user interface and the rendering engine

 * Rendering engine:
it’s responsible for displaying the web page. The rendering engine parses the HTML and the CSS and displays the parsed content on the screen.

 * Networking: 
 these are network calls such as XHR requests, made by using different implementations for the different platforms, which are behind a platform-independent interface. 
 
                              We talked about the networking layer in more detail in a previous post of this series.
 * UI backend: 
it’s used for drawing the core widgets such as checkboxes and windows. This backend exposes a generic interface that is not platform-specific.
 It uses operating system UI methods underneath.
 * JavaScript engine:
We’ve covered this in great detail in a previous post from the series. Basically, this is where the JavaScript gets executed.
 * Data persistence:
your app might need to store all data locally. The supported types of storage mechanisms include localStorage, indexDB, WebSQL and FileSystem.

 # Document Object Model:
 
After fetching HTML from the server, the browser starts parsing it and creating DOM, This process itself is divided into many further steps that are needed to be performed in order to create the DOM tree, these steps include, converting received HTML bytes into Tokens, creating nodes using these tokens and then connecting these nodes to create DOM Tree.

![image](https://user-images.githubusercontent.com/110593422/183360195-ff32b357-1a40-4091-a79f-a523a9c366b5.png)

 # Creating Tokens:

The browser starts reading Fetched HTML character by character and creates a token whenever an HTML tag is encountered. There are two types of tokens, StartTag Token, and EndDate token, as the names suggest StartTag token is created when an HTML start (opening) tag is encountered, and EndTag token is created when any HTML end (closing) tag is encountered.

Following tokens will be created after parsing the HTML shown in the example above

![image](https://user-images.githubusercontent.com/110593422/183360892-71569214-be7c-4ba0-ac41-5bb438f663c6.png)

 # Creating Nodes and DOM Tree

Tokens created in the first step are used to create Nodes for every HTML tag. Nodes contain the complete information of that particular tag, like attributes, classes, etc.

The sequence of the Tokens is also very important as it is used to identify the relationship between the nodes i.e if a node is a child of another node or not. Once all the tags are parsed and nodes are created, the next step is to connect those nodes by identifying the relationship between them using their sequence.

In Tokens created in the previous step, StartTag and EndTag of span come between StartTag and EndTag of div which means span Node will be a child of div Node in the DOM tree as shown below.

![image](https://user-images.githubusercontent.com/110593422/183361044-a1736ed4-2a90-4ada-80c8-a12506bcdcf6.png)

 # CSS Object Model (CSSOM):
 
While parsing HTML, If the browser encounters any style tag or link tag referencing a stylesheet, it starts fetching (in case of external CSS) and parsing those styles, and converting them into a CSS Object model which contains all the information about the page styles.

The CSSOM creation process is similar to DOM creation, i.e, CSS bytes are first converted into characters, then characters are converted into tokens, then nodes and finally we get CSSOM after linking those nodes.

![image](https://user-images.githubusercontent.com/110593422/183361260-3c8e1b39-3a75-4e0b-8ec4-14fba0c008ac.png)

Every style in the parent node is also applied to the child node, as you can see in the example above, the span node has font-size property inherited from the parent div node.

 # Render Tree:
 
The render tree is the combination of DOM and CSSOM. In order to create Render Tree, the browser starts traversing the DOM and looks for the matching styles in CSSOM, then these two are combined to create a render tree node that contains both content and styles information.

![image](https://user-images.githubusercontent.com/110593422/183361493-0d001276-1d37-4857-abbd-778246a1da43.png)

Render Tree contains only nodes that will be visible on the screen, for example, nodes of the tags like meta, script, link, etc, or nodes that are hidden using CSS styles (like display: none), are not included.

 # Reference:
 
https://javascript.plainenglish.io/web-performance-understanding-critical-rendering-path-72283caefc1f

https://blog.logrocket.com/how-browser-rendering-works-behind-scenes/



