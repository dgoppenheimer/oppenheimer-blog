---
layout: post
title: Using the Solarized themes in Textwrangler 5
tags: [Textwrangler, code, highlighting, Solarized]
image:
  feature: textwrangler-solarized.jpg
  credit: Textwrangler and Solarized
comments: true
---
I have been using [Textwrangler](http://www.barebones.com/products/textwrangler/) for years as my go-to text editor. I also used [Atom](https://atom.io) for a while and learned that I appreciate the finer things like syntax highlighting and the lower contrast themes that help reduce eyestrain. There are certain features of Textwrangler that I like, so I decided to stick with it as my primary text editor, but customize it a bit by installing one of the low contrast themes.

[Solarized](http://ethanschoonover.com/solarized) is a carefully chosen color scheme designed to reduce eyestrain when editing text. It has been ported to most of the text editors that are popular among coders, including Textwrangler. I recently updated to Textwrangler 5.0, and the Solarized files I downloaded from [GitHub](https://github.com/rcarmo/textwrangler-bbedit-solarized) would not work. However, [other theme files](https://github.com/billkeller/BBEdit-Color-Schemes-Pack) worked fine. I compared the files that worked with the ones that did not, and eventually got the Solarized themes to work. I thought I would post my solution here in case anybody else had this problem. *Disclaimer:* I am not a coder, just a dabbler. I'm sure anyone with a little experience would have identified the problem and fixed it much faster and easier than I did.

Here is how I got the Solarized themes working in Textwrangler 5.0: 

Open Terminal (MacOSX)
Download the [BBEdit Color Schemes Pack](https://github.com/billkeller/BBEdit-Color-Schemes-Pack) from GitHub, and copy the `.bbcolors` files to the directory:

{% highlight console %}
~/Library/Application Support/TextWrangler/Color Schemes
{% endhighlight %}

        
{% highlight console %}
$ git clone https://github.com/billkeller/BBEdit-Color-Schemes-Pack.git
$ cd BBEdit-Color-Schemes-Pack
$ cp *.bbcolors ~/Library/Application\ Support/TextWrangler/Color\ Schemes/
{% endhighlight %}
        
Open `Solarized Dark.bbcolors`, and `Tomorrow.bbcolors` in Textwrangler. 

Make a copy of the `Tomorrow.bbcolors` file and name it `Tomorrow-copy.bbcolors`
In Textwrangler, select (cmd-click) both the `Solarized Dark.bbcolors` and the `Tomorrow-copy.bbcolors files`.

Right-click (ctl-click) one of the selected files and select `Compare` from the menu. 

{% highlight console %}
$ git clone https://github.com/billkeller/BBEdit-Color-Schemes-Pack.git
$ cd BBEdit-Color-Schemes-Pack
$ cp *.bbcolors ~/Library/Application\ Support/TextWrangler/Color\ Schemes/
{% endhighlight %}
        
For each of `<key>` values in the `Tomorrow-copy.bbcolors` file, replace the `<string>` values with the corresponding `<string>` values from the `Solarized Dark.bbcolors` file.         
Some of the `<key>` values in the `Solarized Dark.bbcolors` file are different than the values in the `Tomorrow-copy.bbcolors` file, and may be the source of the problem. Do not copy those `<key>` values to the `Tomorrow-copy.bbcolors` file. For example, in the `Solarized Dark.bbcolors` file we find:
    
{% highlight html %}
<key>Color:UseCustomHighlight</key>
<string>1</string>
{% endhighlight %}
	
But in the `Tomorrow-copy.bbcolors` file we find:
	
{% highlight html %}
<key>UseCustomHighlightColor</key>
<false/>
{% endhighlight %}
    
Instead, use `<key>UseCustomHighlightColor</key>` but change `<false/>` to `<true/>`.
    
Delete the `Solarized Dark.bbcolors` and the `Solarized Dark.bbColorScheme` files from the `Color Schemes` directory. 

Rename the `Tomorrow-copy.bbcolors` file to `Solarized Dark.bbcolors`.

Restart Textwrangler.

Choose the new Solarized Dark theme from the Text Color preferences. 

Repeat the above steps for the Solarized Light theme.

Here are Gists of the Solarized Light and Solarized Dark themes for Textwrangler 5. For Mac OSX 10.10 put these files in 

{% highlight html %}
~/Library/Application\ Support/TextWrangler/Color\ Schemes
{% endhighlight %}

and restart Textwrangler.

## Solarized Light.bbcolors

{% gist 056c9bcbe2eeffd626ad %}

## Solarized Dark.bbcolors

{% gist d58dbc7bd9170f1b1a5d %}

### Happy coding!








 