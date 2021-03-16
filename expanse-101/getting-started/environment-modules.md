---
description: Customizing Your User Environment
---

# Environment Modules

The Environment Modules package provides for dynamic modification of your shell environment. Module commands set, change, or delete environment variables, typically in support of a particular application. They also let the user choose between different versions of the same software or different combinations of related codes. See the [Expanse User Guide](https://www.sdsc.edu/support/user_guides/expanse.html#modules).

## Introduction to Lmod

Expanse uses [Lmod](https://lmod.readthedocs.io/en/latest/010_user.html), a Lua based module system. Users setup custom environments by loading available modules into the shell environment, including needed compilers and libraries and the batch scheduler.

### What's the same as Comet:

* Dynamic modification of your shell environment
* User can set, change, or delete environment variables
* User chooses between different versions of the same software or different combinations of related codes.

### What's Different?

* Users will need to load the scheduler \(e.g. Slurm\)
* Depending on which hardware users are working on, users will need to load either the `gpu` or `cpu` modules.
* Users will not see all available modules when they run command "module available" without loading a compiler.
* Use the command "module spider" option to see if a particular package exists and can be loaded, run command
  * `module spider`
  * `module keywords`
* For additional details, and to identify module dependencies modules, use the command
  * `module spider`
* The module paths are different for the CPU and GPU nodes. Users can enable the paths by loading the following modules:            
  * `module load cpu`  \(for cpu nodes\)
  * `module load gpu`  \(for gpu nodes\)     

{% hint style="danger" %}
#### Avoid loading both modules
{% endhint %}

## Popular Lmod Commands

Here are some common module commands and their descriptions

| Lmod Command | Description |
| :--- | :--- |
| `module purge` | Remove all modules that are currently loaded |
| `module list` | List the modules that are currently loaded |
| `module avail` | List the modules that are available in environment |
| `module spider` | List of the modules and extensions currently available |
| `module display` | Show the environment variables used by  and how they are affected |
| `module unload` | Remove  from the environment |
| `module load` | Load  into the environment |
| `module swap` | Replace  with  in the environment |
| `module save` | Save the current list of modules to "name" collection. |
| `module savelist` | List of saved module collections. |
| `module describe` | Describe the contents of a module collection. |
| `module help` | Get a list of all the commands that module knows about do: |

Lmod commands support _short-hand_ notation, for example:

`ml foo` == `module load foo`

`foo ml -bar` == `module unload bar`

{% hint style="info" %}
SDSC Guidance: add module calls to your environment and batch scripts
{% endhint %}

#### Examples

* Default environment for a new user/new login: `list`, `li`

```text
(base) [user@login01 expanse-101]$ module list
Currently Loaded Modules:
1) shared   2) slurm/expanse/20.02.3   3) cpu/0.15.4   4) DefaultModules
```

* List available modules:  `available`, `avail`, `av`

```text
$ module av
[user@expanse-ln3:~] module av
(base) [user@login01 expanse-101]$ module available

--------------- /cm/shared/apps/spack/cpu/lmod/linux-centos8-x86_64/Core ----------------
   abaqus/2018     gaussian/16.C.01        gmp/6.1.2           mpfr/4.0.2
   aocc/2.2.0      gcc/7.5.0               intel/19.1.1.217    openjdk/11.0.2
   cmake/3.18.2    gcc/9.2.0               libtirpc/1.2.6      parallel/20200822
   emboss/6.6.0    gcc/10.2.0       (D)    matlab/2020b        subversion/1.14.0

--------------------------------- /cm/local/modulefiles ---------------------------------
   cluster-tools-dell/9.0        gcc/9.2.0                    null
   cluster-tools/9.0             gpu/1.0                      openldap
   cmd                           ipmitool/1.8.18              python3
   cmjob                         kubernetes/expanse/1.18.8    python37
   cpu/1.0                (L)    lua/5.3.5                    shared                (L)
   docker/19.03.5                luajit                       singularitypro/3.5
   dot                           module-git                   slurm/expanse/20.02.3
   freeipmi/1.6.4                module-info

-------------------------------- /usr/share/modulefiles ---------------------------------
   DefaultModules (L)    gct/6.2    globus/6.0

-------------------------------- /cm/shared/modulefiles ---------------------------------
   bonnie++/1.98                default-environment           netperf/2.7.0
   cm-pmix3/3.1.4               gdb/8.3.1                     openblas/dynamic/0.3.7
   cuda10.2/blas/10.2.89        hdf5/1.10.1                   openmpi/gcc/64/1.10.7
   cuda10.2/fft/10.2.89         hdf5_18/1.8.21                sdsc/1.0
   cuda10.2/nsight/10.2.89      hwloc/1.11.11                 ucx/1.6.1
   cuda10.2/profiler/10.2.89    iozone/3_487
   cuda10.2/toolkit/10.2.89     netcdf/gcc/64/gcc/64/4.7.3

  Where:
   L:  Module is loaded
   D:  Default Module
```

{% hint style="info" %}
Module defaults are chosen based on Find First Rules due to Name/Version/Version modules found in the module tree. See [https://lmod.readthedocs.io/en/latest/060\_locating.html](https://lmod.readthedocs.io/en/latest/060_locating.html) for details.
{% endhint %}

Use `module spider` to find all possible modules and extensions.

```text
(base) [user@login02 ~]$ module spider MPI
-------------------------------------------------------------------------------------
  intel-mpi: intel-mpi/2019.8.254
-------------------------------------------------------------------------------------
    You will need to load the set of module(s) on any one of the lines below before the "intel-mpi/2019.8.254" module is available to load.

      cpu/1.0  gcc/10.2.0
      cpu/1.0  intel/19.1.1.217
      gpu/1.0
      gpu/1.0  intel/19.0.5.281
      gpu/1.0  pgi/20.4

    Help:
      Intel MPI
-------------------------------------------------------------------------------------
  openmpi:
-------------------------------------------------------------------------------------
     Versions:
        openmpi/3.1.6
        openmpi/4.0.4-nocuda
        openmpi/4.0.4
-------------------------------------------------------------------------------------
  For detailed information about a specific "openmpi" package (including how to load the modules) use the module's full name. Note that names that have a trailing (E) are extensions provided by other modules.
  For example:

     $ module spider openmpi/4.0.4
-------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------
  openmpi/gcc/64: openmpi/gcc/64/1.10.7
-------------------------------------------------------------------------------------
    You will need to load all module(s) on any one of the lines below before the "openmpi/gcc/64/1.10.7" module is available to load.
      shared
    Help:
        Adds OpenMPI to your environment variables,
```

## Environment Checking

In this example, we will add the Slurm library, and and verify that it is in your environment

* Check module environment after loggin on to the system:

```text
(base) [user@login01 ~]$ module li

Currently Loaded Modules:
  1) shared   2) slurm/expanse/20.02.3   3) cpu/0.15.4   4) DefaultModules
```

* Note that Slurm \(the cluster resource manager\) is not in the environment.

  Check environment looking for Slurm commands

```text
(base) [user@login01 ~]$ which squeue
/usr/bin/which: no squeue in (/home/user/miniconda3/bin/conda:/home/user/miniconda3/bin:/home/user/miniconda3/condabin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/dell/srvadmin/bin:/home/user/.local/bin:/home/user/bin)
```

* Since Slurm commands do not exist,  we need to load that module:

```text
(base) [user@login01 ~]$ module load slurm
(base) [user@login01 ~]$ which squeue
/cm/shared/apps/Slurm/current/bin/squeue
```

* Display loaded module details:

```text
(base) [user@login02 ~]$ module display Slurm
-------------------------------------------------------------------------------------
   /cm/local/modulefiles/slurm/expanse/20.02.3:
-------------------------------------------------------------------------------------
whatis("Adds Slurm to your environment ")
setenv("CMD_WLM_CLUSTER_NAME","expanse")
setenv("Slurm_CONF","/cm/shared/apps/Slurm/var/etc/expanse/Slurm.conf")
prepend_path("PATH","/cm/shared/apps/Slurm/current/bin")
prepend_path("PATH","/cm/shared/apps/Slurm/current/sbin")
prepend_path("MANPATH","/cm/shared/apps/Slurm/current/man")
prepend_path("LD_LIBRARY_PATH","/cm/shared/apps/Slurm/current/lib64")
prepend_path("LD_LIBRARY_PATH","/cm/shared/apps/Slurm/current/lib64/Slurm")
prepend_path("LIBRARY_PATH","/cm/shared/apps/Slurm/current/lib64")
prepend_path("LIBRARY_PATH","/cm/shared/apps/Slurm/current/lib64/Slurm")
prepend_path("CPATH","/cm/shared/apps/Slurm/current/include")
help([[ Adds Slurm to your environment
]])
```

Once you have loaded the modules, you can check the system variables that are available for you to use.

* To see all variable, run the **`env`** command. Typically, you will see more than 60 lines containing information such as your login name, shell, your home directory:

```text
[user@expanse-ln3 IBRUN]$ env
CONDA_EXE=/home/user/miniconda3/bin/conda
__LMOD_REF_COUNT_PATH=/cm/shared/apps/Slurm/current/sbin:1;/cm/shared/apps/Slurm/current/bin:1;/home/user/miniconda3/bin/conda:1;/home/user/miniconda3/bin:1;/home/user/miniconda3/condabin:1;/usr/local/bin:1;/usr/bin:1;/usr/local/sbin:1;/usr/sbin:1;/opt/dell/srvadmin/bin:1;/home/user/.local/bin:1;/home/user/bin:1
HOSTNAME=login02
USER=user
HOME=/home/user
CONDA_PYTHON_EXE=/home/user/miniconda3/bin/python
BASH_ENV=/usr/share/lmod/lmod/init/bash
BASHRC_READ=1
LIBRARY_PATH=/cm/shared/apps/Slurm/current/lib64/Slurm:/cm/shared/apps/Slurm/current/lib64
Slurm_CONF=/cm/shared/apps/Slurm/var/etc/expanse/Slurm.conf
LOADEDMODULES=shared:cpu/1.0:DefaultModules:slurm/expanse/20.02.3
__LMOD_REF_COUNT_MANPATH=/cm/shared/apps/Slurm/current/man:1;/usr/share/lmod/lmod/share/man:1;/usr/local/. . . .
MANPATH=/cm/shared/apps/Slurm/current/man:/usr/share/lmod/lmod/share/man:/usr/local/share/man:/usr/share/man:/cm/local/apps/environment-modules/current/share/man
MODULEPATH=/cm/shared/apps/spack/cpu/lmod/linux-centos8-x86_64/Core:/cm/local/modulefiles:/etc/modulefiles:/usr/share/modulefiles:/usr/share/Modules/modulefiles:/cm/shared/modulefiles
MODULEPATH_ROOT=/usr/share/modulefiles
PATH=/cm/shared/apps/Slurm/current/sbin:/cm/shared/apps/Slurm/current/bin:/home/user/miniconda3/bin/conda:/home/user/miniconda3/bin:/home/user/miniconda3/condabin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/dell/srvadmin/bin:/home/user/.local/bin:/home/user/bin
_LMFILES_=/cm/local/modulefiles/shared:/cm/local/modulefiles/cpu/1.0.lua:/usr/share/modulefiles/DefaultModules.lua:/cm/local/modulefiles/slurm/expanse/20.02.3
MODULESHOME=/usr/share/lmod/lmod
CONDA_DEFAULT_ENV=base
```

To see the value for any of these variables, use the `echo` command. In this example we show how to activate your miniconda environment so you can run Jupyter Notebooks:

```text
(base) [user@login02 ~]$ echo $CONDA_PYTHON_EXE
[user@login02 ~]$
[user@login02 ~]$ conda activate
-bash: conda: command not found
[user@login02 ~]$ . /home/$USER/miniconda3/etc/profile.d/conda.sh
[user@login02 ~]$ conda activate
(base) [user@login02 ~]$ which jupyter
~/miniconda3/bin/jupyter
(base) [user@login02 ~]$ echo $CONDA_PYTHON_EXE
/home/user/miniconda3/bin/python
(base) [user@login02 ~]$
```

## Loading Modules at Login

You can override, and add to the standard set of login modules in two ways. 

1. Adding module commands to your personal startup files
2. Using `module save` command

{% hint style="info" %}
Make sure that you always want the module loaded at login
{% endhint %}

For Bash: put the following block into your `~/.bash_profile` file:

```bash
if [ -f ~/.bashrc ]; then
       . ~/.bashrc
fi
```

Place the following in your `~/.bashrc` file:

```text
# Place module commands here
# module load hdf5
```

* First edit your `.bashrc` and `.bash_profile` files:

```text
[user@login02 ~]$ cat .bash_profile
# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
. ~/.bashrc
fi
[SNIP]
[user@login01 ~]$
[user@login02 ~]$ cat .bashrc
# .bashrc
# Source global definitions
if [ -f /etc/bashrc ]; then
. /etc/bashrc
fi

# Place any module commands here
module load hdf5

[SNIP]
```

* Next LOGOUT and LOG BACK IN:

```text
(base) [user@login02 ~]$ env | grep Slurm
[snip]
MANPATH=/cm/shared/apps/Slurm/current/man:/usr/share/lmod/lmod/share/man:/usr/local/share/man:/usr/share/man:/cm/local/apps/environment-modules/current/share/man
PATH=/cm/shared/apps/Slurm/current/sbin:/cm/shared/apps/Slurm/current/bin:/home/user/miniconda3/bin/conda:/home/user/miniconda3/bin:/home/user/miniconda3/condabin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/dell/srvadmin/bin:/home/user/.local/bin:/home/user/bin
[snip]
(base) [user@login02 ~]$ which squeue
/cm/shared/apps/Slurm/current/bin/squeue
```

## Troubleshooting

### Lmod warning "rebuild your saved collection"

Lmod allows a user to save a bundle of modules as a collection using module save and module restore. This enables you to quickly get the same list of modules loaded if you tend to use the same modules over and over. With a new module scheme came a different system MODULEPATH. For this reason, if you have some module collections saved, you will experience the following warning: “Lmod Warning: The system MODULEPATH has changed: please rebuild your saved collection.” To solve this you need to remove your old collections and create them again.

* Too see the list of module collections that you currently have:

```text
[mthomas@login02 ~]$ module savelist
Named collection list :
  1) default  2) hdf5_env
```

* To remove or disable a saved collection:

```text
[mthomas@login02 ~]$ module disable hdf5_env
Disabling hdf5_env collection by renaming with a "~"
[mthomas@login02 ~]$ module savelist
Named collection list :
  1) default
[mthomas@login02 ~]$
```

### Module Error

Sometimes this error is encountered when switching from one shell to another or attempting to run the module command from within a shell script or batch job. The module command may not be inherited between the shells. To keep this from happening, execute the following command:

```text
[expanse-ln3:~]source /etc/profile.d/modules.sh
```

OR add this command to your shell script \(including Slurm batch scripts\)

