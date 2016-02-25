---
layout: post
title: Small Molecule Movies in PyMol
description: "How to make simple movies of small molecules using PyMol and FFmpeg"
modified: 
tags: [PyMol, code, molecules, ffmpeg, mp4]
image:
  feature: test.jpg
  credit: D. G. Oppenheimer
comments: false
excerpt: Molecules are inherently three dimensional and 2D representations do not often adequately show the arrangement of atoms, especially for "larger" small molecules. To better show the structure, I made a simple movie where the small molecule rotates around the Y-axis. Here's how I did it. 
---

I often show molecular structures when I teach the biochemistry section of Integrated Principles of Biology, and the typical 2D representations often obscure some of the key 3D features. With the help of [Pymol](https://www.pymol.org), [ChemSpider](http://www.chemspider.com), and [FFmpeg](https://ffmpeg.org), I can easily make video of molecular structures where the molecule rotates. This helps demonstrate the inherent 3D properties of the molecules and looks good, too. Here is a quick tutorial. 

- First, choose an interesting small molecule. For this example, I chose Oxytocin, also known as the "love hormone". 
- Open your browser and go to [ChemSpider](http://www.chemspider.com). Search for Oxytocin.
- Click the `3D` button under the image of the structure of Oxytocin. 
- Click the `save to disk` button to save the 3D structure file to your disk. I renamed the `388434.mol` file to `Oxytocin-388434.mol`.
- Start PyMol. Open the 3D structure file (with the `.mol` extension) in PyMol. 

At this point, you can manipulate the structure file as you would any other structure in PyMol. Below I have included a PyMol script that will apply nice styling to the molecule, but you can style the molecule however you like. This script also contains the movie commands, so comment out the movie lines if you just want to play with the molecule styling or you will have to wait while PyMol tries to ray trace every frame.

PyMol has several utilities to make simple movies, and the script below uses the `util.mroll` utility. If you have a PyMol subscription, you can save the movie directly as an mpeg, but I compiled my own version of PyMol so that function is missing. Nonetheless Pymol will save each frame of the movie as a `.png` file and I can create a movie from the files using the free program, [FFmpeg](https://ffmpeg.org). There is a learning curve with FFmpeg, but there are lots of resources available. For some FFmpeg encoding tips, see the post by [Jess Hyden on Octoshape Support](https://support.octoshape.com/entries/25126002-Encoding-best-practices-using-ffmpeg) and the [FFmpeg wiki](https://trac.ffmpeg.org/wiki/Encode/H.264). In the PyMol script, below, I have included the FFmpeg settings that I used to generate the movie in this post. 

<iframe width="420" height="315" src="https://www.youtube.com/embed/KZAlSPZx7Io" frameborder="0"> </iframe>


## Small molecule rotation movie

{% gist 10485119b49731dc4833 %}

