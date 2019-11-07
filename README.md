# Intro to Express

## Prerequisites

- JavaScript
- APIs

## Objectives

By the end of this lesson, developers should be able to:

- Define what an application framework is
- Build a simple server side application with Express

## What is a Framework?

Express is a framework for building web applications. What does that mean?

In development, a framework is a collection of tools, patterns, and conventions
that let you perform some task quickly and efficiently.

There are lots of different kinds of frameworks for different kinds of tasks.
There are frameworks for building command line applications, frameworks for
deploying applications to the cloud, and frameworks for running tests against an
application to make sure everything is working as it's supposed to.

Express is a framework for building _web applications_, meaning it's a set of
tools, patterns, and conventions for building applications for the internet.

It works within the context of the internet so that when you navigate to a URL
in your browser, an app built with Express can handle that URL and send you some
response (like an HTML file).

## Express

Express is a minimalistic web framework. Compared to web frameworks like Django
and Ruby on Rails, Express is tiny.

But it was intentionally designed that way. Throughout Express' history and
development, the core of the web framework has gotten smaller as more and more
functionality is spun-off into separate packages.

Express feels "close to the wire" - i.e. you will be building out the
functionality that you want. This minimalism comes with some trade-offs.

On the one hand, you won't have unnecessarily complicated code in your
application or things that you don't need. It also means you'll be responsible
for building out everything you do need.

Additionally, Express is very unopinionated: it doesn't really care how you
structure your app, for instance, and doesn't provide any guidance on how to do
so.

That makes it extremely flexible and practical for a lot of different types and
sizes of applications; it also means that you have to figure out the structure
yourself. PayPal uses Express, but built a more opinionated framework
(Kraken.js) on top of it to give its developers more structure.

## Setting up an Express App

1. Create a new directory called "express-hello-world" in your sandbox.
2. Change into the new directory
3. Run `npm init`. Press enter a bunch of times to accept all the default values
   (or just run `npm init -y`)
4. Run the command `npm install express`

### Getting Started

Building our first server is pretty straightforward. Create an `index.js` file
and write the following inside of it:

```js
const express = require("express");
const app = express();

app.listen(4000, () => {
  console.log("app listening on port 4000");
});
```

To start up our server, we just need to execute this file with node:

```sh
node index.js
```

What's going on here?

- We're requiring the Express module, which is a function that returns an
  instance of a web app
- We're invoking the module, instantiating a constant app which holds all the
  methods and state we use to write and run our web app
- We're starting our server (and app) by listening on port 4000 for incoming
  requests

When we run the application from the terminal, `node index.js`, we can see app
listening on port 4000 printed to the terminal. The process continues to run,
occupying the shell until we hit Ctrl + C.

If we visit `http://localhost:4000` in the browser, we'll see something like
this:

```sh
Cannot GET /
```

Our app is running and we're able to visit it in the browser. But we're missing
routes!

### Routing

The first key feature that Express provides is simple and easy routing.

A _route_ is a path and an HTTP method. The path will come from the URL, so if
we visit `http://www.cat-astrophy.com/whiskers` the path will be `/whiskers`.
The HTTP method will be the method we want to accept: `GET`, `POST`, `PUT`, or
`DELETE`.

Express contains a function for each HTTP method, which in turn accepts a path as
the first argument then some number of callback functions. We'll start with just
one callback function.

Let's update `index.js`. Add this above `app.listen()`

```js
app.get("/", (request, response) => {
  response.send("Hello World");
});
```

Let's break down the syntax here.

```js
app.get();
```

`app` is the variable we've declared above. It's an `instance` of the Express
server. `get()` is a function that tells express what `http method` to listen
for.

```js
app.get("/");
```

The first argument that `get()` takes is the `path`. This one is set to the
**root** of wherever our server is listening (which is `http://localhost:4000`).

```js
app.get("/", (request, response) => {});
```

The second argument that `get()` takes is a function. It's how we tell Express
what we want to do when the server receives a GET request at the root `"/"` url.
The preferred syntax is to use arrow functions here, to keep it concise.

In the example above, the callback function is given two arguments: the first
represents the HTTP request object and the second represents the HTTP response
object.

**We always have to send a response**. We do that by using the response variable
that we've declared in the callback. So we end up with a working route!

```js
app.get("/", (request, response) => {
  response.send("Hello World");
});
```

We've added a route and a handler for requests to the `"/"` route. In this case,
we're sending the string `"Hello World"` as the response. Let's see if this
takes effect in the browser:

```
Cannot GET /
```

No change. The running server won't change until we restart it and refresh the
page. Once we do that, we'll see:

```
Hello World
```

Constantly needing to restart the server will get very tedious, very quickly.
Instead, we can use the `nodemon` module to run our server. Instead of requiring
`nodemon` in our code, we use `nodemon` from the command line. Then, `nodemon`
will restart our server for us whenever a file is changed.

To check if you have nodemon, run: `nodemon -v`.

