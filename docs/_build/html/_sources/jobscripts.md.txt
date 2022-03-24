# Job Scripts

For NERSC clusters, you can use the [NERSC job script generator](https://my.nersc.gov/script_generator.php).

## Cori Haswell
For Cori-hsw, use VASP 5 (pure MPI)

```
#!/bin/bash
#SBATCH -N 4
#SBATCH -C haswell
#SBATCH -q regular
#SBATCH -t 00:30:00

#OpenMP settings:
export OMP_NUM_THREADS=1
export OMP_PLACES=threads
export OMP_PROC_BIND=spread

module load vasp/5.4.4-hsw

srun -n 128 -c 2 --cpu_bind=cores vasp_ncl
```
The `-c` flag is always `2` for Haswell in pure MPI mode.

## Cori Knight's Landing
For Cori-knl, use VASP 6 (Hybrid OpenMP+MPI)

```
#!/bin/bash
#SBATCH -N 4
#SBATCH -C knl
#SBATCH -q regular
#SBATCH -t 00:30:00

#OpenMP settings:
export OMP_NUM_THREADS=4
export OMP_PLACES=threads
export OMP_PROC_BIND=spread

module load vasp/6.3.0-knl

srun -n 64 -c 16 --cpu_bind=cores vasp_ncl

```

You want the product of the `-c` flag and the `-n` flag to be `num_nodes * 64 cores/node * 4 threads/core` on KNL. Note that KNL technically has 68 cores per node, but we usually stick to 64. As for the `-c` flag, you want that to be `$OMP_NUM_THREADS * 4`.


## LRC
