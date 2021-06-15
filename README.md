
# WebKriti 2021 Guide

A short guide to help plan out your project and serve as a quick reference.


## Getting Started

Before we rush into writing some code, projects require discussion.
Here's how to get started with planning a project.

### Settle on a Feature Set

You have been given certain project requirements based on your chosen project topic. List down all of these requirements.
Beyond this, you may chose to add your own additional features. Discuss all of the things you want to add before you start working on the project.

It is important to have an understanding of what you are implementing before you write any code.
Note down every planned feature in one place so you can refer to it easily.

### Design a UI (Optional)

Figma is a great place to start designing your UI.
Now that you have a feature set that is agreed upon, you can design user interfaces for each of them.

**This step is not compulsory**, espescially if you are short on time.
However it is recommended that you do this before proceeding with the application.
It will help you visualise your frontend making it easier to write the HTML/CSS later.

### Plan Your API

The API is the core of your website.
It is what will allow the `Client(Frontend)` to communicate with the `Server` in a meaningful manner.

#### Endpoints:

The first step is to figure out your endpoints.
Have a look at your feature set.
Every independent feature requires it's own endpoint, a location(URL) from where that feature can be accessed.

*For Example:* Let us assume we are making a very simple Blogging Website that provides the following functionalities.

- Sign up the user
- Log in the user
- Browse a feed of blogs
- Read a specific blog
- Post a blog

Then for each of these features, we should identify an endpoint that holds the functionality.
For this example, I might define them as follows:

- `website.com/`**`register`**  - To sign up the user.
- `website.com/`**`login`** - To log in the user
- `website.com/`**`feed`** - To display the feed
- `website.com/`**`blog?id={blog_id}`** - To display a specific blog that is assosciated with a given *`blog-id`* that is passed as a query parameter.
- `website.com/`**`write`** - To allow a user to write and submit a blog

As you saw, the part of the URL following the website itself acts as a path to your various features.
It is a good idea to decide these as they can define the structure of your application.

#### Routes:

Building an application with `node` and `express`, you will be creating `routes` at which you handle requests.
These routes correspond to the endpoints you defined just now.

Routes hold information on `request` and `response`.
At each route you expect to receive a certain request from the client.
You handle the request data, performing the necessary actions, and generate a certain response that the client expects

Let us see how we would define the routes in the Blogging example from before:

**`POST ROUTE /register`**
```
- Expect { email, password }
- Handle
    Verify that email hasn't registered before
    Push data to PSQL database
- Respond { access_token } or { error }
```

**`POST ROUTE /login`**
```
- Expect { email, password }
- Handle
    Verify that email exists in database
    Verify password hash matches hash in database
- Respond { access_token } or { error }
```

**`GET ROUTE /feed`**
```
- Handle
    Generate list of posts as feed_list
- Respond { feed_list } or { error }
```

**`GET ROUTE /blog?id={blog_id}`**
```
- Handle
    Find blog post with an id of query.params.id whose value is {blog_id}
- Respond { blog_title, blog_author, blog_text } or { error }
```

**`POST ROUTE /write`**
```
- Expect { access_token, blog_title, blog_text }
- Handle
    Verify token as proof that user is signed in
    Push blog data to PSQL database
- Respond { success } or { error }
```

As we saw, the server can be defined with a blueprint like this that tells us how the comminication with clients should work.

> NOTE: `http` or `https` comminication is always initiated by the `client`. The client (frontend) app first has to generate a request, to which a server is able to respond. If we require the server to initiate communication (for example, to notify a certain user about an event) then we can use something like [socket.io](https://socket.io/)

#### Database:

You will be wo in PostgreSQL for the project.
It is a good idea to have atleast a basic description of what your database will look like.
This is not an advanced database course, so something simple should do.

Expanding on the Blog example, we could make a basic blueprint for the database as:

```
Database:BlogApp
    - Table:Users
        uid <unique>,
        email,
        password_hash
    - Table:Blogs
        blog_id <unique>,
        title,
        text,
        author_uid
```

This gives us a fairly good idea of what tables should be creted and what fields they should have.
The rest is up to the SQL queries.

> NOTE: Try and make sure you are not making the database too complex. While you are not required to follow any rules for this, remeber that the more complex the database, the more complicated the queries will be to perform certain actions. If the query isn't written correctly it may cause inconsistencies that could break the app.

## Programming the Website

Now that you hava planned a satisfactory API, it's time to code! Here's some tips to get you going.

### Don't Forget your Plans

Keep your planning accessible and legible for all team members.
And always refer to it before you implement anything new.

### The Frontend and Backend Are Decoupled

The `server` application is purely the routes you defined along with your database. 
It receives JSON requests, and returns JSON responses.

The `client` application is a seperate frontend project that is able to communicate with your server through the API.
It will use the `fetch()` javascript function to send requests to the `server`, and use the response to update the frontend.

> NOTE: Make sure you understand using javascript to manipulate the frontend. You will be making a heavy use of the [Javascript Document Object](https://www.w3schools.com/js/js_htmldom_document.asp) and the [Fetch Function](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch)

### Divide Your Work (If you are in a group)

The best part of having decoupled the backend and frontend is you can work on them simultaneously.
As long as everyone follows the plans, it is easy to divide frontend and backend between the members.

It should go without saying, do not change the plans midway through programming unless every member is involved and clear on the changes.
Confusion can lead to a codebase that isn't compatible with itself.

### Modular Code and Comments

Extract code that is repeatedly used into it's own function or module.
This helps keep your project organized and clean.

Add comments as frequently as you can.
This makes your code understandable not only to others, but also for your own team.
Doing this at the end can be tedious.
This is one of the hardest tips to follow, but it is good to at least try.

### Cross-Origin Resource Sharing

Cross-Origin Resource Sharing, or CORS, can break the communication between the clients and the server, if the client application is hosted seperately.
For instance, let us say your `node` backend is hosted on [Heroku](https://www.heroku.com/) while your frontend has been uploaded to [Netlify](https://www.netlify.com/).
Then you must ensure that your server application is able to accept Cross-Origin (different origin) requests from the Netlify domain.

Make sure you understanding [CORS](https://auth0.com/blog/cors-tutorial-a-guide-to-cross-origin-resource-sharing/) if you end up in that scenario.

## Guidelines

For the sake of the competition, the guidelines must be followed. Some important points have been listed as a reminder.

- Don't use frontend frameworks such as React or Vue
- Don't use JQuery
- Don't use Bootstrap or any CSS libraries
- Use PostgreSQL for your database
- Try and implement at least the required features for your project topic.
