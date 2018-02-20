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
    └── content.toml
```

`page-id` can be any unicode character except those listed in the [manual](./manual.md#pages), Glittery will ignore pages
with `page-id` that contain any of them.

Both files are required to construct an HTML page, any page that does miss any of them will be ignored by Glittery.
These files will be explained in the following chapters.

When Glittery build a page for the first time, a folder called `.status` will be created in the page folder wich will
contain two files:
``` sh
pages-path
└── about-me
    ├── .status               # a folder that contains all data that belong to this page
    |   ├── info.toml             # file that contain all info that belong to this page
    |   └── page.html             # the HTML page that have been constructed from 'content.toml' and 'template.hbs'
    ├── template.hbs
    └── content.toml
```

This folder and its content is created and updated by `glittery` it self, so users doesn't need to edit any file under
`.status` folder.

The `info.toml` file will contain data for the page, such as **visitors number** for the page since it
was created, some other infomation that help `glittery` keep watching changes on page's files to update page.html when
needed.

The `page.html` file that was constructed is also stored in `.status` folder, this is the HTML page that visitors
will see when they visit this page.

#### content.toml File
This file contains the configuration and content of the page, and is written using
[Toml Minimal Language](https://github.com/toml-lang/toml).

There are two tables in this file, `[config]` and `[content]`. The `[config]` table ment to configure the page. While
the `[content]` table is ment to contain the page content that will appaer in the HTML page, users can define thier own
key/value pairs freely in this table and use these keys inside `template.hbs` to represent thier values.

**Key name** may be any unicode character except those listed in the [manual](./manual.md#pages), Glittery will ignore keys
with invalid character.

**Info**: There is no meaning to have page without `[content]` table since the HTML page won't have any content.

#### template.hbs File
After we fill up `content.toml` with our page's content, we write this file to describe how the page content will appaer
in our page using [Handlebars templates](https://handlebarsjs.com/).

Glittery let's you accesse your key/value pairs in your `template.hbs` file, let say we have this `content.toml`:
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

So for example we have two pages **Home** page and **About Me** page, we want **Home** to have small panle that contains
the **About Me** page's content, we could achieve this by adding `partial.hbs` file for **About Me** page that
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
    ├── content.toml
    └── resources
        ├── me.png
        └── CV.pdf
```

#### Fundamental Pages
Since this's a blogging service, we need to write at less two page **Home** page and **Post** page.

You can think of these two pages as special pages, since Glittery considered **Home** page as the main page of your
blog, and **Post** page is the page that will be used as template for all posts in your blog.

**Home** page's folder must have the name `home`, and must use helper function called `posts` some where in the
`template.hbs` file.

**Post** page's folder must have the name `post`, and must use helper function called `post-content` some where in the
`template.hbs` file.

#### Basic Example
In this example we will write **Home**, **Post** and **About Me** pages, and we'll use all what we learned from the
previous chapters.

Good news that `glittery` command-line interface can take care of creating all files and folders we need to start writing
our pages, and that could be done by running:
``` sh
$ glittery --blog --new-page [PAGE_ID]
```

First we will create all pages we need by running:
``` sh
$ glittery --blog --new-page home
[DONE] 'home' page have been created
$ glittery --blog --new-page post
[DONE] 'post' page have been created
$ glittery --blog --new-page about-me
[DONE] 'about-me' page have been created
```

Now our `pages-path` should look like this:
``` sh
pages-path
├── about-me
│   ├── content.toml
│   └── template.hbs
├── home
│   ├── content.toml
│   └── template.hbs
└── post
    ├── content.toml
    └── template.hbs
```

All `content.toml` files will contain the following:
``` toml
[config]
# Change this to 'static' to get static page.
type = "dynamic"
```

All `template.hbs` will be empty. and we will need to fill them the way we want.

##### Home page
let start **Home** page, in the `content.toml` file we will write:
``` toml
[config]
# Change this to 'static' to get static page.
type = "dynamic"

[content]
css-file = dark.css
language = "en"

title = "Home"
summary = "I write my artical here, most of my articals are about programming, photography and 3D modeling"
```

