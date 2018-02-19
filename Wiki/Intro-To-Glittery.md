# Intro To Glittery
TODO
## Blog Service
Glittery provides a blogging service that helps you make your own blog in simple yet powerfull way. It gives you the
ability to write custom pages such as `About Me` and `Search` pages using 
[Handlebars templates](https://handlebarsjs.com/), with a tons of helper functions.

Glittery let its users write thier posts in [CommonMark](http://commonmark.org/) and convert these posts to HTML files,
it also give its users the ability to customize each posts using 
[Toml Minimal Language](https://github.com/toml-lang/toml)

Both pages and posts can use CSS files to customize thier look and feel.

Users can manage and communicate with blog service using `glittery` command-line interface, you can get help message by
running `glittery --blog --help`, all arguments that belong to blog service should come after `--blog`.

The following sections will explain how to use Glittery Blog Service.
### Pages
Glittery let you write your own pages using [Toml Minimal Language](https://github.com/toml-lang/toml) and
[Handlebars templates](https://handlebarsjs.com/), that will give you the ability to write customized pages.

Every page should have a folder that contains three files, and the name of that folder is the `id` for that page, this
folder should be stored in `pages-path` folder, so if we want to create **About Me** page, its tree files would look
like this:
``` sh
pages-path
└── about-me                  # The page's folder, it's name is the 'id' for this page
    ├── partial.hbs
    ├── page.hbs
    └── content.toml
```

Page's `id` can be any unicode character except those are not considered valid by
[Identifiers in handlebars](https://handlebarsjs.com/expressions.html), Glittery will ignore pages with `id` that
contain any of them.

All these files are required to construct an HTML page, and any page that does miss any of them will be ignored by
Glittery. These files will be explained in the following lines.

#### content.toml File
The content of the page goes in `content.toml`, this file is written using
[Toml Minimal Language](https://github.com/toml-lang/toml) wich contain two tables `[config]` and `[content]`.

The `[config]` table is **required** and is ment to configure the page, the following key/value pairs can be used inside
this table:

| Key Name | Type   | Required | Default Value | commecnt                                                                                                   |
|----------|--------|----------|---------------|------------------------------------------------------------------------------------------------------------|
| url      | String | No       | `page-id`     | this is the url for this page, so value like 'about-me' would have url like http://localhost/blog/about-me |
| type     | String | No       | dynamic       | this is the type of the page,                                                                                |

**Note**: Glittery will ignore pages that doesn't fill required key/values pairs such as `title` or pages that miss
`[config]` table.

**Note**: the key name `page-id` is  already reserved, and it's value is the page id (the page folder's name). So you
can use that name for any key in `[config]` table.

The `[content]` table is **optional** and is ment to contain the page content that will appaer in the HTML page, users can
define thier own key/value pairs freely in this table and use these keys inside `page.hbs` and `partial.hbs` to
represent thier 

**Info**: There is no meaning to have page without `[content]` table since the HTML page won't have any content.

**Note**: Keys names may be any unicode character except those considered invalid characters by [Identifiers in
handlebars](https://handlebarsjs.com/expressions.html) in thier key names. and Glittery will ignore any key that have
invalid character and it's value won't be accessable by `page.hbs` and `partial.hbs`.

#### page.hbs & partial.hbs Files
there are two others files we didn't talk about yet, these files are
written using [Handlebars templates](https://handlebarsjs.com/) with `.hbs` extension.

These two files have the same purpose wich is describing how the values in `content.toml` file will appaer in HTML page,
but they are used in different way, `page.hbs` together with `content.toml` used to construct a standalone HTML page,
while `partial.hbs` together with `content.toml` used to create an HTML chunk that can be embedded into other pages
in thier `page.hbs` file, and this could be done by adding `{{> [PAGEID]-partial PAGEID }}` (this will be covered in
details in next chapters).

#### Page Resources
Users can easily add `resources` in thier pages and that could be done by adding a new folder called resources in the
page's folder, and put what ever resources they want to use in thier page inside `resources` folder, such as images and
videos.

The **About Me**'s tree files would look like this when we add some resources:
``` sh
pages-path
└── about-me
    ├── partial.hbs
    ├── page.hbs
    ├── content.toml
    └── resources
        ├── me.png
        └── CV.pdf
```

#### Example of how to create 'About Me' page
Good news that `glittery` command-line interface can take care of creating all files and folders we need to start wrting
our pages, and that could be done by running:
``` sh
$ glittery --blog --new-page [PAGE_ID]
```

So we can create our **About Me** page by running:
``` sh
$ glittery --blog --new-page about-me
```

this will create a new folder that have three files:
``` sh
pages-path
└── about-me
    ├── partial.hbs
    ├── page.hbs
    └── content.toml
```

and these files will have the default values.

`content.toml` will look like this:
``` toml
[config]
title = "about-me"
```

`page.hbs` will look like this:
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
``` toml
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

Then we will add these keys into our `page.hbs`, so it would look like this:
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
other pages in Home page, I would write this in home-page.hbs:
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
And then I select home-page.hbs as layout-file for my home.toml, `glittery` will replace `link` function with desired
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
