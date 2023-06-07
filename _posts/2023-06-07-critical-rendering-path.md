---
layout: post
usehighlight: true
tags: [JavaScript, HTML, CSS, Web Browser, Frontend]
title: The critical rendering path of web browser
---

## Overview

The critical rendering path refers to the series of steps that a web browser takes to render and display a web page on the user's screen. It involves fetching, parsing, and rendering various resources such as HTML, CSS, JavaScript, and images.

Here is a simplified overview of the critical rendering path:

- HTML Parsing: The browser starts by fetching the HTML file of the web page from the server. It parses the HTML document and builds the Document Object Model (DOM) tree, representing the structure of the page.

- CSS Parsing and Rendering: As the browser encounters CSS stylesheets in the HTML, it fetches and parses them. It creates the CSS Object Model (CSSOM) tree, which represents the styles applied to the elements in the DOM tree. The DOM and CSSOM are then combined to form the render tree.

- Layout (or Reflow): The browser determines the layout of the elements based on their styles and dimensions. It calculates the position and size of each element on the page.

- Painting: The browser paints the pixels on the screen according to the layout information. It converts the render tree into actual pixels by traversing the tree and drawing the elements on the screen.

- Compositing: If there are overlapping elements or layers with transparency, the browser performs compositing to blend the different layers and create the final visual result.

- Display: The rendered page is displayed on the user's screen.

It's important to optimize the critical rendering path to ensure a fast and smooth user experience. Techniques like minification and compression of resources, reducing the number of server requests, asynchronous loading of scripts, and utilizing browser caching can help improve the overall performance of web page rendering.

## Document Object Model (DOM)

The Document Object Model (DOM) is a programming interface for web documents. It represents the structure and content of an HTML or XML document as a tree-like structure, where each node in the tree represents an element, attribute, or piece of text. Here's an overview of its structure:

- Document Node: At the top of the DOM tree is the document node, which represents the entire document. It serves as the entry point to access and manipulate the document's content.

- Element Nodes: The document node has child nodes that are element nodes. Element nodes represent HTML or XML tags in the document. They have properties such as tag name, attributes, and child nodes.

- Attribute Nodes: Attributes of an element are represented as attribute nodes. Attribute nodes contain information about the attribute's name and value. They are associated with the corresponding element nodes.

- Text Nodes: Text nodes represent the textual content within an element. For example, the text between HTML tags or the content of an XML element. Text nodes do not have child nodes and are leaf nodes in the DOM tree.

- Other Node Types: Besides element, attribute, and text nodes, the DOM tree may include other types of nodes such as comment nodes (representing HTML or XML comments) and processing instruction nodes (representing processing instructions in XML).

The DOM tree is constructed by the browser when parsing an HTML or XML document. It starts with a root node, called the document node, which represents the entire document. The document node has child nodes that correspond to the different elements and text within the document.

Each node in the DOM tree represents an individual element, attribute, or text node within the document. Elements are represented by element nodes, which contain information such as the element's tag name, attributes, and child nodes. Attribute nodes represent the attributes of elements, containing information such as the attribute's name and value. Text nodes represent the text content within an element.

The DOM tree follows a hierarchical structure, where elements can have child elements, forming a parent-child relationship. This hierarchical structure allows for easy traversal and manipulation of the document's contents using various DOM methods and properties.

Nodes in the DOM tree can be accessed, modified, or manipulated through scripting languages such as JavaScript. Developers can use DOM APIs to dynamically add, remove, or modify elements and attributes, as well as handle events and interact with the document structure and content.

The DOM provides a standardized way for web browsers to interact with web documents, enabling dynamic manipulation of document elements and content. It serves as a bridge between the structure and content of a document and the programming environment, allowing developers to create interactive web applications and websites.

Nodes in the DOM tree can be accessed and manipulated using DOM APIs provided by programming languages such as JavaScript. These APIs offer methods and properties to traverse the tree, retrieve and modify node attributes and text content, create new nodes, remove nodes, and handle events associated with the nodes.

By understanding the structure of the DOM tree, developers can programmatically interact with the elements and content of a web page, enabling dynamic modifications and interactivity.

## CSS Object Model (CSSOM)

The CSS Object Model (CSSOM) tree is a representation of the CSS styles applied to the elements in the Document Object Model (DOM) tree. It provides a structured way to access and manipulate the CSS properties of elements on a web page. The CSSOM tree is constructed by the browser during the rendering process. Here's an overview of its structure:

- Stylesheets: The CSSOM tree starts with a list of stylesheets associated with the web page. Each stylesheet represents a CSS file that is either embedded within the HTML or linked externally.

- Rules: Stylesheets contain rules that define the styles to be applied to specific elements. Each rule consists of a selector and a set of declarations. Selectors specify which elements the rule applies to, and declarations define the CSS properties and their values.

- Selectors: Selectors target elements in the DOM tree based on various criteria such as element names, class names, IDs, attributes, and pseudo-classes. They determine which elements will receive the styles defined in the associated rule.

- Declarations: Declarations within a rule specify the CSS properties and their values. Each declaration consists of a property and a value. Multiple declarations within a rule collectively define the styles to be applied to the selected elements.

- Specificity and Inheritance: The CSSOM tree also takes into account specificity and inheritance. Specificity determines which styles take precedence when multiple rules target the same element. Inheritance determines whether a property value is inherited by child elements or not.

The CSSOM tree is closely related to the DOM tree, as they both represent different aspects of the web page. When rendering the page, the browser combines the DOM tree and CSSOM tree to create the render tree, which is then used for layout and painting.

By accessing and manipulating the CSSOM tree, developers can dynamically modify the styles applied to elements, retrieve computed styles, or perform other operations related to CSS styling on the page.
