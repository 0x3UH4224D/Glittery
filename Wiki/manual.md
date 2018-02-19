# Introduction
This manual will explain Glittery Services in details, if this your first time with Glittery have look at
[Intro To Glittery](./Intro-To-Glittery) since it provides complete examples and HowTo things. This manual will go in
more details with a few short examples so you need to have an idea of what's going on with Glittery to understand this
manual.

This manual will talk about `glittery` the command-line and these files that Glittery can understand in details.

`glittery` command-line can be used this way:
`glittery <service> <operation> [OPTION]...`

One exception is that help message can be shown if there wasn't any arguments:
`$ glittery`
Or:
`$ glittery --help`

There are two kind of help message, the main one we mentioned:
`$ glittery --help`
And the other one is for specific `<service>`, and we could get it by:
`$ glittery <service> --help`

## Blog Service
Users can manage and communicate with blog service using `glittery` command-line interface, you can get help message by
running `glittery --blog --help`, all arguments that belong to blog service should come after `--blog`.

Here is a table that contains all `<operation>`s for the Blog Service:

| Operation       | Arguments | Action                                            | Purpose                                                                                                                                    |
|-----------------|-----------|---------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| `--build`       |           | Construct HTML files for modified pages and posts | This will create HTML files for new pages and posts or if there was any change to them, although `glittery` will create them automatically |
| `--build-all`   |           | Construct HTML files for all pages and posts      | This will recreate HTML files for all pages and posts, even if they wasn't modified                                                        |
| `--new-page`    | [PAGE_ID] | Create a new page                                 | This will create a folder in `pages-path` that contains all necessary files to start writing a new page                                    |
| `--remove-page` | [PAGE_ID] | Remove page                                       | This will remove a page and all its related files                                                                                          |
| `--new-post`    | [POST_ID] | Create a new post                                 | This will create a folder in `posts-path` that contains all necessary files to start writing a new post                                    |
| `--remove-post` | [POST_ID] | Remove post                                       | This will remove a post and all its related files                                                                                                                                           |

### Pages
Glittery construct HTML files for pages that have folder in `pages-path`, each page's folder must contain three files

``` sh
pages-path
└── about-me                  # The page's folder, it's name is the 'id' for this page
    ├── partial.hbs           # handlebars file that can be used as partial.
    ├── page.hbs
    └── content.toml
```
TODO

### content.toml File
TODO

### page.hbs & include.hbs Files
TODO

### Page's Resources
TODO

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


