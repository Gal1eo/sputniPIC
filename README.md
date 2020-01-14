# This is an implementation of porting PIC to GPU
The Particle-in-Cell or PIC method is a computational technique used in space, astrophysics and fusion tools to 
simulate electron and proton dynamics. In this repo, we implemented a GPU version PIC method based on the original 
[repo](https://github.com/iPIC3D/sputniPIC).

## content
| Section | Description |
|-|-|
| [Downloading](#Downloading) | How to download the files |
| [Compiling](#Compiling) | How to load modules and compiling |
| [Allocating](#Allocating) | How to allocate specified GPU |
| [Running](#Running) | How to run the program |


## Downloading
This repo can be downloaded as follows:
```bash
git clone https://github.com/Greeser/sputniPIC.git
```
Before compiling, the directory should be changed；
```bash
cd sputniPIC/
```

## Compiling
This program is tested under the service of [PDC of KTH Royal Institute of Technology](https://www.pdc.kth.se/).

Before compiling, the compiler module should be loaded as follows:
```bash
module load gcc/6.2.0
module load openmpi/3.0-gcc-6.2
module load cuda/10.0
```

Then the compiling could be done as follows:
```bash
make clean
make
```

## Allocating
This program is tested using [5 fat nodes NVIDIA Quadro K420](https://www.pdc.kth.se/hpc-services/computing-systems/tegner-1.737437)

The GPU allocation can be done as follows:
```bash
salloc --nodes=1 --mem=1000000 --gres=gpu:K420:1 -t 00:05:00 -A edu19.DD2360
```

## running 
By checkout to different commit, the program can be runned at different stages. After checkout the program should be recompile.

### Only port particle mover to GPU
```bash
git checkout d357d4c945ce28a92518c29182921a9e1c19f85f
make clean
make
srun -n 1 nvprof ./bin/sputniPIC.out inputfiles/GEM_2D.inp
```

### Port both particle mover and interpolation to GPU
```bash
git checkout 8b5e9e7a2b61577a0da47d559280c07e8d2f9c30
make clean
make
srun -n 1 nvprof ./bin/sputniPIC.out inputfiles/GEM_2D.inp
```

### Mini-batches approach
```bash
git checkout 6915503252668f3ef7bc51874c0905e424b8067f
make clean
make
srun -n 1 nvprof ./bin/sputniPIC.out inputfiles/GEM_2D.inp
```

### Using streams and async copy
```bash
git checkout 77924dab45959d99796a76074ebfd82898bffbac
make clean
make
srun -n 1 nvprof ./bin/sputniPIC.out inputfiles/GEM_2D.inp
srun -n 1 nvprof ./bin/sputniPIC.out inputfiles/GEM_2D_2.inp
srun -n 1 nvprof ./bin/sputniPIC.out inputfiles/GEM_2D_3.inp
srun -n 1 nvprof ./bin/sputniPIC.out inputfiles/GEM_2D_4.inp
```

### Notice
The files of GEM_2D_2.inp, GEM_2D_3.inp, and GEM_2D_4.inp only exist after commit b85868ba30dbef59b76f9b900486098a24f189b7.
