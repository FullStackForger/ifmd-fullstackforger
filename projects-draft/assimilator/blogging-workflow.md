```yml
title: Assimilator - Workflow
info: This post illustrates how to work with Assimilator engine
tags: [assimilator, blogging, nodejs, markdown, workflow, webhooks, git]  
```

# Assimilator - Workflow

## Writing

Create a category folder if you don't have one. Then create a markdown `.md` file and start writing.

## Metadata

You can optionally add metadata to your post. Some meta fields will be overwritten upon post publishing.

For draft posts use meta data tag `draft: true`. It will prevent post from publishing. Simply remove the line when you done and republish.

## Publishing

Assimilator is build based upon storing your blog content in git. Therefore every time you done writing make sure to (1) commit and then (2) push all the changes to remote repository to auto - publish.

### Manual publishing

Manual publishing means you have to ssh into your server and manually pull all the changes. That's the default method and is quite useful if you want to control over when you update your live content. Manual publishing, however, might be a bit cumbersome if you publishing a lot and often or simply don't want to be bothered to do so.

### Git webhooks

[Git webhooks](https://developer.github.com/webhooks/) are very nice way to handle auto publishing. Once set up your site will update automatically every time there is new content.
