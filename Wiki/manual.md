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

| Operation       | Argument    | Action                                            | Purpose                                                                                                                                    |
|-----------------|-------------|---------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| `--build`       |             | Construct HTML files for modified pages and posts | This will create HTML files for new pages and posts or if there was any change to them, although `glittery` will create them automatically |
| `--build-all`   |             | Construct HTML files for all pages and posts      | This will recreate HTML files for all pages and posts, even if they wasn't modified                                                        |
| `--new-page`    | `[PAGE_ID]` | Create a new page                                 | This will create a folder in `pages-path` that contains all necessary files to start writing a new page                                    |
| `--remove-page` | `[PAGE_ID]` | Remove page                                       | This will remove a page and all its related files                                                                                          |
| `--info-page`   | `[PAGE_ID]` | Show info about this page                         | This will print info about this page                                                                                                       |
| `--new-post`    | `[POST_ID]` | Create a new post                                 | This will create a folder in `posts-path` that contains all necessary files to start writing a new post                                    |
| `--remove-post` | `[POST_ID]` | Remove post                                       | This will remove a post and all its related files                                                                                          |
| `--info-post`   | `[POST_ID]` | Show info about this post                         | This will print info about this post                                                                                                       |

### Pages
Glittery construct HTML files for pages that have folder in `pages-path`, each page's folder would looks like this:
``` sh
pages-path
└── about-me                  # The page's folder, it's name is the 'id' for this page
    ├── .status               # a folder that contains all data that belong to this page
    |   ├── info.toml             # file that contain all info that belong to this page
    |   └── page.html             # the HTML page that have been constructed from 'content.toml' and 'template.hbs'
    ├── partial.hbs           # handlebars file that can be used as partial in other 'template.hbs' files
    ├── template.hbs          # handlebars file that will be used with content.toml to construct HTML page
    ├── content.toml          # Toml file that contains the configuration for HTML page and its content
    └── resources             # a folder that contains all resources belong to this page
```

Page's `page-id` can be any unicode character except those are not considered valid by
[Identifiers in handlebars](https://handlebarsjs.com/expressions.html):
`Whitespace` `!` `"` `#` `%` `&` `'` `(` `)` `*` `+` `,` `.` `/` `;` `<` `=` `>` `@` `[` `\` `]` `^`
```
`
```
`{` `|` `}` `~`
Glittery will ignore pages with `page-id` that contain any of them.

Only `template.hbs`, `partial.hbs` and `content.toml` are required, `resources` folder are optional, `.status` folder will
be created with the first build for the page.

#### content.toml File
This file contains the configuration and the content for HTML page, it's written using
[Toml Minimal Language](https://github.com/toml-lang/toml).

There are two tables `[config]` and `[content]`. the `[content]` table can have any key/value pairs. while the
`[config]` table can have the following kay/value pairs:

| Key Name | Type   | Required | Default Value | commecnt                                                                                                                                                                                                                                   |
|----------|--------|----------|---------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| url      | String | No       | `page-id`     | this is the url for this page, so value like 'about-me' would have url like http://localhost/blog/about-me                                                                                                                                 |
| type     | String | No       | dynamic       | this value tell Glittery if this page is 'static' or 'dynamic', set this value to 'static', if you want 'glittery' build the page only when the page's files modified, otherwise this page will be build each time when visitor request it |

The key name `page-id` is already reserved in `[config]`, and it's value is the page id (the page folder's name).

Key name may be any unicode character except those considered invalid characters by [Identifiers in
handlebars](https://handlebarsjs.com/expressions.html). Glittery will ignore any key that have
invalid character and it's value won't be accessable by `template.hbs` and `partial.hbs`.

#### template.hbs & include.hbs Files
These files are written using [Handlebars templates](https://handlebarsjs.com/) with `.hbs` extension.

These two files have the same purpose wich is describing how the values in `content.toml` file will appaer in HTML page,
but they are used in different way, `template.hbs` together with `content.toml` used to construct a standalone HTML page,
while `partial.hbs` together with `content.toml` used to create an HTML chunk that can be embedded into other pages
in thier `template.hbs` file, and this could be done by partial feature in handlebars.

Glittery uses [handlebars-rust](https://github.com/sunng87/handlebars-rust) library, although it has some
[limitation](https://github.com/sunng87/handlebars-rust#limitations) but this may change in the future.

##### Helper Functions

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

### info.toml File
TODO


