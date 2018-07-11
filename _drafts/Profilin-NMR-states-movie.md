tags: [PyMOL, video, movie, protein, NMR]
image:
  feature: profilin098-1136x640.jpg
  credit: D.G. Oppenheimer
comments: true
excerpt: This year I submitted a video of the protein, profilin, morphing between NMR states to the [FASEB BioArt Competition](http://faseb.org/Resources-for-the-Public/Scientific-Contests/BioArt/About-BioArt.aspx) and it won! Here's how I created the video.  
---

This year I entered the [FASEB BioArt Competition](http://faseb.org/Resources-for-the-Public/Scientific-Contests/BioArt/About-BioArt.aspx). I looked at the previous winners and noticed that images and videos of molecular structures was underrepresented. I have been refining my displays of protein structures by adding [ambient occlusion](https://en.wikipedia.org/wiki/Ambient_occlusion) lighting to space filling models[^footnote], and I thought one of these may have a reasonable chance of winning. Also, my videos of NMR states of proteins are always a hit in my classes, so I decided to submit a video that uses ambient occlusion lighting to increase its visual appeal. I was notified in December that my video was chosen as one of the [the 2017 BioArt winners](http://www.faseb.org/Resources-for-the-Public/Scientific-Contests/BioArt/Past-Winners/2017-BioArt-Winners.aspx). Below are the steps you can use to create the winning video.  

- **Choose a structure file.** I chose profilin, an important protein for actin polymerization in eukaryotic cells, as the subject of my video. Go to the [RCSB Protein Data Bank](http://www.rcsb.org/pdb/home/home.do) and search for "profilin". Refine your search by clicking on `Solution NMR` under `EXPERIMENTAL METHOD` in the left menu. I chose human profilin, 1PFL. 

- **Create the transition `.pdb` files between the NMR states.** Go to the [MovieMaker](http://wishart.biology.ualberta.ca/moviemaker/) web site and choose `NMR Ensemble Motions` and click `Continue`. Enter the PDB accession number, 1PFL, into the `Enter a PDB accession number:` box. Click `Submit`. I usually takes a few minutes (sometimes longer) to complete the morphing file creation. It took about 6 minutes to complete the files for 1PFL. When the morphing is finished, download the files by clicking on `Download PDB Files`.

- **move and rename the morphing structure files.** Some of these steps are not strictly necessary, but renaming and moving the files helps me stay organized. 

Move the files:

```bash
$ cd /Users/davidoppenheimer/pymol-work/1-morphing/MovieMaker/Profilin-1PFL
$ mkdir pdb-files
$ cd ~/Downloads/pdb_files
$ mv *.pdb /Users/davidoppenheimer/pymol-work/1-morphing/MovieMaker/Profilin-1PFL/pdb-files
$ cd ../
$ rmdir pdb_files
```

Rename the `pdb` files.


```bash
$ cd /Users/davidoppenheimer/pymol-work/1-morphing/MovieMaker/Profilin-1PFL/pdb-files
$ for file in *.pdb
> do
> echo mv "$file" "${file/int_struct/1pfl}"
> done
```

Remove `echo` to actually rename the files. Leave `echo` in to test the command. 

**Style the protein in PyMOL.** Start PyMOL and style the protein to your liking. 



[^footnote]: [Here](https://www.cgl.ucsf.edu/chimera/data/ambient-jul2014/ambient.html) are some examples of ambient occlusion lighting of biological macromolecules.