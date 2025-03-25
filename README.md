# Rosey Jekyll Starter
Demonstrates a way to use Rosey to control multilingual content on your Jekyll site on CloudCannon.

Generates translation files that are editable on CloudCannon. The files needed by Rosey to create a multilingual site are generated  from those translations. Then on your production site Rosey generates a full multilingual site.

This is a demo site for how to use the Rosey CloudCannon Connector in Jekyll. The Rosey CloudCannon Connector has had the `rosey-connector` directory site mounted onto this site in order for it to run. This allows us to maintain one repository copy of `rosey-connector` for all SSGs, rather than maintaining them separately.

## Getting started
Follow the guide [here](https://github.com/CloudCannon/rcc?tab=readme-ov-file#rosey-cloudcannon-connector). 

## Jekyll specific implementation

### Generating ids

When tagging content for translation, the slugified contents of that translation should be used as the `data-rosey` id.

An example in Jekyll:

```liquid
<h1 class="{{c}}__title" data-rosey="{{ include.title | slugify }}">{{ include.title }}</h1>
```

The built in `slugify` filter makes it easy to slugify the text contents for use as the `data-rosey` tag. Templating with the `markdownify` filter does not need tagged like this, as it will automatically be tagged with plugins.

### Markdown processing

Create a prebuild in your `.cloudcannon` folder.

``` bash
#!/usr/bin/env bash

echo "Moving jekyllMarkdownPlugin.rb to _plugins"
mv rosey-connector/ssgs/jekyllMarkdownPlugin.rb site/_plugins/jekyllMarkdownPlugin.rb
echo "Moved jekyllMarkdownPlugin.rb to _plugins!"
echo "Moving jekyllImagePlugin.rb to _plugins"
mv rosey-connector/ssgs/jekyllImagePlugin.rb site/_plugins/jekyllImagePlugin.rb
echo "Moved jekyllImagePlugin.rb to _plugins!"
```

This prebuild moves two plugins two our sites `_plugins` folder. Both plugins customise the markdown processing of Jekyll; by extending how Jekyll uses Kramdown to parse the markdown. This affects page body content, and templating with the `markdownify` filter. This means neither body content, nor templating with the `markdownify` filter need to be tagged manually.

`jekyllMarkdownPlugin.rb` tags all block level elements with `data-rosey` tags. It uses the slugified text contents of the element for the value.

`jekyllImagePlugin.rb` removes the wrapping paragraph element from an image. This is important so that we don't have image links mistakenly appear in our translations.

### Build directory
Change your postbuild to use `_site` as it's source.

```bash
#!/usr/bin/env bash
npx @bookshop/generate

if [[ $ROSEYPROD != "true" ]];
then
  npx rosey generate --source _site
  node rosey-connector/roseyCloudCannonConnector.js
fi

if [[ $ROSEYPROD == "true" ]];
then
  echo "Translating site with Rosey"
  # By default, Rosey will place the default language under a language code, e.g. /en/index.html, and will generate a redirect file at /index.html.
  # By setting the flag --default-language-at-root, Rosey will output the default language at the root path, e.g. /index.html.
  # By setting the flag --default-language-at-root, Rosey will not generate any redirect pages.

  # We only want this to run on our production site, as it can interfere with CloudCannon CMS's visual editor
  # There's a little bit of shuffling around here to ensure the translated site ends up where CloudCannon picks up your site
  mv ./_site ./untranslated_site                  
  npx rosey build --source untranslated_site --dest _site 
fi
```

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