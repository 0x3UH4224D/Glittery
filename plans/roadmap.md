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
| Section            | Name             | Type             | Default Value                 | commecnt                                                                                                                                                                 |
|--------------------|------------------|------------------|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| blog               | public-path      | String           | blog                          | Path for the blog in the domain, the defualt will be localhost/blog if change this value it will be localhost/NEWPATH                                                    |
| blog               | posts-path       | String           | $HOME/.glittery/blog/posts/   | this folder will contain public posts                                                                                                                                    |
| blog               | css-path         | String           | $HOME/.glittery/blog/css/     | this folder will contain CSS files                                                                                                                                       |
| blog               | layouts-path     | String           | $HOME/.glittery/blog/layouts/ | this folder will contain layout files                                                                                                                                       |
| language           | numbers          | Array of  String | []                            | enter your language's numbers if prefer to display them insted of English numbers or leave it empty otherwise. Note: array elements must start 0 and end with 9          |
| security.passwords | hasher-algorithm | String           | argon2i                       | Pick one of the supported hash algorithms. current supported hash algorithm is only argon2i                                                                              |
| security.argon2i   | parallelism      | Number           | 1                             | Degree of parallelism (i.e. number of threads), Value must be between 1 and 2^24-1                                                                                       |
| security.argon2i   | iterations       | Number           | 10                            | number of iterations to preform. Increasing this froces hashing to longer. Value must be between 1 and 2^32-1                                                            |
| security.argon2i   | memory-size-kib  | Number           | 4096                          | Amount of memory size to use. Increasing this forces hashing to use more memory in order to thwart ASIC-based attacks. Value must be between (8 * iterations) and 2^32-1 |
| security.argon2i   | secret-key       | String           | Random text                   | Optional secret key . Value must vaild UTF-8 and have number of bytes between 0 and 2^32-1                                                                               |

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
that contains the post.md and info.toml files. 

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
# post section
[post]
# post title
title = "Intro To Glittery"
# publishing date
date = 1979-05-27
# post tags that will be used by blog search engine.
tags = [ "intro", "blogging", "server" ]
# select css file, this value will be combined with `css-path` and would be `css-path/css-file`
css-file = "default.css"
# select layout file, this value will be combined with `layouts-path` and would be `layouts-path/layout-file`
layout-file = "default.layout"

# auher section
[author]
name = "Muhannad Alrusayni"
email = "0x3uh4224d@gmail.com"
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
There are two way to publish a posts in bolg, one would be using CL `glittery` and the other would be
using the web interface, both are explained briefly below.

*using `glittery`* \
run:
```
$ glittery --blog --new-post [PATH_TO_POST_FOLDER]
```

*using web interface* \
**TODO:** will be added when web interface is ready.

#### Managing posts
TODO
### Other pages
Other pages such as `Home`, `About me` is just like posts pages they use CSS and Layout, but one different is that 
they have separated layout files one for each 
### Layout your posts
### CSS your posts
# Glittery Services
## Cloud System
## Video & Audio Streaming
## Store System
