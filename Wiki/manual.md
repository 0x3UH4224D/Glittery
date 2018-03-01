# Website Directory Structure
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

**`website.toml`** \
This is where Glittery's websites stores thier information and configuration, and it's required.

**`layouts`** \
Store templates that's used to construct the website. These templates specify how your website's pages will be
rendered.

**`themes`** \
Store CSS files that will be used by this website.

**`pages`** \
All pages for the website will live inside this directory, each page must have subfolder in `pages` folder. so if we
have `about-me` page then it would be stored in `pages/about-me` folder.

**`.status`** \
This folder is created when a website get build for the first time, it contains the necessary files to keep track of the
website, such as log files, visitors informations and many other things. Users doesn't need to create or edit this
folder or it's content, since it's created and managed by Glittery itself.

**`resources`** \
All resources that is shared between pages in the website lives here, such as videos, images, pdf files or any other
resource you want to use in your website.

## website.toml File
As we knew, this file contains all the configuration that effect the website. It also contains information about the
website. It's written using [Toml Minimal Language](https://github.com/toml-lang/toml).

There are several tables we can use in this file, `[info]`, `[config]`, `[server-config]` and `[extra]`.

The `[info]` table is meant to contain the information for the website, here is all key/value pairs that can be used
inside this table:

| Key Name     | Type                                                  | Required | Default Value         | commecnt                                                          |
|--------------|-------------------------------------------------------|----------|-----------------------|-------------------------------------------------------------------|
| title        | String                                                | Yes      | `website-id`          | Title for the website                                             |
| date         | `Offset Date-Time`, `Local Date-Time` or `Local Date` | No       |                       | website creation date                                             |
| summary      | String                                                | No       |                       | Summary for the website                                           |
| author       | String                                                | No       |                       | The author name for this website                                  |
| author-email | String                                                | No       |                       | Your email address where problems with website should be e-mailed |
| categories   | Array of String                                       | No       | `[ "Uncategorized" ]` | Categories this website belong to                                 |
| keywords     | Array of String                                       | No       |                       | Keywords for the website                                          |

TODO: Add full description for each key and its purpose

The `[extra]` table is meant to contain key/value pairs that is defined by the owner of the website, and these
key/value pairs can be accessed later from layouts files.

The `[config]` table is meant to contain the configuration for the website, here is all key/value pairs that can be used
inside this table:

