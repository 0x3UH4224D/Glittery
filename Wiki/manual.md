# Introduction
This manual will explain Glittery Services in details, if this your first time with Glittery have look at
[Intro To Glittery](./Intro-To-Glittery) since it provides complete examples and HowTo things. This manual will go in
more details with a few short examples so you need to have an idea of what's going on with Glittery to understand this
manual.

This manual will talk about `glittery` the command-line and these files that Glittery can understand in details.

`glittery` command-line can be used this way:
`glittery <service> <operation> [ARGUMENT] [OPTION]...`

One exception is that help message will be shown if the user didn't invoke `glittery` correctly.

There are two kind of help message, the main one that could be shown:
`$ glittery --help`
And the other one is for specific `<service>`, and we could get it by:
`$ glittery <service> --help`

## Blog Service
Users can manage and communicate with blog service using `glittery` command-line interface, you can get help message by
running `glittery --blog --help`, all arguments that belong to blog service should come after `--blog`.

Here is a table that contains all `<operation>`s for the Blog Service:

| Operation       | Argument    | Action                                                         |
|-----------------|-------------|----------------------------------------------------------------|
| `--build`       |             | Create `.status` folder for each modified or new page and post |
| `--build-all`   |             | Recreate `.status` folder for all pages and posts in the blog  |
| `--new-page`    | `[PAGE_ID]` | Create a new page and its related files                        |
| `--remove-page` | `[PAGE_ID]` | Remove page and its related files                              |
| `--info-page`   | `[PAGE_ID]` | Show info about this page                                      |
| `--build-page`  | `[PAGE_ID]` | Create `.status` folder and its files                          |
| `--new-post`    | `[POST_ID]` | Create a new post folder and its related files                 |
| `--remove-post` | `[POST_ID]` | Remove post and its related files                              |
| `--info-post`   | `[POST_ID]` | Show info about this post                                      |
| `--build-post`  | `[POST_ID]` | Create `.status` folder and its files                          |

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
There are two tables `[config]` and `[content]`. the `[content]` table can have any key/value pairs. while the
`[config]` table can have the following kay/value pairs:

Here is a full lists of key/value pairs that `glittery` understand. \

`[config]` table:
| Key Name | Type            | Required | Default Value | commecnt                                                                                         |
|----------|-----------------|----------|---------------|--------------------------------------------------------------------------------------------------|
| publish  | boolean         | Yes      | false         | If this is `false` this post won't be published, set it to `true` if you want it to be published |
| date     | Local Date      | Yes      | current date  | The publishing date for this post                                                                |
| tags     | Array of String | No       | [ "Untaged" ] | These tags will be used from search engine in this blog, so posts can be filtered using them     |

The key name `post-id` is already reserved in `[config]` table, and it's value is the post id (the post folder's name).
