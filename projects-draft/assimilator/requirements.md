```yml
title: Assimilator requirements
info: Assimilator requirements document
tags: [assimilator, blogging, nodejs, markdown, webhooks, git, docs]  
```

# Assimilator requirements

## Core requirements

- serving static content
- templating (handlebars)
- layouts
- themes (config.json)
  - prioritize local theme over node_modules
- parsing md
- post metadata
- caching
  - generating category links
  - generating tag links
- git webhooks
- plugins
  - authentication
  - statistics

## Admin

 - admin panel made in react.js
 - custom extensions

## Structure
```
# required files
.gitignore
package.json
config.json <- can be included in package.json as `assimilator` object

# auto generated
.cache/categories/category_1.html   <- category based toc
.cache/tags/tag_1.html    <- tag based toc
.cache/tags/tag_2.html    <- tag based toc
.cache/tags/tag_3.html    <- tag based toc
.cache/blog/category_1/index.html             <- list of posts
.cache/blog/category_1/some-article.html      <- static post page
.cache/blog/category_1/another-article.html   <- static post page

blog/category_1/
blog/category_1/some-article.md
blog/category_2/another-article.md

pages/
pages/index/index.html
pages/index/index.css
pages/index/script.js
pages/some_static_page/
pages/some_static_page/index.html
pages/some_static_page/style.css
pages/some_static_page/img/
pages/some_static_page/img/image.jpg

themes/
themes/theme_1/
themes/theme_1/index.hl s-tml
themes/theme_1/index.css
themes/theme_1/script.js
```