| Key Name      | Type   | Required | Default Value     | commecnt                                                                                                    |
|---------------|--------|----------|-------------------|-------------------------------------------------------------------------------------------------------------|
| language-code | String | No       | en                | Website language, (list of [language code](https://www.w3schools.com/tags/ref_language_codes.asp)) |
| theme         | String | No       |                   | Theme to use it with layout files to make your website look better                                          |

TODO: Add full description for each key and its purpose

The `[server-config]` table is meant to contain the configuration for the server that will publish our website,
`glittery` in our case, here is all key/value pairs that can be used inside this table:

| Key Name | Type   | Required | Default Value | commecnt                                  |
|----------|--------|----------|---------------|-------------------------------------------|
| port     | number | No       | `8080`        | Listen on the given port                  |
| host     | String | No       | `localhost`   | Listen at the given hostname              |
| base-url | String | No       |               | Serve the website from the given base URL |

TODO: Add full description for each key and its purpose

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

The minimal structure is:

``` sh
.
├── layouts
│   └── template
│       ├── info.toml
│       └── template.hbs
...
```

### info.toml File
This file is similar to `website.toml` file, but it's for the layout not the website, it only have two tables, `[config]`
and `[extra]`.

The `[config]` table is meant to contain the configuration for the layout, here is all key/value pairs that can be used
inside this table:

| Key Name | Type    | Required | Default Value | commecnt                                  |
|----------|---------|----------|---------------|-------------------------------------------|
| type     | String  | Yes      | `base`        | Type of the template                      |
| dynamic  | Boolean | No       | `false`       | Whether the template is dynamic or static |

TODO: Add full description for each key and its purpose

The `[extra]` table is meant to contain key/value pairs that is defined by the creator of the template, and these
key/value pairs can be accessed from `template.hbs` file.

### resources Folder
This folder stores resources that is only related to this template, and can be accessed from `template.hbs` file.

### template.hbs File
The main file that every template must have, it's written using [Handlebars templates](https://handlebarsjs.com/), it's
meant to tell Glittery how to generate HTML pages from the website content.

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

TODO: Explain what is accessable to templates
TODO: Add way to access all pages content, something like `{{ pages.home.title }}`

#### Helpers
[Handlebars templates](https://handlebarsjs.com/) have something called helpers, thier purpose is to make it easy to you
to write good template, Glittery ships with a lot of usefull helpers that you can use when you write your templates.

The following are helpers that come with Glittery and thier documentation.

`include partial-id` \
This helper is used by Base Templates to include Partial Templates in thier `template.hbs` file. The arguments should be
`partial-id`.

If the partial-id doesn't exist, Glittery will add warning to the log files, and the Base template will not be useable
since it doesn't find partial template that it need.

`link id` \
This helper return relaitve link for pages and resouces in the website. The arguments should be `page-id`, `resouces-id`
and `css-id`.

If the id that was passed to the helper doesn't exist, Glittery will add warning to the log files, and the returned
value will be empty text.

TODO: Add more helpers.

## pages Folder
This folder contains all pages in the website, Every website must have at less one page wich is `Home` page, this page
is the main page for the website and its special page.

Special pages are just like any normal page but the only different is that Glittery handle them a bit more differently.
Here are all the special pages that website can have:

`home` \
The Home page for the website, it's the first page that visitor see when they visit your website.

`url-note-found` \
This page will be used when visitor try to visit URL that doesn't exist.

Every page in Glittery must be in subfolder, that subfolder can have the following structure:

``` sh
.                         # Website root folder
├── pages                 # Pages root folder
│   └── page              # Page folder, its name is the 'page-id'
│       ├── content.md    # The content of the page written in Markdown
│       ├── info.toml     # info and config for this page
│       ├── resources     # Resources for this page
│       └── .status       # info about this page goes here
```

The minimal structure is:

``` sh
.
├── pages
│   └── page
│       ├── info.toml
│       └── .status
```

### content.md File
This file is written using [CommonMark](http://commonmark.org/) (aka Markdown) and meant to contain big piece of content
that will goes into the template. Pages that use simple template likely will want use this file to put thier content,
such as `Posts`, `Docs` and `Articals` pages. Pages that use complex template such as `Home`, `News` and `Contact us`
will likely to put thier content inside `info.toml` file.

Glittery uses two CommonMark extensions wich are Tables and Footers, so users can write tables and footers in this file,
one more thing is you can use the helper `link` that we talked about in [Helpers chapter](./manual.md#helpers) to get
link for resouces in this website.

This file will be accessable from `Base Template`s by writing `{{ content }}`. Before that it will go through two
phases:
1. This file will be passed to Handlebars preprocessor to replace any use for `link` helper with a proper URL.
2. The result of the first step will be passed to CommonMark preprocessor to produce and HTML chunk that can be used by
   template.

So when you write `{{ content }}` in your template you will get the result of the second phase two.

### info.toml Files
This file is similar to other `.toml` files we have seen earlier, but it's for pages, there are three tables, `[info]`,
`[config]` and `[extra]`.

The `[info]` table is meant to contain the information for the page, here is all key/value pairs that can be used inside
this table:

| Key Name     | Type                                                  | Required | Default Value         | commecnt                       |
|--------------|-------------------------------------------------------|----------|-----------------------|--------------------------------|
| title        | String                                                | No       | website.title         | Page title                     |
| date         | `Offset Date-Time`, `Local Date-Time` or `Local Date` | No       |                       | Page creation date             |
| summary      | String                                                | No       |                       | Page summary                   |
| author       | String                                                | No       | website.author        | The author name for this page  |
| author-email | String                                                | No       | website.author-email  | The author email for this page |
| categories   | Array of String                                       | No       | `[ "Uncategorized" ]` | Categories this page belong to |
| tags         | Array of String                                       | No       | `[ "Untagged" ]`      | Tags for the page              |

TODO: Add full description for each key and its purpose

The `[config]` table is meant to contain the configuration for the page, here is all key/value pairs that can be used
inside this table:

| Key Name      | Type    | Required | Default Value | commecnt                                                                                        |
|---------------|---------|----------|---------------|-------------------------------------------------------------------------------------------------|
| draft         | Boolean | No       | `false`       | whether this page is draft or not                                                               |
| archived      | Boolean | No       | `false`       | whether this page is archived or not                                                            |
| layout        | String  | Yes      |               | Page template, write `layout-id` here.                                                          |
| language-code | String  | No       | `en`          | Page language, (list of [language code](https://www.w3schools.com/tags/ref_language_codes.asp)) |

TODO: Add full description for each key and its purpose

### resources Folder
This folder stores resources that is only related to this page, and can be accessed from `content.md` file.

### .status Folder
This folder is created when this page get build for the first time, it contains the necessary files to keep track of the
page, such as log files for the page, visitors informations and many other things. Users doesn't need to create or edit
this folder or it's content, since it's created and managed by Glittery itself.

TODO: Add more info about the files in this folder and thier contents

## resouces Folder
This folder stores global resources that is shared in the website, and can be accessed from all it's content.

TODO: Add more details about this folder and how it's content are managed by Glittery.

## themes Folder
TODO

## .status Folder
This folder is created when a website get build for the first time, it contains the necessary files to keep track of the
website, such as log files, visitors informations and many other things. Users doesn't need to create or edit this
folder or it's content, since it's created and managed by Glittery itself.

TODO: Add more info about the files in this folder and thier contents

## Page, Layout, Resouce and Theme ID
Glittery can identify pages, layouts, resouces and themes by using thier ID. The ID is the name of the folder that
contains them, for example, the `page-id` for `About Me` page with the following website directory structure:

``` sh
.                         # Website root folder
├── pages                 # Pages root folder
│   └── about-me          # Page folder, its name is the 'page-id'
│       ├── info.toml
│       └── .status
...
```

So the `page-id` would be `about-me` in this case. The same concept for other content in the website:

``` sh
.                         # Website root folder
├── layouts
│   ├── about-me          # 'layout-id' = 'about-me'
│   │   ├── resources
│   │   │   └── me.png    # 'resouce-id' = 'me.png'
│   │   └── ...
│   └── home              # 'layout-id' = 'home'
│       └── ...
├── pages
│   ├── about-me          # 'page-id' = 'about-me'
│   │   └── ...
│   └── home              # 'page-id' = 'home'
│       └── ...
├── resouces
│   └── logo.png          # 'resouce-id' = 'logo.png'
├── themes
│   ├── light-moon        # 'theme-id' = 'light-moon'
│   │   └── ...
│   └── sky-waves         # 'page-id' = 'home'
│       └── ...
...
```
## Visiblity Scope
TODO

## Website map
TODO

# Glittery Command Line Interface
Glittery have a powerfull and user-friendly Command-Line interface, it does all what you need to create and build a
website, the following are all commands you can use with `glittery` and a thier arguments with short description for
thier purpose.

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
    new           Create a new website, page, layout or theme folder with all necessary files
    build         Build the current website
    start         Build the website and start a server for it
    check         Analyze the current website and report errors without build the website
    clean         Remove all .status directories in current website
    remove        Remove a page, layout or theme folder with all its related files
    bench         Run a benchmarks for the website
    info          Print information about the current website's or specific page in it

Run 'glittery help <command>' for more information on a specific command.
```

## new Command
TODO

## build Command
TODO

## check Command
TODO

## clean Command
TODO

## remove Command
TODO

## start Command
TODO

## bench Command
TODO

## info Command
TODO
