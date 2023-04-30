# Populating the page: how browsers work (Part 1)

## 1. Introduction

- In my previous presentation, I covered why it is important to improve browser performance, important issues related to web performance, and what happens in the network when loading a web page.
- In this session, I will talk about the **Critical Rendering Path**. To briefly introduce what the Critical Rendering Path is, the sequence of steps that a browser takes to convert HTML, CSS, and JavaScript into a rendered web page on the user's screen.

<br/>

## 2. Critical Rendering Path

![image](https://user-images.githubusercontent.com/85419343/235330520-f5c2381d-a50a-412a-afb8-2802a89bdf59.png)

- The Critical Rendering Path is the sequence of steps the browser goes through to convert the HTML, CSS and JavaScript into pixels on the screen.
- Optimizing the critical render path improves render performance.
- The critical rendering path includes the Document Object Model (DOM), CSS Object Model (CSSOM), render tree, layout, and paint.

<br/>

## 3. Parsing

> Parsing is the step the browser takes to turn the data into the DOM and CSSOM.

- Once the browser receives the first chunk of data, it can begin parsing the information received.
- Before anything is rendered to the screen, **the HTML, CSS, and JavaScript have to be parsed**.

### 3-1. DOM

> The first step of parsing is processing the HTML markup and building the DOM tree.

- DOM: the internal representation of the markup for the browser, exposed, can be manipulated through various APIs in JavaScript

- HTML parsing involves [tokenization](https://developer.mozilla.org/en-US/docs/Web/API/DOMTokenList) and tree construction.
  - The parser parses tokenized input into the document, building up the document tree.
- HTML tokens include start and end tags, as well as attribute names and values.
- If the document is well-formed, parsing it is straightforward and faster.
  ![image](https://user-images.githubusercontent.com/85419343/235330913-85aa1dc9-3b70-4809-aede-06faa54d3f2c.png)
- The DOM tree describes the **content of the document**.
- The [`<html>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/html)element is the first tag and root node of the document tree.
- The tree reflects **the relationships and hierarchies between different tags**.
  - Tags nested within other tags are child nodes.

💡 The greater the number of DOM nodes, the longer it takes to construct the DOM tree.

- When the parser finds **non-blocking resources**, such as an image, the browser will request those resources and continue parsing.
- Parsing can continue when a CSS file is encountered, but `<script>`tags—particularly those without an [`async`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) or `defer` attribute—block rendering, and pause the parsing of HTML.
- Though the browser's preload scanner hastens this process, excessive scripts can still be a significant bottleneck.

### 3-2. CSSOM

> The second step in the critical rendering path is processing CSS and building the CSSOM tree.

![image](https://user-images.githubusercontent.com/85419343/235331067-6392abfa-b5ff-4ccb-a901-0ac30eb12a7a.png)

- The CSS object model is similar to the DOM.
  - The DOM and CSSOM are both trees. They are independent data structures.
  - The browser converts the CSS rules into a map of styles it can understand and work with. As with HTML, the browser needs to convert the received CSS rules into something it can work with.
  - The browser goes through each rule set in the CSS, creating a tree of nodes with parent, child, and sibling relationships based on the CSS selectors.
- CSSOM tree
  - includes styles from the **user agent style sheet**
  - The browser begins with the most general rule applicable to a node and recursively refines the computed styles by applying more specific rules.
    - In other words, it **cascades** the property values.
- Building the CSSOM is very fast
  - the "Recalculate Style" in developer tools shows the total time it takes to parse CSS, construct the CSSOM tree, and recursively calculate computed styles.
  - In terms of web performance optimization, there are lower hanging fruit, **as the total time to create the CSSOM is generally less than the time it takes for one DNS lookup**.

### 3-3. JavaScript Compilation

- While the CSS is being parsed and the CSSOM created, other assets, including JavaScript files, are downloading (thanks to the preload scanner).
- JavaScript is interpreted, compiled, parsed, and executed.
- The scripts are parsed into **abstract syntax trees(AST)**.

<br />

## 4. Render

> Rendering steps include style, layout, paint and, in some cases, compositing.

### 4-1. Render Tree

> The third step in the critical rendering path is combining the DOM and CSSOM into a render tree.

![image](https://user-images.githubusercontent.com/85419343/235331165-b86c65d6-dbde-4fc6-91e6-a5f7809d2abe.png)

- The CSSOM and DOM trees created in the parsing step are combined into a **render tree**.
  - Used to compute the layout of every visible element → painted to the screen
- **In some cases, content can be promoted to its own layers and composited, improving performance by painting portions of the screen on the GPU instead of the CPU, freeing up the main thread.**

### 4-2. Layout

### 4-3. Paint

---

### Reference

- https://guillermo.at/browser-critical-render-path
- https://developer.mozilla.org/en-US/docs/Web/Performance/Critical_rendering_path
