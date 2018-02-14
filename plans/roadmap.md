# Intro To Glittery
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
| Section            | Key Name         | Type             | Default Value                     | commecnt                                                                                                                                                                 |
|--------------------|------------------|------------------|-----------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| blog               | public-path      | String           | blog                              | Path for the blog in the domain, the defualt will be localhost/blog if change this value it will be localhost/NEWPATH                                                    |
| blog               | posts-path       | String           | $HOME/.glittery/blog/posts/       | this folder will contain public posts                                                                                                                                    |
| blog               | css-path         | String           | $HOME/.glittery/blog/css/         | this folder will contain CSS files                                                                                                                                       |
| blog               | layouts-path     | String           | $HOME/.glittery/blog/layouts/     | this folder will contain layout files                                                                                                                                    |
| blog               | other-pages-path | String           | $HOME/.glittery/blog/other-pages/ | this folder will contain other pages such as `Home` and `About Me` files                                                                                                 |
| language           | numbers          | Array of  String | []                                | enter your language's numbers if prefer to display them insted of English numbers or leave it empty otherwise. Note: array elements must start 0 and end with 9          |
| security.passwords | hasher-algorithm | String           | argon2i                           | Pick one of the supported hash algorithms. current supported hash algorithm is only argon2i                                                                              |
| security.argon2i   | parallelism      | Number           | 1                                 | Degree of parallelism (i.e. number of threads), Value must be between 1 and 2^24-1                                                                                       |
| security.argon2i   | iterations       | Number           | 10                                | number of iterations to preform. Increasing this froces hashing to longer. Value must be between 1 and 2^32-1                                                            |
| security.argon2i   | memory-size-kib  | Number           | 4096                              | Amount of memory size to use. Increasing this forces hashing to use more memory in order to thwart ASIC-based attacks. Value must be between (8 * iterations) and 2^32-1 |
| security.argon2i   | secret-key       | String           | Random text                       | Optional secret key . Value must vaild UTF-8 and have number of bytes between 0 and 2^32-1                                                                               |

## Logging file
TODO
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

## Blog System
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

Make sure you have default.css in `css-path` and default.layout in `layouts-path` since they are the default for new
posts or post that didn't selelct css-file and layout-file explicitly. otherwise `glittery` will give error.

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

| Key Name    | Type            | Default Value  | commecnt                                                                                                                        |
|-------------|-----------------|----------------|---------------------------------------------------------------------------------------------------------------------------------|
| title       | String          | Empty String   | Post's title, **Note** when title value's empty then `glittery` will ignore it                                                  |
| pinned      | boolean         | false          | Set `true` if you want to pinned this post in top or bottom page, otherwise set it to `false`                                   |
| language    | String          | en             | Post's language, a list of supported language can be found [here](https://www.w3schools.com/tags/ref_language_codes.asp)        |
| rtl         | boolean         | false          | set `true` if your language is from right to left, otherwise set it to `false`                                                  |
| date        | Local Date      | current date   | `glittery` will add current date for the local system                                                                           |
| publish     | Boolean         | false          | set `true` to publish the post, otherwise set it to `false`                                                                     |
| tags        | Array of String | [ "untaged" ]  | these tags will be used from search engine in this blog, so posts can be filtered using tags                                    |
| css-file    | String          | default.css    | select css file for this post, this value will be combined with `css-path` and would be `css-path/css-file`                     |
| layout-file | String          | default.layout | select layout file to layout this post, this value will be combined with `layouts-path` and would be `layouts-path/layout-file` |

here is an example using some of these key/value pairs.
``` toml
# This table/section contain all values that belong to the post it self
[post]
# post title
title = "مدخل إلى Glittery"
pinned = true
language = "ar"
rtl = true

# publishing date
date = 2018-02-14
# if you stil editing this post, you can change this to false, so glittery doesn't publish this post until you make it true.
publish = true

# post tags that will be used by blog search engine.
tags = [ "مقدمة", "مقالة", "خادم", "Intro", "server", "artical" ]

# select css file, this value will be combined with `css-path` and would be `css-path/css-file`
css-file = "dark-theme.css"
# select layout file, this value will be combined with `layouts-path` and would be `layouts-path/layout-file`
layout-file = "simple-ui.layout"
```

### Other pages
Pages such as `Home`, `About Me`, `Search`, `Contact` is similer to posts pages they use CSS and Layout, but they
differ in the way they are represented. They are stored in `other-pages-path` and using Toml files, the tree files
then would look like this:
```
other-pages-path
├── home.toml
├── about-me.toml
├── search.toml
└── contact.toml
```
Every file of these should have at less section/table called `[page]`, and that section can have any key/value pairs of
the following list:

| Key Name    | Type    | Default Value  | commecnt                                                                                                                        |
|-------------|---------|----------------|---------------------------------------------------------------------------------------------------------------------------------|
| title       | String  | Empty String   | Post's title, **Note** when title value's empty then `glittery` will ignore it                                                  |
| language    | String  | en             | Post's language, a list of supported language can be found [here](https://www.w3schools.com/tags/ref_language_codes.asp)        |
| rtl         | boolean | false          | set `true` if your language is from right to left, otherwise set it to `false`                                                  |
| css-file    | String  | default.css    | select css file for this post, this value will be combined with `css-path` and would be `css-path/css-file`                     |
| layout-file | String  | default.layout | select layout file to layout this post, this value will be combined with `layouts-path` and would be `layouts-path/layout-file` |

You can also defind your own key/value pairs in separate section/table called `[other]` and use these keys you have
defind in your layout files to represent thier values.

Here is an example of `about-me.toml` file:
```
[page]
title = "About Me"
language = "en"

# I use separate layout file for home page
layout-file = "home.layout"

# in this section I can defind whatever key/value pairs I want to represent using home.layout
[other]
name = "Author Name"
email = "author@example.org"
description = """
Here is my description that I want people see in About Me page
I can write what ever I want.
"""
```

### Layout your posts
### CSS your posts
# Glittery Services
## Cloud System
## Video & Audio Streaming
## Store System
