## General exercise instructions

For most of the exercises, skeleton codes are provided both for Fortran and C
in the corresponding subdirectory. Some exercise skeletons have sections
marked with “TODO” for completing the exercises. In addition, all of the
exercises have exemplary full codes (that can be compiled and run) in the
`solutions` folder. Note that these are seldom the only or even the best way to
solve the problem.

### Computing servers
We will use CSC’s Cray XC40 supercomputer Sisu for the exercises. Log onto
Sisu using the provided username and password, for example

```
ssh –X user@sisu.csc.fi
```

For editing program source files you can use e.g. Emacs editor with or without
(the option –nw) X-Windows:

```
emacs –nw prog.f90
emacs prog.f90
```

Also other popular editors (vim, nano) are available.
In case you have working parallel program development environment in your
laptop (Fortran or C compiler, MPI development library, etc.) you may also use
that.

## Compilation and execution
Compilation of the source codes is performed with the `ftn` and `cc` wrapper
commands:
```
ftn –o my_mpi_exe_test.f90
```
or
```
cc –o my mpi_exe_test.c
```

The wrapper commands include automatically all the flags needed for building
MPI programs.
Typically we will use the default Cray compiling environment. There are also
other compilers (GNU and Intel) available on Sisu, which  can be changed via
(for example)
```
module swap PrgEnv-cray PrgEnv-gnu
```
Use the commands module list and module avail to see the currently loaded and
available modules, respectively.
The programs can be executed in the sc. interactive nodes as
```
aprun –n 4 ./my_mpi_exe
```
For executing OpenMP jobs on the interactive nodes, do
```
aprun –n 1 –d 4 –e OMP_NUM_THREADS=4 ./my_omp_exe
```
Similarly, a hybrid MPI+OpenMP would be invoked for example as
```
aprun –n 4 –d 6 –e OMP_NUM_THREADS=6 ./my_hyb_exe
```

### Batch jobs
Larger and/or longer runs should be submitted via the batch system. An example
batch job script (a text file, let’s call it `sisu_job.sh`) for a MPI job could
look like
```
#!/bin/bash
#SBATCH -t 00:10:00
#SBATCH -J MPI_job
#SBATCH -o out.%j
#SBATCH -e err.%j
#SBATCH -p test
#SBATCH --nodes=1
aprun -n 4 ./my_mpi_exe
```
Here we run job with 4 tasks on 1 node.

The batch script is then submitted with the sbatch command as
```
sbatch sisu_job.sh
```
See the Sisu User’s Guide at http://research.csc.fi/sisu-user-guide for more
details.
