Arf1 GTPase

2KSQ
The myristoylated yeast ARF1 in a GTP and bicelle bound conformation

2K5U
Solution structure of myirstoylated yeast ARF1 protein, GDP-bound


4BZI
The structure of the COPII coat assembled on membranes

## Modify the pdb files

Start Terminal.

```bash
$ cd ~/climber-work
$ mkdir Arf1
$ cd Arf1
```

The files are from a solution structure so there are many states. We only want 1 state for Climber.

```python
> fetch 2k5u, async=0
> split_states 2k5u
> delete 2k5u
> extract GDP, /2k5u_0001/B/A/GDP
> disable all
> enable 2k5u_0001
> cd ~/climber-work/Arf1
> save 2k5u_orig.pdb, 2k5u_0001, state=-1
> reinitialize
cd ~/pymol-work/1-scripts


```

remove the other atoms (GTP and myrostylation)









```python
fetch 2ksq, async=0
split_states 2ksq
delete 2ksq
extract GTP, /2ksq_0001/G/A/GTP
extract chainB, /2ksq_0001/B
extract chainC, /2ksq_0001/C
extract chainD, /2ksq_0001/D
extract chainE, /2ksq_0001/E
extract chainF, /2ksq_0001/F
disable all
enable 2ksq_0001
cd ~/climber-work/Arf1
save 2ksq_orig.pdb, 2ksq_0001, state=-1
reinitialize 
#load 2ksq_orig.pdb
cd ~/pymol-work/1-scripts
```

```python









Save the single file `2k5u_0001` as a `.pdb` file in the `climber-work/Arf1` directory.  Repeat the steps for `2KSQ`.

```python
> reinitialize
> fetch 2ksq
> split_states 2ksq
> delete 2ksq
> disable all
> enable 2ksq_0001
```

Set the environment variable `CLIMBERDIR`.

```bash
$ export CLIMBERDIR=~/climber-work/Climber
```

Create an alignment.

```bash
$ awk '/ATOM/ {print}' < 2k5u_orig.pdb > 2k5u.pdb
$ awk '/ATOM/ {print}' < 2ksq_orig.pdb > 2ksq.pdb
$ ${CLIMBERDIR}/sh/rms.sh 2k5u 2ksq > morphx.al1
```

Morph 50 steps. **Note:** This step can take a while depending on how many steps you chose. On my laptop with 50 steps, Climber created xxx `.pdb` files in 1 hour and 35 minutes.

```bash
$ ${CLIMBERDIR}/sh/morphx.sh 2k5u 2ksq 50
```

88 files were made
Load morph files into pymol

```python
> cd ~/climber-work/Arf1/2k5u_2ksq_050step
> for idx in range(0,87):cmd.load("file%05d.pdb"%idx,"mov")
```


GAP activity assay may do-able

rho, rac, cdc42, arf, all 


Arf1 (golgi and actin) and Arf6 (actin via rho)

cdc42

rac


   N-Myristoylation inhibitors
   
   http://www.jlr.org/content/early/2012/07/24/jlr.D026997.full.pdf









