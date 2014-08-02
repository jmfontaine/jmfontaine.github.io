# Jean-Marc Fontaine's blog sources

All the contents of the blog, except for comments, and the title picture copyright 2003-2014 Jean-Marc Fontaine, distributed under [Creative Commons License](https://creativecommons.org/licenses/by-nc-nd/4.0/).

This blog is powered by [Jekyll](http://jekyllrb.com/) and [GitHub Pages](https://pages.github.com/).

The theme is [Incorporated](http://jekyllthemes.org/themes/incorporated/) by Kippt Inc.

## Installation & Usage

*Note: Jekyll requires Ruby version 1.9.3 or 2.0.0.*

Install Bundler:

``` 
gem install bundler
``` 

Install all dependencies using Bundler:
   
``` 
bundle install --path vendor/bundle
``` 
    
On OS X Mavericks it might be needed to run Bundler this way:

``` 
ARCHFLAGS=-Wno-error=unused-command-line-argument-hard-error-in-future bundle install --path vendor/bundle
``` 
    
Run Jekyll locally:
   
``` 
bundle exec jekyll serve --watch
``` 

Run Jekyll locally and display drafts:
   
``` 
bundle exec jekyll serve --watch --drafts
``` 
    
## Publish to Github Pages

``` 
rake site:publish
```
