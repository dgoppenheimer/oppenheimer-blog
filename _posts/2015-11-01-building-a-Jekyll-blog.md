---
layout: post
title: Building a Jekyll Blog
tags: [Jekyll, code, HPSTR]
description: "Building a blog using Jekyll and hosting it on GitHub-Pages"
image:
  feature: jekyll-banner-image-1.jpg
  credit: Tom Preston-Werner
  creditlink: http://jekyllrb.com
modified: 2015-11-08
comments: true
---

My previous attempt at setting up a Jekyll blog was successful, but I did not like the theme I was using. This time I will try the [HPSTR theme](http://jekyllthemes.org/themes/hpstr/) by [Michael Rose](https://mademistakes.com). I have Jekyll installed already, but I think bundler will install it again. I have a GitHub account. I'll delete the previous repositories and start fresh. I am doing this on a MacBook Pro running Mac OSX 10.10.5.

## Install Bundler ##

{% highlight console %}
$ cd ~
$ sudo gem install bundler
$ cd ~/Sites/oppenheimer-blog
$ bundle install # I got errors
{% endhighlight %}

Create a file called `Gemfile` in the new repository.

{% highlight console %}
$ cd oppenheimer-blog
$ nano Gemfile
{% endhighlight %}

Add the lines: 

```
gem 'github-pages'
source 'https://rubygems.org'
```

Save and exit.

Finish installation. _Note_: I had some errors installing nokogiri. After Googling around and trying a few things, I came upon [this solution on Stackoverflow](http://stackoverflow.com/questions/24091869/installing-nokogiri-on-osx-10-10-yosemite).

{% highlight console %}
$ sudo port selfupdate
$ sudo port upgrade outdated # this took a long time
$ sudo port install libiconv libxslt libxml2
$ sudo gem install nokogiri -- --use-system-libraries --with-xml2-include=/usr/include/libxml2 --with-xml2-lib=/usr/lib/
$ cd ~/Sites/oppenheimer-blog
$ bundle install # Success!
{% endhighlight %}

## Install Octopress ##

{% highlight console %}
$ sudo gem install octopress --pre
{% endhighlight %}

Octopress 3 is due out soon, so I may wait before I dive into learning Octopress.

## Fork a Theme ##

I've decided to try the [HPSTR theme](http://jekyllthemes.org/themes/hpstr/) from [Michael Rose](https://mademistakes.com/work/jekyll-themes/).

I [forked the theme](https://github.com/mmistakes/hpstr-jekyll-theme/fork) to my Git repository. I have previously set up ssh keys for authentication to GitHub.

Clone the theme to my local repository.

{% highlight console %}
$ cd ~/Sites
$ git clone git@github.com:dgoppenheimer/hpstr-jekyll-theme.git
$ cd hpstr-jekyll-theme
{% endhighlight %}

Try the `bundle install` again in the `hpstr-jekyll-theme` directory.

{% highlight console %}
Bundle complete! 4 Gemfile dependencies, 38 gems now installed.
Use bundle show [gemname] to see where a bundled gem is installed.
{% endhighlight %}

Rename the repository on GitHub. Go into the repo, and click `Settings`. Change the repo name to `oppenheimer-blog` and click `Rename`. Also update the existing local clone.

{% highlight console %}
$ git remote set-url origin git@github.com:dgoppenheimer/oppenheimer-blog.git
{% endhighlight %}

Rename the local directory.

```console
$ cd ../ # move out of the hpstr-jekyll-theme directory
$ mv hpstr-jekyll-theme oppenheimer-blog
$ cd oppenheimer-blog
```

Give it a whirl. _Note_: I changed a file in the images directory.

```console
$ git add .
$ git commit -m "added my profile photo; changed avatar.jpg to avatar-orig.jpg"
$ git push
$ git checkout gh-pages
$ git merge --no-ff master 
$ # there were several conflicts
$ git log
$ git reset --hard 16638192f5
$ git checkout master
$ # delete local and remote gh-pages branches
$ git branch -d gh-pages
$ git push origin :gh-pages
$ git checkout -b gh-pages # create new, fresh gh-pages branch
$ git push origin gh-pages
```

Crank up the local server.

```console
$ # in the oppenheimer-blog directory, on the gh-pages branch
$ bundle exec jekyll serve
```

Check `http://localhost:4000` for the site. **Success!**

## Testing the Site on GitHub Pages ##

The site should be visible at [http://dgoppenheimer.github.io/oppenheimer-blog/](http://dgoppenheimer.github.io/oppenheimer-blog/). However, it is lacking the css.

Start by following the tips on [the Jekyll site](http://jekyllrb.com/docs/github-pages/). Start by looking at the `base url` option in `_config.yml`.  While I'm at it, I'll change a few of the other options like Site name, etc.

In the `oppenheimer-blog` directory:

```console
$ git status
$ git commit -am "changed _config.yml"
$ git push origin gh-pages
$ git checkout master
$ git merge gh-pages
$ git push origin master
$ bundle exec jekyll serve --baseurl '' # to test locally
```

So the changes I made to `_config.yml` did not completely destroy the site. I can access it locally, and all the links work. Now I'll see if I can get it to work on GitHub Pages.

Workflow:

Make sure you are on the gh-pages branch.

```console
$ git branch
gh-pages
master
$ # make changes to _config.yml
$ git commit -am "made changes to _config.yml"
$ git push origin gh-pages
```

Check site at http://dgoppenheimer.github.io/oppenheimer-blog/

I finally got it to work. It helped to view page source in Safari and look at the path that was being used. Then I could go back to `_config.yml` and adjust the `url` option. I eventually used `url: http://dgoppenheimer.github.io/oppenheimer-blog` and `baseurl: /oppenheimer-blog`.

When developing locally, change the `url` option to `http://localhost:4000` and use `bundle exec jekyll serve --baseurl ''`.

## Customizing the Site ##

### Header ###

Add a bio to the `_config.yml` file.

### Images ###

Crop images to 1024 x 256 pixels

## Deleting the Master Branch ##

Since I was planning to keep the gh-pages and master branches in sync anyway, I thought I should just delete the master branch to keep things easier. I got these instructions from [Oli's blog](http://oli.jp/2011/github-pages-workflow/).

Make sure that you are in the `oppenheimer-blog` directory and on the `gh-pages` branch.

```console
$ git status
On branch gh-pages
nothing to commit, working directory clean
```

Go to GitHub and change the default branch to `gh-pages` for the `oppenheimer-blog` repository. Click on the repository name, then _Settings_ > _Branches_. Change the default to the `gh-pages` branch and click _Update_.

Delete the `master` branch on the remote (GitHub) and locally.

```console
$ cd ~/Sites/oppenheimer-blog
$ git push origin :master
$ git branch -d master
```

## Enabling Disqus Comments

Follow the instructions from the accepted answer to this question on [Stackoverflow](http://stackoverflow.com/questions/21446165/how-do-i-use-disqus-comments-in-github-pages-blog-markdown). The instructions from Disqus are [here](https://help.disqus.com/customer/portal/articles/472138-jekyll-installation-instructions).

Briefly:

Register on [Disqus](https://disqus.com). 

Add `comments: true` to the [YAML](http://jekyllrb.com/docs/frontmatter/) front matter.

Modify the `disqus_comments.html` file in the `_includes` directory. Replace the example `site.disqus_shortname` with the disqus shortname you received when you registered your site. I also added this line so I could test the site locally:

```js
// var disqus_developer = 1; // Comment out before you push to gh-pages
```

I added these lines as well.

{% highlight js %}
var disqus_identifier = "{{ site.disqus_shortname }}{{ page.url | replace:'index.html','' }}";
var disqus_url = '{{ site.url }}{{ page.url }}';
var disqus_title = '\{\{ page.title | slugify \}\}';
{% endhighlight %}
 

## Workflow

Write a post using [Markdown](https://daringfireball.net/projects/markdown/). Name it according to [Jekyll](http://jekyllrb.com/docs/posts/) conventions and put it in the `_posts` directory. Then commit it locally and push it to GitHub.

```console
$ cd ~/Sites/oppenheimer-blog
$ git status
$ git add .
$ git commit -m "a message about the post"
$ git push origin gh-pages # check site at http://dgoppenheimer.github.io/oppenheimer-blog/
```

**—and Bob's your uncle!**