**If you do not already have nodemon installed**

> In the terminal, run: `npm install --global nodemon`

> When using the `--global` flag (or `-g` for short), we're specifying that
> `nodemon` will be installed "globally" so we can utilize `nodemon` across all
> of our node applications (and also that it is not included in our project
> dependencies).

We start up our application a bit differently now:

```sh
nodemon index.js
```

In the browser, we 

### Defining Routes in Express

Defining routes in Express is pretty straightforward. Every route we define is
going to follow this pattern:

```js
router.method(path, controllerAction);
```

Let's break each part of this down:

- `router` - this is going to be an instance of Express (typically a variable
  called `app`) or an instance of the
  [Express Router (`express.Router()`)](https://expressjs.com/en/4x/api.html#router)
- `method` - this is going to be a method of our router object. Luckily, the
  name of the methods are identical to the HTTP method names, meaning the method
  names are: `get`, `post`, `put`, `patch`, and `delete`.
- `path` - this is going to be a string that matches the path of the URL
- `controllerAction` - this is a callback function that handles the request and
  sends a response.

> Read more on the
> [Basic Routing](https://expressjs.com/en/starter/basic-routing.html) page of
> the Express Documentation.

### Identifying the Parts of a Route

An example of a "hello world" route would look like this:

```js
const express = require("express");
const app = express();

app.get("/", function(req, res) {
  res.send("hello world");
});
```

Notice the pattern?

- Where is the `router`?
- Where is the `path`?
- Where is the `method`?
- Where is the controller action?

All routes follow this pattern:

```js
const express = require("express");
const app = express();

app.get("/", function(req, res) {});

app.post("/", function(req, res) {});

app.put("/:id", function(req, res) {});

app.patch("/:id", function(req, res) {});

app.delete("/:id", function(req, res) {});
```

The examples above attach routes directly to the instance of our Express app.
This works great if our app is small and only has a few routes.

The apps we're building are big, so defining our routes like this will quickly
become unmanageable. So we're going to use the
[Express Router](https://expressjs.com/en/4x/api.html#router):

If we used the Author model as the basis of our resource, we can start building
the routes by:

- Creating a directory called `routes` in the root of th express application.
- Creating a `routes/author.js` file and enter the following:

```js
// routes/author.js
const express = require("express");
const router = express.Router();

router.get("/", function(req, res) {
  res.send("author - hello from GET");
});

router.post("/", function(req, res) {
  res.send("author - hello from POST");
});

router.put("/:id", function(req, res) {
  res.send("author:id - hello from PUT");
});

router.patch("/:id", function(req, res) {
  res.send("author:id hello from GET");
});

router.delete("/:id", function(req, res) {
  res.send("author:id - hello from DELETE");
});

module.exports = router;
```

Then inside our `index.js` (or wherever we're actually creating an instance of
our Express app):

```js
const express = require("express");
const app = express();
const authorRouter = require("./routes/author.js");

// add befpre the previous routs
app.use("/author", authorRouter);

// rest of the code
```

## RESTful routing in Express

Notice in the examples above that we're passing in a callback function to the
route method? That is one way to design and implement your routes - but there's
a better way to do it: separate your routes and your controller actions.

In the example above, we're doing our routing and controlling in the same place.
We want to separate them by moving those callback functions. If we think back to
our file system from before: we're going to define our routes inside the
`routes/` directory and our controllers inside the `controllers/` directory. A
controller is just an object made up of methods (the actions in the Resource
Table above). We'll export that object from our `controllers/` directory and
import it into our router. Then, we'll define our routes and map them to the
corresponding controller action.

```js
// controllers/author.js
module.exports = {
  index: function(req, res) {
    // listing all authors
    res.send("listing all authors");
  },
  new: function(req, res) {
    // rending the form to create a new author
    res.send("rending the form to create a new author");
  },
  create: function(req, res) {
    // creating a new author and saving it to the database
    res.send("creating a new author and saving it to the database");
  },
  show: function(req, res) {
    // displaying the data for a single author
    res.send("displaying the data for a single author");
  },
  edit: function(req, res) {
    // rendering the form to update an existing author
    res.send("rendering the form to update an existing author");
  },
  update: function(req, res) {
    // updating an author in the database
    res.send("updating an author in the database");
  },
  destroy: function(req, res) {
    // deleting an author
    res.send("deleting an author");
  }
};
```

```js
// routers/author.js
const express = require("express");
const router = express.Router();
const authorController = require("../controllers/author.js");

router.get("/", authorController.index);
router.get("/new", authorController.new);
router.post("/", authorController.create);
router.get("/:id", authorController.show);
router.get("/:id/edit", authorController.edit);
router.put("/:id", authorController.update);
router.delete("/:id", authorController.destroy);

module.exports = router;
```

## Closing

Over the next few classes, we're going to continue using Express to building out
dynamic applications. Next, we'll learn about Models and build out models for
our apps so that we can incorporate data interaction with MongoDB.
