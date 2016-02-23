```yml
title: Assimilator - Post Metadata
info: Short post explaining how to use metadata in Assimilator markdown post
tags: [assimilator, blogging, nodejs, markdown]  
draft: true
```

# Assimilator Post metadata

## Example usage

You can use markdown flavored code backtics <code>&#x0060;&#x0060;&#x0060;yml</code>

```yml
```yml
title: Assimilator - Post Metadata
info: Short post explaining how to use metadata in Assimilator markdown post
tags: [assimilator, blogging, nodejs, markdown]  
draft: true
layout: default
``````

Or if you want to hide metadata in your markdown use

```yml
<!---yml
title: Assimilator - Post Metadata
info: Short post explaining how to use metadata in Assimilator markdown post
tags: [assimilator, blogging, nodejs, markdown]  
draft: true
layout: default
-->
```

## Fields

* `title` - optional post title. If it is missing it will be auto generated from first highest `<h>` (html heading) tag.

* `info` - optional but useful for more post information on listing pages. `info` tag allows multiline with yml mulitline `>` tag

```yml
info:>
    Short post explaining how to use metadata in Assimilator markdown post.
    It contains some useful examples.

```

* `tags` - yml array of strings

Those can be either defined as elements of an array.

```yml
tags: [assimilator, blogging, nodejs, markdown]  
```

Or if you prefer as list:

```yml
tags:
  - assimilator
  - blogging
  - nodejs
  - markdown
```

* `draft` - optional and `false` by default. Set it to `true` to prevent post from being published.

```yml
draft: true
```

* `layout` - optional and `default` by default. Layout field allows to use custom page layout for particular post. If layout files are not found it will automatically fallback to `default`.

```yml
draft: my-super-layout
```

* `created` - optional and auto generated
* `updated`  - optional and auto generated