Now let's fillup `template.hbs` file, with the following:
``` html
<!doctype html>
<html lang="{{ language }}">
    <head>
        <meta charset="UTF-8" />
        {{ use-css (css-file) }}
        <title>{{ title }}</title>
    </head>
    <body>
        <div class="nav-bar">
            <ul class="nav-menu">
                <li class="nav-item"><a class="nav-link" href="{{ link home }}">{{ page-title home }}</a></li>
                <li class="nav-item"><a class="nav-link" href="{{ link about-me }}">{{ page-title about-me }}</a></li>
            </ul>
        </div>
        <div>
            <h2 clas="summary">{{ summary }}</h2>
        </div>
        <div>
            {{ posts 10 }}
        </div>
    </body>
</html>
```

As you can see, we use keys that we have defined in `content.toml` file such as language key:
``` html
<html lang="{{ language }}">
```

to get output like:
``` html
<html lang="en">
```

We also use some [**Helper Functions**](./manual.md#helper-functions) such as `use-css`, `link`, `page-title` and
`posts`, we will explain each one of them briefly.

First let's talk about [**Helper Functions**](./manual.md#helper-functions) and what's thier purpose, helper functions
helps you to do a specific task, they usually make your life easier, 

The things comes after helper functions called argument (or parameters), so in this line:
``` html
{{ page-title home }}
```
`home` is the argument that we pass to our helper function `page-title`.

When we right helper function we actually call that help function, and by do that we want it to do a task for us, and
each helper function does different task. and as result they return a string for us.

Now let's see what we will get when we call a `page-title` as we did in our `template.hbs`:
``` html
{{ page-title home }}
```

The result will be like this:
``` html
Home
```

When we use the same helper function, but with different argument, such as:
``` html
{{ page-title about-me }}
```

The result will different since the argument was different and we will get:
``` html
About Me
```

As you may guessed `page-title` return the page title when we pass the `page-id`.

What about `link` ?, `link` is helper function that takes `page-id`, `css-id` or `post-id` and return the public link
it. and in our example we pass `page-id`:
``` html
{{ link home }}
```

The result will be something like:
``` html
http://localhost/blog
```

This is usefull since you will not need to write the full link, and will make your `template.hbs` more readable.

`use-css` is similar to link, but it only accept `css-id` as its argument, and we use it this way:
``` html
{{ use-css (css-file) }}
```

It will return something like this:
``` html
<link rel="stylesheet" href="http://localhost/blog/css/dark.css">
```

You may notice the braces `(` and `)`, this's called Subexpressions in 
[Handlebars templates](https://handlebarsjs.com/), by using subexpression we tell `glittery` to bring the value of the
key `css-file` we defined in our `content.toml` file and pass it to the function, so it's equal to this line:
``` html
{{ use-css dark.css }}
```

You may say this is easier let's just use this way, and that's right. But it's good idea to spilt up the data out of the
template files so you could easily change the css file used in `template.hbs` when you need to by changing it's value in
`content.toml` insted of changing it in `template.hbs` since `content.toml` is more readable.

It's also good idea to do so if you plane to share your page with people that doesn't understand HTML markup language.

Finally we have `posts` helper function and it's accept a number as its argument, it return the a list of posts as HTML
list, and this is done by useing `partial.hbs` file that belong to **Post** page to construct a that HTML list.

By doing this we was able to include a list that contains our last 10 posts without haveing much trouble.

In our example we use it this way:
``` html
{{ posts 10 }}
```

And the returned value can not be told until we write `partial.hbs` file for **Post** page.

##### About Me Page
Just like **Home** page we will first fillup `content.toml` file, and we could write something like this:
``` toml
[config]
# Change this to 'static' to get static page.
type = "static"

[content]
css-file = dark.css
language = "en"

title = "About Me"
auther = "Muhannad Alrusayni"
description = """
I can write what ever description that I want
in multiline string
"""
email = "muhannad.alrusayni@gmail.com"
skills = [ "Prgramming", "UI Design", "Photography", "3D Modeling" ]
```

Now let's fillup our `template.hbs` file, with the following:
``` html
<!doctype html>
<html lang="{{ language }}">
    <head>
        <meta charset="UTF-8" />
        {{ use-css (css-file) }}
        <title>{{ title }}</title>
    </head>
    <body>
        <div class="nav-bar">
            <ul class="nav-menu">
                <li class="nav-item"><a class="nav-link" href="{{ link home }}">{{ page-title home }}</a></li>
                <li class="nav-item"><a class="nav-link" href="{{ link about-me }}">{{ page-title about-me }}</a></li>
            </ul>
        </div>
        <div>
            <h1>I'm {{ auther }}</h1>
            <p>{{ description }}</p>
            <p>Skills: {{ list (skills) }}</p>
            <p>Cantact Me: {{ email }}</p>
        </div>
    </body>
</html>
```

We got new helper function called `list`, this function accept key with value type
[Array](https://github.com/toml-lang/toml#array).

TODO: STOPED HERE

---

So we can create our **About Me** page by running:
``` sh
$ glittery --blog --new-page about-me
```

this will create a new folder that have three files:
``` sh
pages-path
└── about-me
    ├── partial.hbs
    ├── template.hbs
    └── content.toml
```

and these files will have the default values.

`content.toml` will look like this:

``` toml
[config]
# this value will tell 'glittery' when to build the page. 
# set this value to 'static', if you want 'glittery' build the page only when the page's files modified
# or 'dynamic' to build this page every time when visitors request it's visited
type = dynamic
```

`template.hbs` will look like this:
``` html
<!doctype html>
<html lang="{{config.language}}">
    <head>
        <meta charset="UTF-8"/>
        <link rel="stylesheet" href="{{link config.css-file}}">
        <title>{{config.title}}</title>
    </head>
    <body>
        <div class="about-me">

        </div>	
    </body>
</html>
```

`partial.hbs` will contain:
``` html
<div class="{{config.page-id}}">

</div>
```

Now we start edting these files to suite our need, and we will start by adding some key/value pairs in `content.toml` file, so it
would looks like this:
p``` toml
[config]
title = "about-me"

[content]
name = "Muhannad Alrusayni"
description = """
I can write what ever I want here to use it as description in my About Me page.
I can write more than one line to...
"""
email = "muhannad.alrusayni@gmail.com"
```

Then we will add these keys into our `template.hbs`, so it would look like this:
``` html
<!doctype html>
<html lang="{{config.language}}">
    <head>
        <meta charset="UTF-8"/>
        <link rel="stylesheet" href="{{link config.css-file}}">
        <title>{{config.title}}</title>
    </head>
    <body>
        <div class="about-me">
            <h1>I am {{name}}</h1>
            <p>
                {{description}}<br>
                Feel free to <a href="mailto:{{email}}">email</a> me if you have any question.
            </p>
        </div>	
    </body>
</html>
```

And the same goes with `partial.hbs`:


TODO: Stoped here.
``` html
<div class="about-me">
    <h1>I am {{name}}</h1>
    <p>
        {{description}}<br>
        Feel free to <a href="mailto:{{email}}">email</a> me if you have any question.
    </p>
</div>	
```



---

Now that we have done this simple example using about-me page, all other pages follow the same method.

*You may ask how to include or link these pages to my blog pages ?* \
`glittery` provides two helper functions `include` and `link`, that can be used in layout files and give you more
control over page content.

`link` take one argument that is the the file name of the page, so if I want to have link to about-me.toml page and
other pages in Home page, I would write this in home-template.hbs:
``` html
<!doctype html>
<html lang="{{language}}">
    <head>
		<meta charset="UTF-8"/>
		<link rel="stylesheet" href="{{blog.css-path}}{{css-file}}">
		<title>{{title}}</title>
    </head>
    <body style="direction: {{text-direction}}">
		<ul>
			<li><a href="{{link home}}">Home</a></li>
			<li><a href="{{link about-me}}">About Me</a></li>
			<li><a href="{{link search}}">Search</a></li>
			<li><a href="{{link contact}}">Contact</a></li>
		</ul>
		Home page content goes here...
	</body>
</html>
```
And then I select home-template.hbs as layout-file for my home.toml, `glittery` will replace `link` function with desired
link for eash page.

`include` take the same argument as `link` but it include the output from combining content.toml file and it's
`include-layout-file`, so if we want to include our contact.toml content into our home.toml's `layout-file`, we would
end up with layout looks like this:
``` html
<!doctype html>
<html lang="{{language}}">
    <head>
		<meta charset="UTF-8"/>
		<link rel="stylesheet" href="{{blog.css-path}}{{css-file}}">
		<title>{{title}}</title>
    </head>
    <body style="direction: {{text-direction}}">
		Home page content goes here...
		{{include contact}}
	</body>
</html>
```
`glittery` then will look at `include-layout-file` that is in contact.toml file and use it to layout contact.toml.

### Posts
Glittery stores posts and thier related files in subfolders under `posts-path`, every posts most have it's own folder
that contains post.md and info.toml files. 

`post.md` file is written in [CommonMark](http://commonmark.org/) a markup language with two extensions, tables and
footnotes.

`info.toml` file is written in [Toml Minimal Language](https://github.com/toml-lang/toml).

Posts can use CSS to have nicer look, you can add your own CSS file in `css-path` folder and select it through info.toml
file as the examples below will show.

Glittery uses [Handlebars templates](https://handlebarsjs.com/) to arrange post content in different way, and give you
the ability to use your own handlebars templates by adding them in `layouts-path` folder and select your layout through
info.toml as the examples below will show.

Make sure you have default.css in `css-path` since this is the default for new posts or post that didn't selelct
css-file. otherwise `glittery` will give error.

Here is a basic post example:
```
post-path
└── Intro To Glittery
    ├── post.md
    └── info.toml
```
Post content goes into `post.md` file and post info goes into `info.toml` file. Here is the content for them.

post.md:
```
# Introduction
Glittery is **Simple yet powerfull** server that can help you start your blog, cloud or sharing your video library.

# Installtion
you can visit https://github.com/0x3UH4224D/Glittery for more info.
```

info.toml:
```
# This table/section contain all values that belong to the post it self
[post]
# post title
title = "Intro To Glittery"
# post summary
summary = "Introducing Glittery, in short and simple way"

# publishing date
date = 2018-02-14
# if you stil editing this post, you can change this to false, so glittery doesn't publish this post until you make it true.
publish = true

# post tags that will be used by blog search engine.
tags = [ "intro", "blogging", "server" ]
```

Resources that is used in post such as images and video files should be stored in subfolder `resources`, the tree files
then would look like this:
```
post-path
└── Intro To Glittery
    ├── post.md
    ├── info.toml
    └── resources
        ├── glittery-theme.png
        └── how-to-make-theme.webm
```
By doing that we organize our posts in simple yet elgent way. This approach will make it perrty easy to manage posts 
even by using 3rd party apps.

#### Publishing posts
`glittery` makes it pretty easy to publish a posts. let say you want to start a new post with title like **Intro To
Glittery**, we simply run the following:
```
$ glittery --blog --new "Intro To Glittery"
```
glittery will take care of that and add a new folder in `post-path` with default values in info.toml and empty post.md
file, so you can start writing your post. Once you have done wrting your post you can change `publish`'s value to `true`
in info.toml and your post should be available to others.

#### Deleteing posts
Some time you want to delete post, to do that simply run the following:
```
$ glittery --blog --remove "Intro To Glittery"
```
and `glittery` will take care of the rest. **Note** that if there are more than one post that have the same title then
`glittery` will delete them all, so try not to use the same title.

#### Rename posts
When you want to rename post's title, tun the following:
```
$ glittery --blog -rename "Intro To Glittery" "Blogging With Glittery"
```
`glittery` will rename the post "intro To Glittery"'s folder to "Blogging With Glittery" and the `title`'s value in
info.toml to the new name, so you don't need to rename it twice.

*What if there was two post with the same name/title ?* \
`glittery` will not rename any of them and you have to do it manual if you really want two posts with the same title.

#### info.toml
`glittery` need infomation about every posts, so every posts folder should have info.toml that contains infomation
related to that post.

Here is a full lists of key/values pairs that `glittery` understand. \
`[post]` table:

| Key Name       | Type            | Default Value  | commecnt                                                                                                                        |
|----------------|-----------------|----------------|---------------------------------------------------------------------------------------------------------------------------------|
| title          | String          | Empty String   | Post's title, **Note** when title value's empty then `glittery` will ignore it                                                  |
| summary        | String          | Empty String   | Post's summary, that wil be shown around the post's title.                                                                      |
| pinned         | boolean         | false          | Set `true` if you want to pinned this post in top or bottom page, otherwise set it to `false`                                   |
| language       | String          | en             | Post's language, a list of supported language can be found [here](https://www.w3schools.com/tags/ref_language_codes.asp)        |
| text-direction | String          | ltr            | change this value to `rtl` if you want Right-To-Left text direction                                                  |
| date           | Local Date      | current date   | `glittery` will add current date for the local system                                                                           |
| publish        | Boolean         | false          | set `true` to publish the post, otherwise set it to `false`                                                                     |
| tags           | Array of String | [ "untaged" ]  | these tags will be used from search engine in this blog, so posts can be filtered using tags                                    |
| css-file       | String          | default.css    | select css file for this post, this value will be combined with `css-path` and would be `css-path/css-file`                     |

here is an example using some of these key/value pairs.
``` toml
# This table/section contain all values that belong to the post it self
[post]
# post title
title = "مدخل إلى Glittery"
summary = "مقدمة عن Glittery وخصائص وطريقة تشغيلة"
pinned = true
language = "ar"
text-direction = rtl

# publishing date
date = 2018-02-14
# if you stil editing this post, you can change this to false, so glittery doesn't publish this post until you make it true.
publish = true

# post tags that will be used by blog search engine.
tags = [ "مقدمة", "مقالة", "خادم", "Intro", "server", "artical" ]

# select css file, this value will be combined with `css-path` and would be `css-path/css-file`
css-file = "dark-theme.css"
```

### CSS your posts
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
| Name                 | comment                                                                                          |
|----------------------|--------------------------------------------------------------------------------------------------|
| blog                 | All configuration values that effect blog                                                        |
| language             | All configuration values that effect languages support                                           |
| security             | All configuration values that effect the security in this server                                 |
| security.passwords   | All configuration values that effect how to encrypt and hash passwords to store them in database |
| security.argon2i     | All parameters to be used with argon2i algorithm                                                 |

### Content
| Section            | Key Name         | Type             | Default Value               | commecnt                                                                                                                                                                 |
|--------------------|------------------|------------------|-----------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| blog               | public-path      | String           | blog                        | Path for the blog in the domain, the defualt will be localhost/blog if change this value it will be localhost/NEWPATH                                                    |
| blog               | posts-path       | String           | $HOME/.glittery/blog/posts/ | this folder will contain public posts                                                                                                                                    |
| blog               | css-path         | String           | $HOME/.glittery/blog/css/   | this folder will contain CSS files                                                                                                                                       |
| blog               | pages-path       | String           | $HOME/.glittery/blog/pages/ | this folder will contain other pages such as `Home` and `About Me` files                                                                                                 |
| language           | numbers          | Array of  String | []                          | enter your language's numbers if prefer to display them insted of English numbers or leave it empty otherwise. Note: array elements must start 0 and end with 9          |
| security.passwords | hasher-algorithm | String           | argon2i                     | Pick one of the supported hash algorithms. current supported hash algorithm is only argon2i                                                                              |
| security.argon2i   | parallelism      | Number           | 1                           | Degree of parallelism (i.e. number of threads), Value must be between 1 and 2^24-1                                                                                       |
| security.argon2i   | iterations       | Number           | 10                          | number of iterations to preform. Increasing this froces hashing to longer. Value must be between 1 and 2^32-1                                                            |
| security.argon2i   | memory-size-kib  | Number           | 4096                        | Amount of memory size to use. Increasing this forces hashing to use more memory in order to thwart ASIC-based attacks. Value must be between (8 * iterations) and 2^32-1 |
| security.argon2i   | secret-key       | String           | Random text                 | Optional secret key . Value must vaild UTF-8 and have number of bytes between 0 and 2^32-1                                                                               |

# Glittery Services
## Cloud System
## Video & Audio Streaming
## Store System
