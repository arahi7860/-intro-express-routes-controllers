# Intro to Express: Routes & Controllers

Today is a big day in SEI! We've build front-end applications using our
knowledge and expertise of HTML, CSS and JavaScript. Next we're going to venture
in to new territory: building full-stack applications!

The framework we're going to learn for building full-stack applications is
called Express. We're going to use it to build feature-rich applications, like
the ones you often use: Spotify, Facebook, Twitter, etc. We'll learn how these
applications are able to handle requests from users and save data to a database.

Before we get to that, let's learn a little more about how the internet works
and how Express fits in to building full-stack applications.

## Learning Objectives

By the end of this lesson, developers should be able to:

- Define what an application framework is.
- Explain and understand the request-response cycle.
- Discuss how the internet works including:
  - Browser requests
  - Status codes
  - HTTP methods
- Build a simple server side application with Express:
  - Configure Express application
  - Routes
  - Params
  - RESTful architecture
  - Controllers

## Introduction

### What is a Framework?

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

## Review: How Does the Internet Work?

Before we can build our first full-stack application, we need to discuss how the
internet works. Once we know a little bit about how the internet works, we can
start to think about how Express (our back-end) fits in with our front-end.

### Client-Server

The internet follows a model of communication called `client-server`. Your
browser is the `client`, used for navigating and interacting with the web. While
our browsers exist on our computers or phones, the internet does not! Our
browsers are portals to the internet. The other side of the equation is the
`server` - a computer somewhere that stores and _serves_ webpages.

![Client-Server](./images/client-server.png)

The client and server communicate using `http` and the `request-response` cycle.

### HTTP

HTTP stands for **Hyper-Text Transfer Protocol**. If the client and server
communicate, then HTTP is the structure of that communication. HTTP dictates how
the client requests information from a server and how the server responds. Each
message is similarly formatted - so you can think of them as like electronic
telegrams.

Requests always have these three parts:

1. Request line (including the URL and the HTTP Method)

- We will refer to the _path_ and _method_ of a request throughout the lesson.

2. Request header (additional information about the request and what we expect
   in the response)
3. Body message (optional - things like form data)

Responses in turn always have these three parts:

1. Status (a status code indicating how the request was handled)
2. Response header (additional information about the response)
3. Body message (optional - an html document, JSON, XML)

### HTTP Methods

Clients make requests to a location (called a URL) using a method. There are 10
possible HTTP methods, but only 5 that are important:

| Method Name | Description                             |
| ----------- | --------------------------------------- |
| GET         | Used for retrieving data from a server. |
| POST        | Used for sending data to the server.    |
| PUT         | Used for replacing data on the server.  |
| PATCH       | Used for updating data on the server.   |
| DELETE      | Used for deleting data from the server. |

Browsers have only implemented GET and POST, the rest we need to do using
JavaScript or find some kind of work-around.

When the server receives a request, it processes the message and then sends a
response. The server always sends a response, though sometimes that response is
just to tell the client that there was an error. Generally, the response will be
an HTML document.

### How Does it Actually Work?

When you type a URL in the navigation bar of your browser, you make a GET
request to that URL (i.e. `http://www.google.com`). The server receives that
message and formulates a response: a 200 status code (to indicate everything
worked out just fine) and an HTML document for the Google home page.

When you click on a link, you're making a GET request to a URL, just like when
you type the URL in the navigation bar of your browser. The server receives the
request and sends a response with a new HTML document for you.

When you submit a form, you make a POST request to a URL defined in the `action`
attribute of the `<form>` element. The fields in the form become the request
body. The server processes the request (typically saving the request body to a
database) and then sends a response.

### Why Do We Care? How can we use this?

When the internet was first created, you would request a document that already
existed (in full) on the server. So when you typed in
`http://www.timberners-lee.com`, you received an HTML document on Tim's server.
If you wanted to see the about page on Tim's website, you would navigate to
`http://www.timberners-lee.com/about.html`. The important thing to note is that
someone wrote those HTML pages in full and by hand.

