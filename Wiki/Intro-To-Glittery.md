# Intro To Glittery
TODO
## Blog Service
Glittery provides a blogging service that helps you make your own blog in simple yet powerfull way. It gives you the
ability to write custom pages such as `About Me` and `Search` pages using
[Handlebars templates](https://handlebarsjs.com/), with a tons of helper functions.

Glittery let its users write thier posts in [CommonMark](http://commonmark.org/) and convert these posts to HTML files,
it also give its users the ability to customize each posts using
[Toml Minimal Language](https://github.com/toml-lang/toml)

Users can manage and communicate with blog service using `glittery` command-line interface, you can get help message by
running `glittery --blog --help`, all arguments that belong to blog service should come after `--blog`.

The following chapters will explain how to use Glittery Blog Service.
### Pages
Glittery let you write your own pages using [Toml Minimal Language](https://github.com/toml-lang/toml) and
[Handlebars templates](https://handlebarsjs.com/), that will give you the ability to write customized pages.

Pages also can uses CSS files, CSS files are stored in `css-path` so they can be shared between pages to have uniforme
look.

Every page should have a folder that contains two files, and the name of that folder is the `page-id` for that page, this
folder should be stored in `pages-path` folder, so if we want to create **About Me** page, its tree files would look
like this:
``` sh
pages-path
└── about-me                  # <-- The page's folder, it's name is the 'page-id' for this page
    ├── template.hbs
    └── info.toml
```

`page-id` can be any unicode character except those listed in the [manual](./manual.md#pages), Glittery will ignore pages
with `page-id` that contain any of them.

Both files are required to construct an HTML page, any page that does miss any of them will be ignored by Glittery.
These files will be explained in the following chapters.

#### info.toml File
This file contains the configuration and content of the page, and is written using
[Toml Minimal Language](https://github.com/toml-lang/toml).

There are two tables in this file, `[config]` and `[content]`. The `[config]` table ment to configure the page. While
the `[content]` table is ment to contain the page content that will appaer in the HTML page, users can define thier own
key/value pairs freely in this table and use these keys inside `template.hbs` to represent thier values.

**Key name** may be any unicode character except those listed in the [manual](./manual.md#pages), Glittery will ignore keys
with invalid character.

**Info**: There is no meaning to have page without `[content]` table since the HTML page won't have any content.

#### template.hbs File
After we fill up `info.toml` with our page's content, we write this file to describe how the page content will appaer
in our page using [Handlebars templates](https://handlebarsjs.com/).

Glittery let's you accesse your key/value pairs in your `template.hbs` file, let say we have this `info.toml`:
``` toml
[content]
name = "Muhannad Alrusayni"
```

You could use the value that assigned to `name` key by writing this inside your `template.hbs`:
``` html
<h1>My name's {{ name }}</h1>
```

Glittery will convert this to something like:
``` html
<h1>My name's Muhannad Alrusayni</h1>
```

Glittery also provides a lot of [**Helper Functions**](./manual.md#helper-functions) to help you write your pages
easily, we'll see some **Helper Functions** in next chapters.

#### partial.hbs File
There is another file we didn't talk about yet, this file is just like `template.hbs`, and it's optional, it's purpose is
to provide another template for the page, wich could be used to embedded the page into other page's `template.hbs` files.

So for example if we have two pages, **Home** page and **About Me** page, we want **Home** to have small panle that
contains the **About Me** page's content, we could achieve this by adding `partial.hbs` file for **About Me** page that
describes how the **About Me** panle will look like when it's embedded into **Home** page or any other page. We will see
a real example next chapters.

#### Page Resources
Users can easily add resources in thier pages and that could be done by adding a new folder called `resources` in the
page's folder, and put what ever resources they want to use in thier page, such as images and videos.

The **About Me**'s tree files would look like this when we add some resources:
``` sh
pages-path
└── about-me
    ├── template.hbs
    ├── info.toml
    └── resources
        ├── me.png
        └── CV.pdf
```

#### Fundamental Pages
Since this's a blogging service, we need to write at less two page **Home** page and **Post** page.

You can think of these two pages as special pages, since Glittery considered **Home** page as the main page of your
blog, and **Post** page is the page that will be used as template for all posts in your blog.

There are some rules to follow when you write these fundamental pages.

**Home** page's rule:
  - Home page's folder must have the name `home`.
  - Home page cann't have `partial.hbs` file, since it cann't be part of other page.
  - `template.hbs` must call helper function `posts`.

**Post** page's rule:
  - Post page's folder must have the name `post`.
  - Post page's must have `partial.hbs` in addition to `template.hbs` and `info.toml`.
  - `template.hbs` and `partial.hbs` must call helper function `post-content`.

#### Creating Pages
`glittery` makes it pretty easy to create a pages. let's say you want to create a new page called **My CV**, we simply run the following:
```
$ glittery --blog --new-post my-cv
```

glittery will take care of that and add a new folder in `pages-path` with default values in `info.toml` and
`template.hbs` file, the tree files would look like this:
``` sh
pages-path
└── my-cv
    ├── template.hbs
    └── info.toml
```

Then you can start writing your page. Once you have done wrting your page you can change `publish`'s
value to `true` in `info.toml` and your page should be available to others in seconds.

#### Deleteing Pages
Some time you want to delete pages that you don't need anymore, to do that simply run the following:
``` sh
$ glittery --blog --remove-page [PAGE_ID]
```
And `glittery` will take care of the rest and delete that page and its related files and folders.

Let's say you want to delete a page that have `contact-me` id, then you would run the following:
``` sh
$ glittery --blog --remove-page contact-me
```

And that's it.

#### CSS Files
Glittery makes it easy to use CSS files in your pages, and that could be done by storing CSS files under `css-path` and
link them in your page's `template.hbs`.

Of course you can link your pages to use an external CSS files such as [Bootstrap](https://getbootstrap.com/),
[Spectre](https://picturepan2.github.io/spectre/) or any other CSS library you like.

Just make sure you use thier classes in your `template.hbs` and `partial.hbs` files.

#### Building Pages
Once you'r done from writing your page's files, you need to build it by running:
``` sh
$ glittery --blog --build-page [PAGE_ID]
```

Let's say we want to build our **About Me** page, then we would run:
``` sh
$ glittery --blog --build-page about-me
```

Once you build your page for the first time a new folder called `.status` will be created inside the page's folder wich
will contain two files:
``` sh
pages-path
└── about-me
    ├── .status               # a folder that contains all other necessary files that belong to this page
    |   ├── data.toml             # file that contain all data that belong to this page
    |   └── page.html             # the HTML page that have been constructed from 'info.toml' and 'template.hbs'
    ├── template.hbs
    └── info.toml
```

This folder contains all other necessary files for the page such as the HTML file and some other files that help
Glittery handle the page in a way or another. Its content is created and updated by `glittery` it self, so users doesn't
need to edit any file under `.status` folder.

The `data.toml` file will contain data for the page, such as **visitors number** for the page since it was created, some
other infomation that help `glittery` keep watching changes on page's files to update page.html when
needed.

The `page.html` is HTML page that was constructed from `template.hbs` and `info.toml` files, and this is the HTML page
that will be published by Glittery.

### Posts
Glittery stores posts and thier related files in subfolders under `posts-path`, every posts most have it's own folder
that contains `post.md` and `info.toml` files.

`post.md` file is written in [CommonMark](http://commonmark.org/) a markup language with two extensions, tables and
footnotes.

`info.toml` file is written in [Toml Minimal Language](https://github.com/toml-lang/toml). This file is just like
Page's `info.toml` file and is used in similar way.

Every posts stored in `posts-path` will get an HTML page by combining it with
[**Post** page](./Intro-To-Glittery.md#fundamental-pages) we talked about earlier. That's why we considere **Post** page
a fundamental page.

Here is a basic post's tree files:
``` sh
post-path
└── intro-to-glittery         # <-- Post's folder, its name is the 'post-id'.
    ├── post.md               # Post's content goes here
    └── info.toml             # Post's infomation goes here
```

#### post.md File
As we said earlier Posts are written in [CommonMark](http://commonmark.org/) (aka Markdown) markup language with two
extensions, tables and footnotes.

This markup language is so simple, you could learn it in less than 1 hour, you can start with its
[tutorial](http://commonmark.org/help/tutorial/).

Once you learned the basic you can write your `post.md` files.

#### info.toml File
`glittery` need some infomation for every posts, so every post's folder must have `info.toml` file.

This file is similar to the `info.toml` file for pages, but `[config]` table have different key/value pairs that we
could used (such as the `date` or `tags` see [manual](./manual.md#configtoml-file)).

#### Post's Resources
Just like pages, posts can have thier resources stored in `resources` subfolder, a tree files for post that have
resources would look like this:
``` sh
post-path
└── intro-to-glittery
    ├── post.md
    ├── info.toml
    └── resources                    # <-- resources folder
        ├── glittery-theme.png
        └── how-to-make-theme.webm
```

#### Creating Posts
`glittery` makes it pretty easy to create a posts. let's say you want to start a new post with title like **Intro To
Glittery**, we simply run the following:
```
$ glittery --blog --new-post intro-to-glittery
```
glittery will take care of that and add a new folder in `post-path` with default values in `info.toml` and `post.md`
files, the tree files would look like this:
``` sh
post-path
└── intro-to-glittery
    ├── post.md
    └── info.toml
```

Then you can start writing your post. Once you have done wrting your post you can change `publish`'s value
to `true` in `info.toml` and your post should be available to others in seconds.

#### Deleteing Posts
Some time you want to delete posts that you don't need anymore, to do that simply run the following:
``` sh
$ glittery --blog --remove-post [POST_ID]
```
And `glittery` will take care of the rest and delete that post and its related files and folders.

Let's say you want to delete a post that have `my-journey-in-japan` id, then you would run the following:
``` sh
$ glittery --blog --remove-page my-journey-in-japan
```

And that's it.

#### Building Posts
Just like how we build our pages, once you'r done from writing your post's files, you need to build it by running:
``` sh
$ glittery --blog --build-post [POST_ID]
```

Let's say we want to build our **Intro To Glittery** post, then we would run:
``` sh
$ glittery --blog --build-post intro-to-glittery
```

When you build your post for the first time a new folder called `.status` will be created inside the post's folder wich
will contain two files:
``` sh
post-path
└── intro-to-glittery
    ├── .status               # a folder that contains all other necessary files that belong to this post
    |   ├── data.toml             # file that contain all data that belong to this post
    |   └── page.html             # the HTML page that have been constructed from 'post.md', 'info.toml' and 'template.hbs' file that belong to Post page.
    ├── post.md
    └── info.toml
```

They are the same files that we talked about in [Building Pages chapter](./intro-to-glittery.md#building-pages), but
this time for the post.

### Basic Example
In this example we will write **Home**, **Post** and **About Me** pages, and we'll use all what we learned from the
previous chapters.

First we will create all pages we need by running:
``` sh
$ glittery --blog --new-page home
[DONE] fundamental 'home' page have been created
$ glittery --blog --new-page post
[DONE] fundamental 'post' page have been created
$ glittery --blog --new-page about-me
[DONE] 'about-me' page have been created
```

Now our `pages-path` should look like this:
``` sh
pages-path
├── about-me
│   ├── info.toml
│   └── template.hbs
├── home
│   ├── info.toml
│   └── template.hbs
└── post
    ├── info.toml
    ├── partial.toml
    └── template.hbs
```

All files have been created with the default values, `info.toml` files will contain the
following:
``` toml
[config]
# If this is `false` this page won't be published, set it to `true` if you want it to be published
publish = false
```

Now let's start writing our pages.

##### Home page
let start with **Home** page, in the `info.toml` file we will write:
``` toml
[config]
# If this is `false` this page won't be published, set it to `true` if you want it to be published
publish = false

[content]
# The page html language
language = "en"

# This is the page's title that will be used in the HTML file
title = "Home"

# Header content
my-name = "Muhannad Alrusayni"
intrest = "Programming ꞏ UI Design ꞏ Photography ꞏ 3D Moduleing"
```

Now let's fillup `template.hbs` file, with the following:
``` html
<!doctype html>
<html lang="{{ language }}">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
        <!-- import Bootstrap CSS library -->
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
        <title>{{ title }}</title>
    </head>
    <body>
        <!-- header -->
        <div class="jumbotron jumbotron-fluid bg-dark py-0 m-0">
            <div class="container text-center pt-5 pb-4">
                <p class="h4 font-weight-bold text-light">{{ my-name }}</p>
                <p class="h6 font-weight-light text-secondary">{{ intrest }}</p>
            </div>
            <div class="d-flex justify-content-center">
                <ul class="nav nav-tabs">
                    <li class="nav-item"><a class="nav-link active" href="./">{{ title }}</a></li>
                    <li class="nav-item"><a class="nav-link text-light" href="{{ link about-me }}">{{ get-title about-me }}</a></li>
                </ul>
            </div>
        </div>
        <!-- page content -->
        <div class="container p-2">
            {{ posts 10 }}
        </div>
        <!-- footer -->
        <div class="container-fluid bg-light text-secondary">
            <small class="">Powered by <a class="text-secondary" href="https://github.com/0x3UH4224D/Glittery">Glittery</a></small>
        </div>
    </body>
</html>
```

As you can see, we use keys that we have defined in `info.toml` file such as language key:
``` html
<html lang="{{ language }}">
```

To get output like:
``` html
<html lang="en">
```

We also use some [**Helper Functions**](./manual.md#helper-functions) such as `link`, `get-title` and
`posts`, we will explain each one of them in minutes.

First let's talk about [**Helper Functions**](./manual.md#helper-functions) and what's thier purpose, helper functions
helps you to do your work faster and easier as we will see in the following lines.

The things comes after helper functions called arguments (or parameters), so in this line:
``` html
{{ get-title about-me }}
```

`about-me` is the argument that we pass to our helper function `get-title`.

When we write helper function we actually call that help function to do specific task for us, each helper function does
different task. As result they return a string for us.

Now let's see what we will get when we call a `get-title` as we did in our `template.hbs`:
``` html
{{ get-title about-me }}
```

The result will be like this:
``` html
About Me
```

As you may guessed `get-title` return the page title when we pass the `page-id` or `post-id`.

What about `link` ?, `link` is helper function that can take `page-id`, `css-id`, `post-id` or even `resource-id` as
it's arguments and return the public link for it. And in our example we pass `page-id`:
``` html
{{ link about-me }}
```

The result will be something like:
``` html
./about-me/
```

This function is usefull since you don't need to write the link yourself you only pass id for the page, post, css or
resource file and it will return a proper link for it, it also will make your `template.hbs` more readable.

Finally we have `posts` helper function and it accept a number as its argument, it return a list of posts as HTML
list, and this is done by useing `partial.hbs` file that belong to **Post** page to construct a that HTML list.

By doing this we'll be able to include as much posts as we want in our **Home** page without haveing much trouble.

Ee use it this way:
``` html
{{ posts 10 }}
```

And the returned value can not be told at this point until we write `partial.hbs` file for **Post** page.

##### About Me Page
Just like **Home** page we will first fillup `info.toml` file, we would write something like this:
``` toml
[config]
# Change this to 'static' to get static page.
type = "static"

[content]
language = "en"

# Header content
title = "About Me"
my-name = "Muhannad Alrusayni"
my-short-name = "M.Alrusayni"

intrest = "Programming ꞏ UI Design ꞏ Photography ꞏ 3D Moduleing"
description = """
I can write what ever description that I want
in multiline string
"""
email = "muhannad.alrusayni@gmail.com"
skills = [ "Prgramming", "UI Design", "Photography", "3D Modeling" ]
```

As you can see we use an array as value for `skills` key, we will see how to handle arrays in our `template.hbs` file
next.

We also change the `type` of our page to `static`, this will tell `glittery` to construct HTML file for this page
only once and use it for all client request. This method is faster and lighter on the server that will run `glittery`
but some helper functions won't be so usefull, since thier use is shine in dynamic page but not static.

Before we go and write our `template.hbs`, I'll add an image as resources to the page, so our **About Me**
tree files would look like this:
``` sh
pages-path
└── about-me
    ├── template.hbs
    ├── info.toml
    └── resources
        └── me.png
```

Now let's fillup our `template.hbs` file, with the following:
``` html
<!doctype html>
<html lang="{{ language }}">
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <!-- import Bulma CSS library -->
        <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/bulma/0.6.2/css/bulma.css">
        <title>{{ title }}</title>
    </head>
    <body>
        <!-- header -->
        <section class="hero is-dark">
            <!-- hero content -->
            <div class="hero-body">
                <!-- Title for big screen -->
                <div class="container has-text-centered is-hidden-mobile">
                    <h1 class="title">{{ my-name }}</h1>
                    <h2 class="subtitle is-capitalized has-text-weight-light">
                        {{ intrest }}
                    </h2>
                </div>
                <!-- Title for small screen -->
                <div class="container has-text-centered is-hidden-tablet">
                    <h1 class="title">{{ my-short-name }}</h1>
                    <small class="is-capitalized has-text-weight-light">
                        {{ intrest }}
                    </small>
                </div>
            </div>
            <!-- links to my blog pages -->
            <div class="hero-foot">
                <nav class="tabs is-boxed">
                    <div class="container">
                        <ul>
                            <li><a href="{{ link home }}">{{ get-title home }}</a></li>
                            <li class="is-active"><a href="">{{ title }}</a></li>
                        </ul>
                    </div>
                </nav>
            </div>
        </section>
        <!-- content of the page -->
        <div class="container">

        </div>
        <!-- footer -->
        <hr class="is-marginless" />
        <footer class="footer is-paddingless">
            <div class="container">
                <p class="content has-text-centered is-small">
                    PoweredBy <a href="https://github.com/0x3UH4224D/Glittery">Glittery</a>
                </p>
            </div>
        </section>
    </body>
</html>
```

As you can see we use a new helper function called `list` that takes key name and return an HTML list, the list items
are taken from the key's value, so in our example the result will be something like this:
``` html
<ul class="list">
    <li class="list-item">Programming</li>
    <li class="list-item">UI Design</li>
    <li class="list-item">Photography</li>
    <li class="list-item">3d Modeling</li>
</ul>
```

And that's it for our **About Me** page.

##### Post page
You may notice that `glittery` have created three files in **Post** page's folder, and that's becuse of the rules in
the [Fundamental Pages](./Intro-To-Glittery.md#fundamental-pages) chapter. wich says that **Post** page's folder must
have `partial.hbs` file.

This page have helper function `post-content` wich does bring the content for posts into the page. This will be explained in minutes.

One another thing to put in mind when you write **Post** page's `template.hbs` and `partial.hbs` files,

TODO: STOPED HERE

## Table Schema
### users table defintion
| column name      | Primary Key | data type           | null-able | default |
|------------------|-------------|---------------------|-----------|---------|
| id               | YES         | number              | NO        |         |
| username         |             | text(25)            | NO        |         |
| password         |             | text(64) (32 bytes) | NO        |         |
| salt             |             | text(64) (32 bytes) | NO        |         |
| photo            |             | blob                | YES       |         |
| premission-level |             | number              | NO        |         |

Make sure users.password column have proper size/length to store the hashed passwords from our supported hash algorithms
32 bytes for argon2 algorithms and SHA256 (64 for TEXT length since each byte will take 2 char, this maybay not correct calc from me)

## Logging file
TODO
## Configuration file
### Sections
| Name               | comment                                                                                          |
|--------------------|--------------------------------------------------------------------------------------------------|
| glittery           | All configuration values that effect glittery                                                    |
| blog               | All configuration values that effect blog                                                        |
| language           | All configuration values that effect languages support                                           |
| security           | All configuration values that effect the security in this server                                 |
| security.passwords | All configuration values that effect how to encrypt and hash passwords to store them in database |
| security.argon2i   | All parameters to be used with argon2i algorithm                                                 |

### Content
| Section            | Key Name         | Type             | Default Value      | commecnt                                                                                                                                                                 |
|--------------------|------------------|------------------|--------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| glittery           | root-path        | String           | $HOME/.glittery/   | this folder will contain all folders and files that belong to glittery                                                                                                   |
| blog               | blog-path        | String           | `root-path`/blog/  | this folder will contain public posts                                                                                                                                    |
| blog               | posts-path       | String           | `blog-path`/posts/ | this folder will contain public posts                                                                                                                                    |
| blog               | css-path         | String           | `blog-path`/css/   | this folder will contain CSS files                                                                                                                                       |
| blog               | pages-path       | String           | `blog-path`/pages/ | This folder contains page's folders and files                                                                                                                            |
| blog               | public-path      | String           | blog               | Path for the blog in the domain, the defualt will be localhost/blog if change this value it will be localhost/NEWPATH                                                    |
| language           | numbers          | Array of  String | []                 | enter your language's numbers if prefer to display them insted of English numbers or leave it empty otherwise. Note: array elements must start 0 and end with 9          |
| security.passwords | hasher-algorithm | String           | argon2i            | Pick one of the supported hash algorithms. current supported hash algorithm is only argon2i                                                                              |
| security.argon2i   | parallelism      | Number           | 1                  | Degree of parallelism (i.e. number of threads), Value must be between 1 and 2^24-1                                                                                       |
| security.argon2i   | iterations       | Number           | 10                 | number of iterations to preform. Increasing this froces hashing to longer. Value must be between 1 and 2^32-1                                                            |
| security.argon2i   | memory-size-kib  | Number           | 4096               | Amount of memory size to use. Increasing this forces hashing to use more memory in order to thwart ASIC-based attacks. Value must be between (8 * iterations) and 2^32-1 |
| security.argon2i   | secret-key       | String           | Random text        | Optional secret key . Value must vaild UTF-8 and have number of bytes between 0 and 2^32-1                                                                               |
