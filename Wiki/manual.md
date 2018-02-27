# Directory Structure
Every website root folder in Glittery should have the following structure:
``` sh
.
├── layouts
├── pages
├── resources
├── themes
├── website.toml
└── .status
```

## Overview
Here is an overview for each directory and file.

**`website.toml`**
This is where Glittery's websites stores thier information and configuration, and it's required. Users doesn't need to
create this folder manually, `glittery` command will do this for you when you run `glittery new [website-id]` and fill
it with the default values

**`layouts`**
Store templates that's used to construct a website. These templates specify how your website's pages will be
rendered. There are two types of templates partial templates and single page templates.

**`themes`**
Store CSS files that will be used by this website.

**`pages`**
All pages for your website will live inside this directory, each page must have subfolder in `pages` folder. so if we
have `about-me` page then it would be stored in `pages/about-me` folder.

**`.status`**
This folder is created when we build a website for the first time, it contains the necessary files to keep track of the
website, such as log files, visitor informations and many other things. Users doesn't need to create or edit this folder
or it's content, since it's created and managed by Glittery itself.

**`resources`**
All resources that is shared between pages in the website lives here, such as videos, images, pdf files or any other
resource you want to use in your website.

## website.toml File
As we knew, this file contains all the configuration that effect the website. It also contains information about the
website. It's written using [Toml Minimal Language](https://github.com/toml-lang/toml).

There are several tables we can use in this file, `[info]`, `[config]`, `[server-config]` and `[extra]`.

The `[info]` table is meant to contain the information for the website, here is all key/value pairs that can be used
inside this table:

| Key Name     | Type            | Required | Default Value       | commecnt                                                          |
|--------------|-----------------|----------|---------------------|-------------------------------------------------------------------|
| title        | String          | Yes      | `website-id`        | Title for the website                                             |
| summary      | String          | No       |                     | Summary for the website                                           |
| author       | String          | No       |                     | The author for the website                                        |
| author-email | String          | No       | author@example.org  | Your email address where problems with website should be e-mailed |
| categories   | Array of String | No       | [ "Uncategorized" ] | Categories this website belong to                                 |
| tags         | Array of String | No       | [ "Untagged" ]      | Tags for the website                                              |

The `[extra]` table is meant to contain key/value pairs that is defined by the owner of the website, and these
key/value pairs can be accessed later from layouts files.

The `[config]` table is meant to contain the configuration for the website, here is all key/value pairs that can be used
inside this table:

