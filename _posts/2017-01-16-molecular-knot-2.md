---
layout: post
title: Animating a Molecular Knot
description: "How to make a molecular knot movie using PyMOL"
modified: 2017-01-16
tags: [PyMOL, video, movie, molecule]
image:
  feature: molecular-knot-2-1136x640.jpg
  credit: D.G. Oppenheimer
comments: true
excerpt: I read an interesting article in Science about a molecular knot. I decided to download the structure file and play with it in PyMOL. Here's what I did with it.  
---


<iframe src="https://player.vimeo.com/video/199729999?loop=1&portrait=0" width="640" height="640" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen> </iframe>
<p><a href="https://vimeo.com/199729999">molecular knot</a> from <a href="https://vimeo.com/user30910406">David Oppenheimer</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

I read an interesting article about the design and synthesis of a molecular knot in *Science* by Danon *et al.*[^footnote], and I thought I would try to play around with the molecule in the molecular structure manipulation program [PyMol](https://www.pymol.org). Here's how I made the movie shown at the beginning of this post.

- **Get the structure file.** The paper mentioned that they deposited the X-ray crystallography structure data in the public [Cambridge Structural Database (CSD)](https://www.ccdc.cam.ac.uk) under the Refcode entry, ODEQIA (CCDC Number, 1497422). I downloaded the structure file, which was in `.cif` format. 

- **Convert the file to `.pdb` format.** The [Mercury](https://www.ccdc.cam.ac.uk/solutions/csd-system/components/mercury/) software package can convert `.cif` files to `.pdb` format. [Download Mercury](https://www.ccdc.cam.ac.uk/support-and-resources/Downloads/), install it and open the `.cif` file. Go to `File -> Save As...` and choose `PDB files` for `Files of type:`. Give your file a name and click `Save`.

- **Open the `.pdb` file in PyMOL.** Once you open the file in PyMOL, you have myriad possibilities for viewing and animating the molecule. 

- **Format the molecule.** I like to use a ball and stick format for small molecules so that I can see all the atoms. I have previously found a ball and stick color scheme that I like on the [PyMOLwiki](https://pymolwiki.org/index.php/Main_Page) (one of the random images in the upper left). To format the molecule and make the movie shown at the beginning of this post, you can use the PyMOL script below. See the comments in the script for details. 

- **Write the movie files.** Either uncomment the `mpng` command before you run the scrip, or type `mpng name-of-file-prefix` to write your movie frames to a series of `.png` files.

- **Make the movie using `ffmpeg`.** If you purchase a license for incentive PyMOL, you can save a movie from within PyMOL proper. Since I am using open-source PyMOL, I use the free program, [ffmpeg](http://www.ffmpeg.org) to make my movies from the series of `.png` files produced by PyMOL. The settings I used are included in the script below. 



## Molecular Knot Movie

{% gist 2fb31b82c3a1ab75f1c50d9384f10940 %}


[^footnote]: Danon, J.J., A. Krüger, D.A. Leigh, J.F. Lemonnier, A.J. Stephens, I.J. Vitorica-Yrezabal and S.L. Woltering. 2017. Braiding a molecular knot with eight crossings. *Science* 355: 159-162. [link to article](http://science.sciencemag.org/content/355/6321/159.full)

