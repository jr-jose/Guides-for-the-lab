# How to launch jobs in CNAG server.


The CNAG server uses *SLURM* as the workload manager. Next lines are a quick 
summary of the basic possibilities it offers. Everything else is available in
the *SLURM* documentation (visit the *users* sections at any time!):
https://slurm.schedmd.com/documentation.html .




### Table of contents.


+ [Launching jobs.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/launching_jobs_with_slurm.md#launching-jobs)
+ [Controlling jobs.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/launching_jobs_with_slurm.md#controlling-jobs)
+ [Complicated script examples.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/launching_jobs_with_slurm.md#complicated-script-examples)
    * [General scripts.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/launching_jobs_with_slurm.md#general-scripts)
    * [R scripts.](https://github.com/jr-jose/Guides-for-the-lab/blob/master/launching_jobs_with_slurm.md#r-scripts)


### Launching jobs.


Jobs are launched using *SBATCH* scripts.  These scripts are always divided in
two parts: the first one has the *SBATCH* directives, the second one has the
commands to be launched.


Here you have a simple *SBATCH* script template to be used. Copy, paste and 
modify these options to launch jobs.


```
#!/bin/bash

##############################
##   'sbatch' directives    ##
##############################

#SBATCH --job-name=test                 ## (name for your job)
#SBATCH --qos=lowprio                   ## (used queue: lowprio/normal/highprio)
#SBATCH --time='167:59:58'              ## (time you are reserving in hours)
##SBATCH --dependency=afterok:job_id    ## (stopping till finishing another job)
#SBATCH --ntasks=1                      ## (number of processes when MPI)
#SBATCH --cpus-per-task=1               ## (cpu's used when multithreading)
##SBATCH --mem-per-cpu=4GB              ## (reserved memory for each threading)
##SBATCH --mem=52GB                     ## (reserved memory for each node)
##SBATCH -D=/scratch/beekman/user_name  ## (folder to launch your job in)
##SBATCH --output=/scratch/beekman/user_name/outputs/job_%j.out

##############################
##    needed environment    ##
##############################

module load PYTHON/2.7.6 MACS2/20160309 gsl/2.4 R/4.0.1    ## (modules you need)

##############################
##    commands execution    ##
##############################

srun echo "hello" > hello.txt           ## (launch the commands you want, one )
srun time sleep 25                      ## (... after another and using 'srun')

Rscript your_R_script.R                 ## (use 'Rscript' to launch R scripts)

exit 0                                  ## (it throws 0 when arrives to the end)

```


First line of the *SBATCH* script indicates the location of the *bash* command.
Check the specific location in your computer launching 'which bash'.


As shown in the example, *SBATCH* directives are preceded by hash symbols ('#').
Two hash symbols ('##') are used to deactivate/comment any directive or command.


The command to be executed should be preceded by `Rscript` or `srun`, depending
if it is a *R* script or just a command line to be launched in the terminal.


To launch your *SBATCH* script, do it using the `sbatch script_file_name.sh` 
command (adding the 'sh' suffix to your *SBATCH* script name is always a good
practice). Change the permissions of your script (with 'chmod') when needed.


Note: if you ask more resources (e.g.: time, memory or cpu's), you will wait
more time in the queue.

### Controlling jobs:


* `squeue -u your_user_name` to see launched jobs.
* `scancel job_id` to cancel launched jobs.
* `scontrol hold job_id` to place a job waiting/held.
* `scontrol release job_id` to release held jobs.
* `scontrol show job -dd job_id` to get detailed information about a job.
* `sacct` (or `sacct --long`) to get info. about running or finished jobs too.
* `sacct -o jobid,maxrss -j job_id` to check the maximum of required memory.
* `sinfo` to get info about nodes and partitions.
* `mnsh` to open a *shell* for 8 hours to make interactive tests; with *X*!


* Use the *command --help* or *man command* to know more about them.


Note: another way to start an interactive session (over-passing the *mnsh*
limitations) is to directly use *srun* with the *'--pty bash'* argument. E.g.: 
`srun --job-name "InteracJob" --cpus-per-task 1 --time 23:59:00 --pty bash`.


### Complicated script examples.


Don't use these templates if you don't understand each line/aspect.
Please, use the available template in the [*launching jobs* section](https://github.com/jr-jose/Guides-for-the-lab/blob/master/launching_jobs_with_slurm.md#launching-jobs).


##### General scripts:


```
#!/bin/bash

##
## Arguments: 'project_name' (after '-J'), the command and optional 'run_name'.
## e.g.: 'sbatch -J project_name ./test_template.sh "time sleep 5" run_name'.
##

##############################
##    'slurm' directives    ##
##############################

#SBATCH --job-name=test                 ## (nombre de proyeco -su directorio-)
#SBATCH --qos=lowprio                   ## (used queue: lowprio/normal/highprio)
#SBATCH --time='00:00:59'               ## (¡obligado!; formato 'DD-HH:MM:SS')
# #SBATCH --begin='2020-06-01'          ## (formato 'YYYY-MM-DD[THH:MM[:SS]]')
# #SBATCH --dependency=afterok:job_id   ## ('afterok/afternotok/afterany')
#SBATCH --comment="running comments"    ## (comentarios sobre el lanzamiento)

#SBATCH --partition=main                ## ('main/genB/gpu/interactive/smp')
# #SBATCH --nodelist=cnc[1-10]          ## (grupo de nodos que serán usados)
# #SBATCH --exclude=cnc[11-20]          ## (grupo de nodos que NO se usarán)
#SBATCH --nodes=1                       ## (número de nodos -ordenadores-)
#SBATCH --ntasks=1                      ## (MPI; número procesos en paralelo)
# #SBATCH --ntasks-per-node=1           ## (número procesos para cada nodo)
# #SBATCH --mincpus=1                   ## (número mínimo de CPUs por nodo)
# #SBATCH --mem=4GB                     ## (memoria reservada para cada nodo)
#SBATCH --cpus-per-task=1               ## (multithreading; añadir N a script)
#SBATCH --mem-per-cpu=4MB               ## (memoria reservada para cada hilo)
# #SBATCH --threads-per-core=1          ## (hilos asociados a cada procesador)

#SBATCH --workdir=/scratch/beekman/johernandez
#SBATCH --output=/scratch/beekman/johernandez/job_%j.out
# #SBATCH --error=/scratch/beekman/johernandez/job_%j.err
# #SBATCH --mail-user=joseramon.hernandez\@crg.eu
# #SBATCH --mail-type=END,FAIL          ## ('NONE/BEGIN/END/FAIL/ALL/')

##############################
##     user environment     ##
##############################

export PATH=$PATH                        ## (paths extra tras 'module load')
export MANPATH=$MANPATH                  ## (a añadir nuevos paths tras ":")
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH

##############################
##    'working' folders     ##
##############################

T0a=$(date +%y%m%d.%H%M%S.%2N)
T0b=$(date +%H:%M_on_%d/%m/%y)
if [ "${2}" != "" ]; then SUFFIX="${2}__${T0a}"
else SUFFIX="${T0a}"; fi
HOME_DIR="$HOME/Projects/${SLURM_JOB_NAME}"
INITIAL_DIR="$(pwd)"
SCRATCH_DIR="${INITIAL_DIR}/${SLURM_JOB_NAME}"
JOB_DIR="${SCRATCH_DIR}/${SUFFIX}"

mkdir -p ${HOME_DIR}
mkdir -p ${JOB_DIR}
cd ${JOB_DIR}
if [ -d "${HOME_DIR}/Initial.data" -o -h "${HOME_DIR}/Initial.data" ]; then
cp -au ${HOME_DIR}/Initial.data ${SCRATCH_DIR}
fi

##############################
##   initial information    ##
##############################

echo ---------------------------------------------------------------------------------
echo "Job launched at $T0b by $USER (${SLURM_JOB_ACCOUNT}'s account at ${SLURM_CLUSTER_NAME})."
echo ---------------------------------------------------------------------------------
echo " "
echo " * The job identifier is ${SLURM_JOBID}."
echo " * The job belongs to a project named ${SLURM_JOB_NAME}."
echo " * The working directory is ${JOB_DIR}."
echo " * The saving directory is ${HOME_DIR}/${SUFFIX}."
if [ "${SLURM_MEM_PER_NODE}" != "" ]; then
echo " * The reserved memory was ${SLURM_MEM_PER_NODE} MB."
fi
echo " * The number of reserved threads was ${SLURM_CPUS_PER_TASK}."
if [ "${SLURM_MEM_PER_CPU}" != "" ]; then
echo " * The reserved memory per thread was ${SLURM_MEM_PER_CPU} MB."
fi
echo " "
echo " * Loaded paths = $PATH"
if [ "${LD_LIBRARY_PATH}" != "" ]; then
echo " * Loaded library paths = $LD_LIBRARY_PATH"
fi
echo " "
echo ---------------------------------------------------------------------------------
echo " "
echo " Launched command:"
echo "srun ${1}"
echo " "
echo ---------------------------------------------------------------------------------

##############################
##    commands execution    ##
##############################

srun ${1}
#Rscript script.R

##############################
##    final information     ##
##############################

T1=$(date +%H:%M_on_%d/%m/%y)

echo " "
echo ---------------------------------------------------------------------------------
echo " "
echo " * Starting:  $T0b."
echo " * Finishing: $T1."
echo " "
srun sstat -o JobID,AveCPU,NTasks,AveRSS,MaxRSS,AveVMSize,MaxVMSize -j ${SLURM_JOBID} >> /scratch/beekman/johernandez/job_${SLURM_JOBID}.out
echo " "
echo ---------------------------------------------------------------------------------

##############################
##      saving results      ##
##############################

mv ${INITIAL_DIR}/job_${SLURM_JOBID}.out ${HOME_DIR}/.job__${SUFFIX}.out
if [ -e "${INITIAL_DIR}/job_${SLURM_JOBID}.err" ]; then
mv ${INITIAL_DIR}/job_${SLURM_JOBID}.err ${HOME_DIR}/.job__${SUFFIX}.err
fi
if [ "${2}" != "" ]; then cp -a ${JOB_DIR} ${HOME_DIR}/
else cp -a ${JOB_DIR}/* ${HOME_DIR}/ ; fi
rm -rf ${JOB_DIR}
if [ ! -d "${SCRATCH_DIR}/Initial.data" -a ! -h "${SCRATCH_DIR}/Initial.data" ]; then
rm -rf ${SCRATCH_DIR}
fi

exit 0

```


##### R scripts:


```
#!/bin/bash

##
## Arguments: 'project_name' (after '-J') and the R script to be launched. 
## e.g.: 'sbatch -J project_name ./R_template.sh script.R'.
##

##############################
##    'slurm' directives    ##
##############################

#SBATCH --job-name=no_name              ## (nombre de proyeco -su directorio-)
#SBATCH --qos=lowprio                   ## (used queue: lowprio/normal/highprio)
#SBATCH --time='07:59:59'               ## (¡obligado!; formato 'DD-HH:MM:SS')
# #SBATCH --begin='2020-06-01'          ## (formato 'YYYY-MM-DD[THH:MM[:SS]]')
# #SBATCH --dependency=afterok:job_id   ## ('afterok/afternotok/afterany')
#SBATCH --comment="running comments"    ## (comentarios sobre el lanzamiento)

#SBATCH --partition=main                ## ('main/genB/gpu/interactive/smp')
# #SBATCH --nodelist=cnc[1-10]          ## (grupo de nodos que serán usados)
# #SBATCH --exclude=cnc[11-20]          ## (grupo de nodos que NO se usarán)
#SBATCH --nodes=1                       ## (número de nodos -ordenadores-)
#SBATCH --ntasks=1                      ## (MPI; número procesos en paralelo)
# #SBATCH --ntasks-per-node=1           ## (número procesos para cada nodo)
# #SBATCH --mincpus=1                   ## (número mínimo de CPUs por nodo)
# #SBATCH --mem=4GB                     ## (memoria reservada para cada nodo)
#SBATCH --cpus-per-task=1               ## (multithreading; añadir N a script)
#SBATCH --mem-per-cpu=4GB               ## (memoria reservada para cada hilo)
# #SBATCH --threads-per-core=1          ## (hilos asociados a cada procesador)

#SBATCH --workdir=/scratch/beekman/johernandez
#SBATCH --output=/scratch/beekman/johernandez/job_%j.out
# #SBATCH --error=/scratch/beekman/johernandez/job_%j.err
# #SBATCH --mail-user=joseramon.hernandez\@crg.eu
# #SBATCH --mail-type=END,FAIL          ## ('NONE/BEGIN/END/FAIL/ALL/')

##############################
##     user environment     ##
##############################

export PATH=/apps/R/3.6.0/bin:/apps/GCC/6.3.0/bin:$PATH
export MANPATH=/apps/R/3.6.0/share/man:/apps/GCC/6.3.0/man:$MANPATH
export LD_LIBRARY_PATH=/apps/R/3.6.0/lib64:/apps/GCC/SRC/extras/MPFR/3.1.5/lib:/apps/R/3.6.0/lib64/R/lib:/apps/R/3.6.0/library:/apps/ZLIB/1.2.8/lib:/apps/BZIP2/1.0.6/lib:/apps/XZ/5.2.2/lib:/apps/PCRE/8.38/lib:/apps/CURL/7.48.0/lib:/apps/GCC/6.3.0/lib64:/apps/GCC/6.3.0/lib:$LD_LIBRARY_PATH

##############################
##    'working' folders     ##
##############################

T0a=$(date +%y%m%d.%H%M%S.%2N)
T0b=$(date +%H:%M_on_%d/%m/%y)
HOME_DIR="$HOME/Projects/${SLURM_JOB_NAME}"
INITIAL_DIR="$(pwd)"
SCRATCH_DIR="${INITIAL_DIR}/${SLURM_JOB_NAME}"
JOB_DIR="${SCRATCH_DIR}/${T0a}"

mkdir -p ${HOME_DIR}
mkdir -p ${JOB_DIR}
cd ${JOB_DIR}
if [ -d "${HOME_DIR}/Initial.data" -o -h "${HOME_DIR}/Initial.data" ]; then
cp -au ${HOME_DIR}/Initial.data ${SCRATCH_DIR}
fi

##############################
##   initial information    ##
##############################

echo ---------------------------------------------------------------------------------
echo "Job launched at $T0b by $USER (${SLURM_JOB_ACCOUNT}'s account at ${SLURM_CLUSTER_NAME})."
echo ---------------------------------------------------------------------------------
echo " "
echo " * The job identifier is ${SLURM_JOBID}."
echo " * The job belongs to a project named ${SLURM_JOB_NAME}."
echo " * The working directory is ${JOB_DIR}."
echo " * The saving directory is ${HOME_DIR}."
if [ "${SLURM_MEM_PER_NODE}" != "" ]; then
echo " * The reserved memory was ${SLURM_MEM_PER_NODE} MB."
fi
echo " * The number of reserved threads was ${SLURM_CPUS_PER_TASK}."
if [ "${SLURM_MEM_PER_CPU}" != "" ]; then
echo " * The reserved memory per thread was ${SLURM_MEM_PER_CPU} MB."
fi
echo " "
echo " * Loaded paths = $PATH"
if [ "${LD_LIBRARY_PATH}" != "" ]; then
echo " * Loaded library paths = $LD_LIBRARY_PATH"
fi
echo " "
echo ---------------------------------------------------------------------------------
echo " "
echo " Launched command:"
echo "Rscript ${1}"
echo " "
echo ---------------------------------------------------------------------------------

##############################
##    commands execution    ##
##############################

Rscript "${1}"
#srun script.R

##############################
##    final information     ##
##############################

T1=$(date +%H:%M_on_%d/%m/%y)

echo " "
echo ---------------------------------------------------------------------------------
echo " "
echo " * Starting:  $T0b."
echo " * Finishing: $T1."
echo " "
srun sstat -o JobID,AveCPU,NTasks,AveRSS,MaxRSS,AveVMSize,MaxVMSize -j ${SLURM_JOBID} >> /scratch/beekman/johernandez/job_${SLURM_JOBID}.out
echo " "
echo ---------------------------------------------------------------------------------

##############################
##      saving results      ##
##############################

mv ${INITIAL_DIR}/job_${SLURM_JOBID}.out ${HOME_DIR}/.job__${T0a}.out
if [ -e "${INITIAL_DIR}/job_${SLURM_JOBID}.err" ]; then
mv ${INITIAL_DIR}/job_${SLURM_JOBID}.err ${HOME_DIR}/.job__${T0a}.err
fi
cp -a ${JOB_DIR}/* ${HOME_DIR}/
rm -rf ${JOB_DIR}
if [ ! -d "${SCRATCH_DIR}/Initial.data" -a ! -h "${SCRATCH_DIR}/Initial.data" ]; then
rm -rf ${SCRATCH_DIR}
fi

exit 0
```