That worked well when the internet was just used for sharing scientific
documents. Back then, websites weren't backed by databases and didn't need to
dynamically retrieve content like they do now.

Imagine you have a website for your cats. Each of your 10 cats has a page on the
site:

- `www.cat-astrophy.com/whiskers.html`
- `www.cat-astrophy.com/mr-fuzzy-pants.html`
- `www.cat-astrophy.com/purrrasaurus-rex.html`
- `www.cat-astrophy.com/walter.html`
- etc

You would have to make a full HTML document for each page. None of the HTML
would change from page to page, but the information inside of that HTML would -
because it was unique to each cat.

What a load of work! We don't have time for that! We're programmers!

> True story: this is why PHP was created.

Instead of writing all that HTML by hand, lets build and use tools that make it
so we can dynamically retrieve content and "fill in" an HTML document like a
template. That is exactly what Express and any other web framework does!

In server-side applications, we can use information in the HTTP requests,
including...

- The _method_ and the URL _path_ in the Request line
- The Request body

..to dynamically generate the content we want to send back to the client in the
Response.

When we use Express, we don't need to have a `whiskers.html` file on our
server - we just need to have all our information about whiskers in our
database. Then when someone makes a GET request to
`ww.cat-astrohpy.com/whiskers`, our server-side application (built with Express)
will see that the request contains `whiskers` inside of it, pull the data about
whiskers from the database and dynamically render and send an HTML document with
that data.

So much easier!

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
   (or just run `npm init -y`).
4. Type `npm install express`.
5. Work through the rest of this lesson

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
- we're invoking the module, instantiating a constant app which holds all the
  methods and state we use to write and run our web app
- we're starting our server (and app) by listening on port 4000 for incoming
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

Express contains a function for each HTTP method which in turn accepts a path as
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

`app` is the variable we've declared above. It's an `instance` of the express
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

The second argument that `get()` takes is a function. It's how we tell express
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

### Params in Express

How do we make our routes dynamic? Using parameters!