| Key Name      | Type   | Required | Default Value     | commecnt                                                                                                    |
|---------------|--------|----------|-------------------|-------------------------------------------------------------------------------------------------------------|
| language-code | String | No       | en                | Website language, (list of [language code](https://www.w3schools.com/tags/ref_language_codes.asp)) |
| theme         | String | No       |                   | Theme to use it with layout files to make your website look better                                          |

The `[server-config]` table is meant to contain the configuration for the server that will publish our website,
`glittery` in our case, here is all key/value pairs that can be used inside this table:

| Key Name | Type   | Required | Default Value | commecnt                                  |
|----------|--------|----------|---------------|-------------------------------------------|
| port     | number | No       | 8080          | Listen on the given port                  |
| host     | String | No       | localhost     | Listen at the given hostname              |
| base-url | String | No       |               | Serve the website from the given base URL |

## layouts Folder
This folder contains all the templates that is used to construct a website. There are two types of template, `partial`
template and `base` template. Every template must be in subfolder, that subfolder have the following structure:

``` sh
.                           # Website root folder
├── layouts                 # Templates root folder
│   └── template            # Template folder, its name is the 'layout-id'
│       ├── info.toml       # info and config for this template
│       ├── resources       # Resources for this template
│       └── template.hbs    # Template file that describe how to render the HTML page
...
```

### info.toml File
This file is similar `website.toml` file, but it's for the layout not the website, it only have two tables, `[config]`
and `[extra]`.

The `[config]` table is meant to contain the configuration for the layout, here is all key/value pairs that can be used
inside this table:

| Key Name | Type    | Required | Default Value | commecnt                                  |
|----------|---------|----------|---------------|-------------------------------------------|
| type     | String  | Yes      | base          | Type of the template                      |
| dynamic  | Boolean | No       | false         | Whether the template is dynamic or static |

The `[extra]` table is meant to contain key/value pairs that is defined by the creator of the template, and these
key/value pairs can be accessed from `template.hbs` file.

### resources Folder
This folder stores resources that is only related to this template, can accessed from `template.hbs` file.

### template.hbs File
The main file that every template must have, it's written using [Handlebars templates](https://handlebarsjs.com/), it's
meant to describe how to render an HTML page by accessing the template files and the page files

**`Base Template`**
This is the main type of the templates, it's meant to build a single HTML page. You gain access to three sources:
- `info.toml` that belong to this template; You can access its content by writing something like
  `{{ template-id.KEY }}`.
- `info.toml` that belong to the page that use this template; You can access its content by writing something like
  `{{ page.KEY }}`.
- `content.md` that belong to the page that use this template; You can access its content by writing something like
  `{{ content }}`.

**`Partial Template`**
This type of template is meant to be included into `Base Templates`, it's used to split common parts in `Base Template`s
so we can reuse this part in more than one `Base Template`. A good example is header and footer partial template. You
gain access to two sources:
- `info.toml` that belong to this template (the partial template file); You can access its content by writing something
  like `{{ template-id.KEY }}`.
- `info.toml` that belong to the `Base Template` that use this partial template; You can access its content by writing
  something like `{{ base.KEY }}`.

#### Helpers
[Handlebars templates](https://handlebarsjs.com/) have something called helpers, thier purpose is to make it easy to you
to write good template, Glittery ships with a lot of usefull helpers that you can use when you write your templates.

The following are helpers that come with Glittery and thier documentation.

`include partial-id`
This helper is used by Base Templates to include Partial Templates in thier `template.hbs` file. The arguments should be
`partial-id`.

If the partial-id doesn't exist, Glittery will add warning to the log files, and the Base template will not be useable
since it doesn't find partial template that it need.

`link id`
This helper return relaitve link for pages and resouces in the website. The arguments should be `page-id`, `resouces-id`
and `css-id`.

If the id that was passed to the helper doesn't exist, Glittery will add warning to the log files, and the returned
value will be empty text.

TODO: Add more helpers.

## pages Folder
TODO
## resouces Folder
TODO
## themes Folder
TODO
## .status Folder
TODO

# Glittery Command Line Interface
Glittery have a powerfull Command-Line interface, it does all what you need to create and build a website, the following
are all commands you can use with `glittery` and a thier arguments with short description for thier purpose.

``` sh
$ glittery help
Usage:
    glittery <command> [<args>...]
    glittery [options]
    
Options:
    -h, --help       Display this message
    -V, --version    Print version info and exit
    -v, --verbose    Print extra info when running a command

Commands:
    build         Build the current website
    check         Analyze the current website and report errors without build the website
    clean         Remove all .status directories in current website
    new           Create a new website, page, layout or theme folder with all necessary files
    remove        Remove a page, layout or theme folder with all its related files
    run           Build the website and start a server for it
    bench         Run a benchmarks for the website
    info          Print information about the current website's or specific page in it 
    set           Set a new value for specific key in the configuration file website.toml
    get           Get a key value from the configuration file website.toml

Run 'glittery help <command>' for more information on a specific command.
```












# Introduction
This manual will explain Glittery Services in details, if this your first time with Glittery have look at
[Intro To Glittery](./Intro-To-Glittery) since it provides complete examples and HowTo things. This manual will go in
more details with a few short examples so you need to have an idea of what's going on with Glittery to understand this
manual.

### Pages
Glittery construct HTML files for pages that have folder in `pages-path`, here are all files that can be stored in the
page's folder:
``` sh
pages-path
└── about-me                  # The page's folder, it's name is the 'id' for this page
    ├── .status               # a folder that contains all other necessary files that belong to this page
    |   ├── data.toml             # file that contain all data that belong to this page
    |   └── page.html             # the HTML page that have been constructed from 'info.toml' and 'template.hbs'
    ├── partial.hbs           # handlebars file that can be used as partial in other 'template.hbs' files
    ├── template.hbs          # handlebars file that will be used with info.toml to construct HTML page
    ├── info.toml             # Toml file that contains the configuration for HTML page and its content
    └── resources             # a folder that contains all resources belong to this page
```

Page's `page-id` can be any unicode character except those are not considered valid by
[Identifiers in handlebars](https://handlebarsjs.com/expressions.html):
`Whitespace` `!` `"` `#` `%` `&` `'` `(` `)` `*` `+` `,` `.` `/` `;` `<` `=` `>` `@` `[` `\` `]` `^` ``` ` ``` `{` `|` `}` `~`

Glittery will ignore pages with `page-id` that contain any of them.

Only `template.hbs`, `partial.hbs` and `info.toml` are required, `resources` folder are optional, `.status` folder will
be created with the first build for the page.

#### info.toml File
This file contains the configuration and the content for HTML page, it's written using
[Toml Minimal Language](https://github.com/toml-lang/toml).

There are two tables `[config]` and `[content]`. the `[content]` table can have any key/value pairs. while the
`[config]` table can have the following kay/value pairs:

| Key Name | Type    | Required | Default Value | commecnt                                                                                                                                                                                                                                   |
|----------|---------|----------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| publish  | boolean | Yes      | false         | If this is `false` this page won't be published, set it to `true` if you want it to be published                                                                                                                                           |
| url      | String  | No       | `page-id`     | This is the url for this page, so value like 'about-me' would have url like http://localhost/blog/about-me                                                                                                                                 |
| type     | String  | No       | dynamic       | This value tell Glittery if this page is 'static' or 'dynamic', set this value to 'static', if you want 'glittery' build the page only when the page's files modified, otherwise this page will be build each time when visitor request it |

The key name `page-id` is already reserved in `[config]`, and it's value is the page id (the page folder's name).

Key name may be any unicode character except those considered invalid characters by [Identifiers in
handlebars](https://handlebarsjs.com/expressions.html). Glittery will ignore any key that have
invalid character and it's value won't be accessable by `template.hbs` and `partial.hbs`.

#### template.hbs & include.hbs Files
These files are written using [Handlebars templates](https://handlebarsjs.com/) with `.hbs` extension.

These two files have the same purpose wich is describing how the values in `info.toml` file will appaer in HTML page,
but they are used in different way, `template.hbs` together with `info.toml` used to construct a standalone HTML page,
while `partial.hbs` together with `info.toml` used to create an HTML chunk that can be embedded into other pages
in thier `template.hbs` file, and this could be done by partial feature in handlebars.

Glittery uses [handlebars-rust](https://github.com/sunng87/handlebars-rust) library, although it has some
[limitation](https://github.com/sunng87/handlebars-rust#limitations) but this may change in the future.

##### Helper Functions
TODO: add all docs for all helper functions in this section

**link [arguments]**


#### Page's Resources
Page's resources can be stored in `reserved` folder.

## Posts
TODO

### Publishing Posts
TODO

### Deleteing Posts
TODO

### Renaming Posts
TODO

### config.toml File

Here is a full lists of key/value pairs that `glittery` understand. \

`[config]` table:
| Key Name | Type            | Required | Default Value | commecnt                                                                                         |
|----------|-----------------|----------|---------------|--------------------------------------------------------------------------------------------------|
| publish  | boolean         | Yes      | false         | If this is `false` this post won't be published, set it to `true` if you want it to be published |
| date     | Local Date      | Yes      | current date  | The publishing date for this post                                                                |
| tags     | Array of String | No       | [ "Untaged" ] | These tags will be used from search engine in this blog, so posts can be filtered using them     |

The key name `post-id` is already reserved in `[config]` table, and it's value is the post id (the post folder's name).
