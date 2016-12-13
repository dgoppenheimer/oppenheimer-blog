---
layout: post
title: Another Protein Conformation Change Movie
description: "How to show a protein changing conformations using PyMOL and the Morphing Server"
modified:
tags: [PyMOL, Morphing Server, video, protein dynamics]
image:
  feature: CaM-1136x640.jpg
  credit: D.G. Oppenheimer
comments: true
excerpt: Since making my first protein conformational change movie, I've been on the lookout for other proteins that undergo large conformational changes. A colleague told me that the crystal structures are available for Calcium-dependent Protein Kinase 1 (CDPKs)



about CDPK1 a kinase that changes shape dramatically, and I thought I should try to make a movie of it. I have been using the [Climber](https://simtk.org/home/climber) program, which can calculate the structural changes necessary to morph one protein conformation into another, and I combine the Climber output with some tweaking in [PyMOL](https://www.pymol.org) to make a sweet movie of the conformational change. Grab some popcorn and enjoy!

---

[Morph Server](http://www2.molmovdb.org/wiki/info/index.php/Morph_Server)
[Single chain server](http://molmovdb.org/cgi-bin/submit.cgi)



The submission is being processed. Your confirmation ID is:
645391-14458
Computation time varies from a few minutes to a day or so. Please wait a bit and then go to this URL to view your results or check on your job: 
http://molmovdb.org/cgi-bin/morph.cgi?ID=645391-14458 

```python
> cd ~/pymol-work/1-morphing/MorphServer/cdpk1-1/645391-14458
> for idx in range(0,9):cmd.load("ff%01d.pdb"%idx,"cdpk2mov")


Totally messed up bond angles and lengths.



---





---

See my previous [protein conformation change movie]({% post-url 2016-03-21-protein-conformation-change-movie.md %}) post on how to install the Climber program. 

This information was taken from Dr. Alice Harmon's [Protopedia](http://proteopedia.org/wiki/index.php/Main_Page) Wiki page on [Calcium-dependent protein kinases (CDPKs)](http://proteopedia.org/wiki/index.php/CDPK).

## Download the two PDB files into the `climber-work` directory ##

First I'll move into the `climber-work` directory, then I'll make a `CDPK1` directory into which I'll download the PDB files.  

```bash
$ cd ~/climber-work
$ mkdir CDPK-2
$ cd CDPK-2
$ curl -o 3KU2_orig.pdb https://files.rcsb.org/download/3KU2.pdb
$ curl -o 3HX4_orig.pdb https://files.rcsb.org/download/3HX4.pdb
```

Set the environment variable `CLIMBERDIR`.

```bash
$ export CLIMBERDIR=~/climber-work/Climber
```

Create an alignment.

```bash
$ awk '/ATOM/ {print}' < 3KU2_orig.pdb > 3KU2.pdb
$ awk '/ATOM/ {print}' < 3HX4_orig.pdb > 3HX4.pdb
$ ${CLIMBERDIR}/sh/rms.sh 3KU2 3HX4 > morphx.al1
```

Morph 50 steps. **Note:** This step can take a while depending on how many steps you chose. On my laptop with 50 steps, Climber created 217 `.pdb` files in 1 hour and 35 minutes.

```bash
$ ${CLIMBERDIR}/sh/morphx.sh 3KU2 3HX4 50
```

Second attempt
    114 files 9:09->9:48
A new directory `3KU2_3HX4_072step` was created. This directory contains all the `.pdb` files for the trajectory.

## Import the files into PyMOL ##

Start PyMOL.

```bash
$ cd ~
$ pymol
```

Load the trajectory files into PyMOL.

```python
> cd ~/climber-work/CDPK1/3KU2_3HX4_072step # path/to/your/pdb-files
> for idx in range(0,107):cmd.load("file%05d.pdb"%idx,"mov")
```


```python
> cd ~/climber-work/CDPK-2/3KU2_3HX4_050step
> for idx in range(0,114):cmd.load("file%05d.pdb"%idx,"cdpk2mov")
```
The ending conformation does not look like 3HX4. I don't think climber can handle this one. It is still better than the other alternatives

---

[MovieMaker web server](http://wishart.biology.ualberta.ca/moviemaker/)

Try MovieMaker

```python
> cd ~/pymol-work/1-morphing/MovieMaker/CDPK1-mm/pdb_files
> for idx in range(0,19):cmd.load("int_struct%01d.pdb"%idx,"cdpk2mov")
```

```python
> mclear
> mset 1 -19 -1
> set cache_frames=0
> set ray_trace_frames=0
> cd ~/pymol-work/1-pymol-movies/cdpk-mm1
> mpng CDPK1-conf-mov
``` 

The linear interpolation makes a mess out of the EF-hands.

---


The first command changes directories so that PyMOL can find the `pdb` files created by `climber`. The second command loads those files into PyMOL as a movie. The `range(0,48)` corresponds to the number of pdb files in the directory. The output files from `climber` are numbered from 0 to 47 (48 files), but PyMOL does not want to use 0 as the first state. Setting the range to be `range(0,48)` results in all 48 files being loaded. The `"file%05d.pdb"` tells PyMOL that the pdb files have a prefix of `file` followed by a 5-digit number, `%05d`, and the extension, `.pdb`. 

If you are successful, you should have a multi-state file loaded with the name of `mov`. 


## Make the protein look pretty ##

I will color the atoms using the pymol script that I created earlier.


```python
# @pretty-atoms1.txt

# Hide lines and show spheres
hide lines
show spheres

# Define some new colors. I'll use the pastel colors I used previously for the ER membrane.

# pastelyellow rgb 247, 220, 111
set_color pastelyellow= [0.9686, 0.8627, 0.4353]

# pastelblue rgb 133, 193, 233
set_color pastelblue= [0.5216, 0.7569, 0.9137]

# pastelpurple rgb 187, 143, 206
set_color pastelpurple= [0.7333, 0.5608, 0.8078]

# salmon2 rgb 241, 148, 138
set_color salmon2= [0.9451, 0.58, 0.541]

# lightgrey2 rgb 229, 232, 232
set_color lightgrey2= [0.898, 0.9098, 0.9098]

# pastelgreen rgb 125, 206, 160
set_color pastelgreen= [0.49, 0.808, 0.6275]

# pastelorange rgb 240, 178, 122
set_color pastelorange= [0.9412, 0.698, 0.4784]

# Adjust the viewport.

viewport 640, 640 # These settings are for Instagram

# Color the atoms.
color salmon2, elem o and mov
color lightgrey2, elem h and mov
color pastelblue, elem c and mov
color pastelpurple, elem n and mov
color pastelyellow, elem s and mov
  
# set lighting
set specular, 0.25
set spec_power, 300
set spec_reflect, 0.5
util.ray_shadows("medium")
set antialias, 2
set orthoscopic, off
set field_of_view, 30

# set the view
set_view (\
     0.064534962,    0.962881327,   -0.262094170,\
    -0.905501723,    0.166886628,    0.390148699,\
     0.419405788,    0.212148622,    0.882660985,\
    -0.000122949,    0.000002109, -180.884475708,\
    -7.947376251,   -1.599441528,   77.428321838,\
  -11735.089843750, 12096.833984375,  -30.000000000 )
  
# grey background gradient
set bg_gradient
set bg_rgb_top, grey90
set bg_rgb_bottom, grey45

```




## Make a movie in PyMOL that loops back and forth ##

```python
> mclear
> mset 1 -70 -1
> set cache_frames=0
> set ray_trace_frames=1
> cd ~/pymol-work/1-pymol-movies/CDPK1-conformation3
> mpng CDPK1-conf-mov
```    
    
The `mclear` command tells PyMOL to remove any saved frames from the cache. 

The `mset` command tells PyMOL which states get included as frames of a movie. In our case, frames 1, 2, 3, ..., 71, 72, then 71, 70, ..., 4, 3, 2. Stopping at state 2 allows smooth motion when we loop the resulting exported movie.

The `set cache_frames=0` command tells PyMOL to not save each frame in memory. If the frame cache is turned on, a large amount of memory is used saving the images in RAM.

The `mpng` command will write each frame of the movie to a separate file with the specified prefix, in this case, `CDPK1-conf-mov`. 

To test the movie, turn off ray tracing of frames: paste `set ray_trace_frames=0` into the PyMOL command line. When you are ready to create the frames of the movie, turn ray tracing back on: `set ray_trace_frames=1`.

**Note:** Since I don't pay for a PyMOL subscription, I have to compile the code myself. The open source version of PyMOL does not have the movie creating commands, so I have to create `png` files for each frame, and then put them together using other software.


## Make the movie from the .png files ##

I have previously installed the free command line video tool, [FFMPEG](http://www.ffmpeg.org). There is definitely a learning curve when using ffmpeg, but luckily, there are lots of tutorials on the web for your learning enjoyment. Here are the options I used to make the movie at the start of this post. I got the explanations from the [Joy of Data blog](http://www.joyofdata.de/blog/hd-clips-with-ffmpeg-for-youtube-and-vimeo/) and the ffmpeg manual. 

```bash
ffmpeg 
  -framerate 30     # input frame rate
  -i image%03d.png  # image names (image000.png, ..., image999.png)
  -s:v 640x640      # video size
  -c:v libx264      # encoder
  -profile:v main   # H.264 profile for video
  -crf 12           # constant rate factor (0 to 51, 0 is lossless)
  -pix_fmt yuv420p  # pixel format; changes the rgb from the pngs to yuv
  -preset veryslow  # encoding speed (slow but higher quality per file size)
  -tune stillimage  # tunes the encoding settings for images
  -r 30             # output frame rate
  output_file.mp4   # movie file name
```

Type the following on the command line. 

    $ cd ~/pymol-work/1-pymol-movies/CDPK1-conformation
    $ ffmpeg -framerate 30 -pattern_type glob -i '*.png' -s:v 640x640 -c:v libx264 -profile:v main -crf 12 -tune stillimage -pix_fmt yuv420p -preset veryslow -r 30 CDPK1-conf03.mp4

**Success!** We have a movie that loops nicely, and that I can post to Instagram. **Note:** To view the movie, I use VLC from the [VideoLAN website](http://www.videolan.org/vlc/download-macosx.html) as my movie player. 

## Optimize the video for Vimeo ##

```bash
ffmpeg 
  -framerate 30        # input frame rate
  -i image%03d.png     # image names (image000.png, ..., image999.png)
  -s:v 1280x720        # video size
  -c:v libx264         # encoder
  -crf 18              # constant rate factor (0 to 51, 0 is lossless)
  -pix_fmt yuv420p     # pixel format; changes the rgb from the pngs to yuv
  -preset veryslow     # encoding speed (slow but higher quality per file size)
  -tune stillimage     # tunes the encoding settings for images
  -r 30                # output frame rate
  output_file.mp4      # movie file name
```

To post on the Cell Bones website, I want to optimize the video for Vimeo. Mainly this means changing the image dimensions and moving the moov atom. I also used a higher antialias value in PyMOL so that the images look sharper. So, briefly, start PyMOL, import session, change viewport to 1280x720, color and reposition protein, turn on ray tracing, set antialias to 2, make the movie, and export frames to png files.

```bash
$ cd  ~/pymol-work/1-pymol-movies/CaM-conf-vimeo
$ ffmpeg -framerate 30 -pattern_type glob -i '*.png' -s:v 1280x720 -c:v libx264 -crf 18 -tune stillimage -pix_fmt yuv420p -preset veryslow -r 30 CDPK1-conf-vimeo1.mp4
```



[^publication]: Weiss D.R., and M. Levitt. 2009. Can morphing methods predict intermediate structures. J Mol Biol. 385: 665-674. [full text of publication](http://www.ncbi.nlm.nih.gov/pmc/articles/PMC2691871/)



---


---



```python
split_states name
delete name
disable my_object*
enable my_object1

run make_pov.py
make_pov 1ydu-01.pov
```

```bash
povray +Imov_0001.pov +Omov_0001.png +FN8 +W640 +H640 +Q9 +A0.3 +ua
```


---

## Making a movie the hard way ##

I want to try to get ambient occlusion using pov-ray to work. 

## Import the files into PyMOL ##

Start PyMOL.

```bash
$ cd ~
$ pymol
```

Load the trajectory files into PyMOL.

```python
> cd ~/climber-work/CDPK1/3KU2_3HX4_072step # path/to/your/pdb-files
> for idx in range(0,107):cmd.load("file%05d.pdb"%idx,"cdpk1mov") 
```

The first command changes directories so that PyMOL can find the `pdb` files created by `climber`. The second command loads those files into PyMOL as a movie. The `range(0,48)` corresponds to the number of pdb files in the directory. The output files from `climber` are numbered from 0 to 47 (48 files), but PyMOL does not want to use 0 as the first state. Setting the range to be `range(0,48)` results in all 48 files being loaded. The `"file%05d.pdb"` tells PyMOL that the pdb files have a prefix of `file` followed by a 5-digit number, `%05d`, and the extension, `.pdb`. 

If you are successful, you should have a multi-state file loaded with the name of `cdpk1mov`. 


```python
> cd ~/pymol-work/1-scripts
> @pretty-atoms1.txt
> cd CDPK1-ao
```



```python
> run make_pov.py
> make_pov cdpk001.pov # repeat for each state
```

povray +Icdpk001.pov +Ocdpk001.png +FN8 +W640 +H640 +Q9 +A0.1 +ua
povray +Icdpk005.pov +Ocdpk005.png +FN8 +W640 +H640 +Q9 +A0.1 +ua

iterate over the entire set of filenames.

Try the shell script

Try batch mode 

I cannot figure out how to batch create the `.pov` files, so I did it manually. 

You do not need to split states. Just use `make_pov mov001.pov` where mov001 is the first state of the movie. Just change the number for each state as you click through the movie. Once you have collected all the mov files, you can run the povray commands to collect each of the individual png files. 


### Convert the .pov files to .png files ###

The official command line POV-Ray utility that I use does not have a batch processor, but I found an [unofficial POV-Ray mac utility](http://megapov.inetart.net/povrayunofficial_mac/finalpov.html) with a graphical user interface that has a batch processor. 

* Download and unpack the software
* Set the image size and quality
* Choose the output directory (under the `Misc`tab)
* Open the `Batch` window
* Add the `.pov` files
* Click `Run All` 

After a while, all the files will be processed and the resulting `.png` files will be in the output directory. Because I did not want to overwhelm my old laptop, I processed only 20 files at a time. I don't think it would have been a problem to process all the files at once, though. 

Now I have a set of `.png` files that represent the frames of the forward conformational change of 3KU2 into 3HX4. I also want the conformational change of 3HX4 back to 3KU2. Since I had to manually process each state of the `cdpkmov` in `PyMOL`, I decided to only do this for the forward conformational change. I figured I can just duplicate the resulting `.png` files and rename them so that I have files representing both the forward and backward conformational changes. In other words, the forward conformational change  files are numbered 001 to 107. I want to duplicate these files and number them as follows: duplicate file 107.png will be named 108.png, duplicate file 106.png will be named 109.png, and so on. This way I can generate the `.png` files for the transition from the 3HX4 back to 3KU2. 

After a bit of web searching, I found [how to rename files consecutively](http://stackoverflow.com/questions/3211595/renaming-files-in-a-folder-to-sequential-numbers). The `ls` command will list the files in the directory, which will be renamed in the subsequent steps. I'll use the `-r` option to reverse sort the list of files. Since all the files in the directory will be renamed, I'll create a new directory that only includes the files I want to rename. 


```bash
ls -1 -c -r | awk 'BEGIN{ a=108 }{ printf "mv \"%s\" cdpk%03d.png\n", $0, a++ }' | bash
```

Success! I now have files numbered 108 to 214 that are for the change back from 3HX4 to 3KU2. 

### Add a color gradient background ###

The PyMOL script that writes the `.pov` files for POV-Ray results in a transparent background in the resulting `.png` files. I like using a gradient of color for the background to make the protein stand out. Luckily, the free (as in beer) command line utility, [ImageMagick](http://www.imagemagick.org/script/index.php), can batch process the files and put a gradient background in each of the `.png` images. 

#### Install ImageMagick ####

I use the package manager, [MacPorts](https://guide.macports.org), to install most of my command line utilities.

```bash
$ cd ~
$ sudo port selfupdate
$ sudo port upgrade outdated  # this can take a while
$ sudo port install ImageMagick
```

#### Add the gradient ####

For the gradient, I went to the [ImageMagick List of Color Names webpage](http://www.imagemagick.org/script/color.php) and chose the `LightSteelBlue2` color. I plugged in the `HEX` code for this color, `#BCD2EE` on the [Paletton](http://paletton.com) website and used the `very light pale pastel` preset. I  chose a light color for the top and a darker color for the bottom of the gradient. 

I took these commands from the `Batch replacing color with transparency` topic on the [StackExchange GRAPHIC DESIGN forum](http://graphicdesign.stackexchange.com/questions/16120/batch-replacing-color-with-transparency), and the `replace a solid colour with a gradient` topic on the [ImageMagick forums](http://www.imagemagick.org/discourse-server/viewtopic.php?t=21359).

Put all the `.png` files in a directory by themselves. Add a subdirectory named `batch`. Run the command from within the directory containing the `.png` files. 

```bash
$ for f in *.png; do convert -size 640x640 gradient:'rgb(195, 207, 222)'-'rgb(63, 95, 136)' "$f" -composite batch/"${f%%.png}.png"; done
```
Success! The `batch` directory is filled with image files that all have a gradient background. 

### Make the movie ###

I will use the command line utility [FFMPEG](http://www.ffmpeg.org) to generate the movie from the `.png` files. I took these settings from the [Joy of Data blog](http://www.joyofdata.de/blog/hd-clips-with-ffmpeg-for-youtube-and-vimeo/).  

```bash
$ ffmpeg -framerate 30 -pattern_type glob -i '*.png' -s:v 640x640 -c:v libx264 -profile:v main -crf 12 -tune stillimage -pix_fmt yuv420p -preset veryslow -r 30 CDPK1-conf01.mp4
```

Success! I have a nice movie that I can post to Instagram, which uses a square video format. I can re-position the protein and remake the movie for formats of different dimensions (like 4:3) if I want to go that route. 

### Final Notes ###

The Climber program created more files than I really needed. When they were loaded into PyMOL, the resulting movie had a long pause in the middle. I found that I could easily remove states from PyMOL to shorten the pause, or remove some `.png` files before the final movie was created with FFMPEG. It is better to do a trial run of the movie in PyMOL, because it is better to not have to manually create more `.pov` files than are needed. 




---

System:
Mac OSX 10.10.5
Climber: latest

I used the CDPK1 (with and without Ca2+) structures for morphing. Climber could not reach the final conformation, i.e. the ending state from climber does not look like one of the protein structures that I loaded. Here is what I did:

$ cd ~/climber-work
$ mkdir CDPK-2
$ cd CDPK-2
$ curl -o 3KU2_orig.pdb https://files.rcsb.org/download/3KU2.pdb
$ curl -o 3HX4_orig.pdb https://files.rcsb.org/download/3HX4.pdb
$ export CLIMBERDIR=~/climber-work/Climber
$ awk '/ATOM/ {print}' < 3KU2_orig.pdb > 3KU2.pdb
$ awk '/ATOM/ {print}' < 3HX4_orig.pdb > 3HX4.pdb
$ ${CLIMBERDIR}/sh/rms.sh 3KU2 3HX4 > morphx.al1
$ ${CLIMBERDIR}/sh/morphx.sh 3KU2 3HX4 50

Climber created 114 files.
I imported them into PyMOL and the last structure does not look at all like 3HX4.