[Route parameters](https://expressjs.com/en/api.html#req.params) give us
flexibility when writing routes in Express.

Let's update `index.js` to include:

```js
app.get("/:name", (req, res) => {
  console.log(req.params);
  res.send(`Hello ${req.params.name}`);
});
```

> Note: the `request` and `response` objects are often shortened to just `req`
> and `res`.

Our route has changed! What is different?

Route parameters are named sections of our path, they are placeholders (similar
to variables or parameters) that capture values at their location in a URL.

These values are held in the `req.params` object and can be used to deliver
dynamic responses to an HTTP request.

Now if we visit `http://localhost:4000/Whiskers`, we should see:

```
hello Whiskers
```

What do you think we'll see if we visit `http://localhost:4000/Purrasaurus-rex`?

## Break (10:00 min)

## Intro to REST

In addition to requesting files on the file system - routes used to take what
ever form the developer(s) pleased. You'd end up with some wacky URLs/paths:

- `/first-post`
- `/post/second`
- `/the-third-post-I-ever-wrote`

There was no pattern for designing routes! This is where REST comes in.

REST stands for **Representational State Transfer**. REST solves a number of
problems with routing, but the most important for our use cases is the problem
of the relationship between URL paths and HTTP methods and how we should define
our Routes.

When we're building web applications (in any framework) we want our routes to be
RESTful, meaning they follow the guidelines of REST. What does that mean?

The first thing to consider is, what is our _resource_?

A resource is any domain of our application. Generally, if you have a model in
your app, it can be considered a resource (though not always). In a to do list
application, the resources are probably: to do items, to do lists, and users.

Another explaination about what is
[a resource:](https://stackoverflow.com/questions/10799198/what-are-rest-resources/10883810#10883810)

> A resource is anything that’s important enough to be referenced as a thing in
> itself... Usually, a resource is something that can be stored on a computer
> and represented as a stream of bits: a document, a row in a database, or the
> result of running an algorithm.

A table of our REST routes and their corresponding controller actions for
authors items would therefore look like this:

| URL              | Path        | Method        | Action   | Description                         |
| ---------------- | ----------- | ------------- | -------- | ----------------------------------- |
| `/author`        | `/`         | `GET`         | #index   | List all authors                    |
| `/author/new`    | `/new`      | `GET`         | #new     | Render form to create a new author  |
| `/author`        | `/`         | `POST`        | #create  | Create a new author in the database |
| `/author/1`      | `/:id`      | `GET`         | #show    | Show a single author                |
| `/author/1/edit` | `/:id/edit` | `GET`         | #edit    | Render form to update an author     |
| `/author/1`      | `/:id`      | `PATCH`/`PUT` | #update  | Update an author in the database    |
| `/author/1`      | `/:id`      | `DELETE`      | #destroy | Delete an author                    |

For every resource in our application, we want to follow this structure. That
doesn't necessarily meant that every resource will have
[all of these routes and actions](https://restfulrouting.com/#introduction) -
but that our routes and actions should follow this pattern.

#### Recap: URL Routes

A **route** is a **path** and a **verb**:

| URL                                       | Path               | Verb  |
| ----------------------------------------- | ------------------ | ----- |
| `http://www.facebook.com/users/stevejobs` | `/users/stevejobs` | `GET` |

REST stands for **Representational State Transfer** and is a pattern that
determines how we should structure our routes.

#### Resource Table

Bookmark this table! It can be really helpful to think through your routes.
Believe it or not: almost every action you perform on the web can be described
by one of these.

| URL                | Path        | Method        | Action   | Description                                        |
| ------------------ | ----------- | ------------- | -------- | -------------------------------------------------- |
| `/resource`        | `/`         | `GET`         | #index   | List all items of `resource`                       |
| `/resource/new`    | `/new`      | `GET`         | #new     | Render form to create a new instance of `resource` |
| `/resource`        | `/`         | `POST`        | #create  | Create new `resource` in the database              |
| `/resource/1`      | `/:id`      | `GET`         | #show    | Show a single `resource`                           |
| `/resource/1/edit` | `/:id/edit` | `GET`         | #edit    | Render form to update a single `resource`          |
| `/resource/1`      | `/:id`      | `PATCH`/`PUT` | #update  | Update `resource` in the database                  |
| `/resource/1`      | `/:id`      | `DELETE`      | #destroy | Delete a `resource`                                |

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
router.destroy("/:id", authorController.destroy);

module.exports = router;
```

### Separating our Router and Controller

We know from previous lessons and exercises that we define routes in Express
like this:

```js
app.get("/", (req, res) => {
  res.send("hello world");
});
```

There's something that we haven't talked about here yet though. The above
snippet is really doing two things: routing and controlling (i.e. we're defining
our router and controller action in the same place). For small apps, this is
totally fine and it can even be alright in larger apps. However, we can benefit
from separating these:

```js
// Router:
app.get("/", controller.index);
```

```js
// Controller:
const controller = {
  index: (req, res) => {
    res.send("hello world");
  }
};
```

The `controller` object acts as our controller and each method we place inside
that object is a controller action (in REST, we use the naming conventions
`index`, `show`, `new`, `create`, `edit`, `update`, or `delete`). Then our
router just adds our controller actions where appropriate. This gets to be a lot
more readable when we want to have multiple actions per route, like when we want
to make sure a user is authenticated before visiting a route:

```js
app.get("/", config.isAuthenticated, controller.index);
```

Our `isAuthenticated` method will check to see if a user is signed in. If they
are, then we'll move on to the `controller.index` action; if they are not, then
we'll redirect them to the sign in page.

## You Do / Build a Simple Express App

We're going to work through building out the routes and controllers for a simple
Express app.
[This is the link to the repo.](https://git.generalassemb.ly/dc-wdi-node-express/routes-controllers-practice)
Follow the instructions there on how to get started.

## Closing

Over the next few classes, we're going to continue using Express to building out
dynamic applications. Next, we'll learn about Models and build out models for
our apps so that we can incorporate data interaction with MongoDB.
