# Rosey Jekyll Starter
Demonstrates a way to use Rosey to control multilingual content on your Jekyll site on CloudCannon.

Generates translation files that are editable on CloudCannon. The files needed by Rosey to create a multilingual site are generated  from those translations. Then on your production site Rosey generates a full multilingual site.

This is a demo site for how to use the Rosey CloudCannon Connector in Jekyll. The Rosey CloudCannon Connector has had the `rosey-connector` directory site mounted onto this site in order for it to run. This allows us to maintain one repository copy of `rosey-connector` for all SSGs, rather than maintaining them separately.

## Getting started
Follow the guide [here](https://github.com/CloudCannon/rcc?tab=readme-ov-file#rosey-cloudcannon-connector). 

## Plugins in Jekyll
You can see in this site's prebuild that we move two plugins two our sites `_plugins` folder. Both customise the markdown processing of Jekyll, by adding onto how Kramdown parses the markdown. This affects page body content, and templating ran through the `markdownify` filter. This means neither body content, nor templating with the `markdownify` filter need to be tagged manually.

`jekyllMarkdownPlugin.rb` tags all block level elements with `data-rosey` tags. It uses the slugified text contents of the element for the value.

`jekyllImagePlugin.rb` removes the wrapping paragraph element from an image. This is important so that we don't have image links mistakenly appear in our translations.

## Local Development
Install the dependencies for Bookshop:

~~~bash
$ npm install
~~~

Install the Jekyll dependencies with [Bundler](http://bundler.io/):

~~~bash
$ npm run jekyll:install
~~~

Run the website:

~~~bash
$ npm start
~~~


> [!IMPORTANT]
> When running locally, and on staging, translations will not work. Rosey runs on your production site so translations will appear there. 


> [!IMPORTANT]
> When running locally, the pagination will not work. Deploy to CloudCannon to see successful pagination. 