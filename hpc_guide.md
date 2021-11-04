# UCT HPC and Slurm Guide

This is a short introductory guide on the usage of UCT's High Performance Cluster (HPC) to run research-related processes that demand high compute power. This guide in essence will serve as an extension of the SSH guide, which may be found [here](https://github.com/African-Robotics-Unit/docs/blob/main/ssh_guide.md). If you are rusty on the fundamentals of remote development you may want to read the SSH guide first as we will be expanding on them here.

## Conceptual Overview of Compute Clusters

If you aren't interested in how compute clusters are configured and how they operate, and prefer to simply send your commands into the aether until they return yielding those precious results, I don't blame you. This section isn't necessary and exists purely to provide more of a theoretical understanding of what you are doing when you make use of a cluster.

### What is a Cluster?

A High Performance Cluster is a system comprised of a number of servers, called **nodes**. These nodes are much like everyday computers and are built from the same components, except they are each optimised for a specific task and are all interconnected within a fast internal network. Different types of nodes exist, namely:

- **The head node:** this is also called the login node and handles, as you would expect, user logins.

- **Data transfer nodes:** these nodes are dedicated to handling data transfer to and from the cluster.

- **Compute nodes:** these tend to be the majority of nodes in a cluster and are tasked with running most of the computations that users submit.

- **Fat nodes:** these "fat" compute nodes are named for their large storage space, usually in excess of 1 Tb, and are used to store large quantities of data.

- **GPU nodes:** these are compute nodes that have access to powerful GPUs, used to perform tasks such as training neural networks and image processing.

Additional hardware includes devices such as a switch, which directs traffic within the cluster network. When you connect to the cluster using `ssh`, you are connected to the head node and are assigned compute nodes or GPU nodes for your jobs as needed.

### Why Would I Use a Cluster?

Oftentimes, it is inconvenient to run intensive programs on your own hardware, if not impossible. An HPC provides on-demand computing power that you don't need to worry about setting up, maintaining, or buying, because UCT has already done all that for you! The benefits of HPCs include:

- Access to high-end hardware to perform tasks that are otherwise impossible on your local hardware.

- Processing times that are often much faster than anything you may set up yourself.

- Vast parallel computing resources that allow you to run multiple intensive tests/jobs simultaneously.

- Enhanced portability as you may control your processes from anywhere with an internet connection.

- If you are anywhere within the UCT network itself, you have the benefit of extremely fast data transfer speeds to and from the cluster. This is a major advantage of the UCT HPC over cloud computing services such as Google Cloud or even other clusters such as CHPC.

Basically, if a process that you need to run is intensive enough that you may need to run your own hardware overnight, you may want to move it to the HPC or find a similar cloud computing solution. This goes doubly if you're in a time crunch. If your process is not particularly intensive and doesn't require a high-end GPU, then you are probably better off simply running it locally.

## Slurm

Since many users are typically connected to the cluster at once needing to run intensive processes, we need to find a way to allocate resources efficiently and fairly. To this end, UCT HPC uses something called the Slurm Workload Manager.

Slurm is a software package that provides a set of commands for allocating jobs between compute nodes for processing. A 'job" is any intensive process, such as training a neural network, that a user submits to the cluster for processing. Slurm handles jobs in a queue: once a job is submitted, it is added to a queue where it waits for a compute node to become available. Thereafter, jobs are taken from the queue based on priority and moved to compute nodes for actual processing. The priority of a job increases the longer it waits in the queue, so your job shouldn't spend too long pending in the slurm queue. Once completed, the terminal output of your job is saved to a slurm file (eg. slurm-1102232.out) and the job is marked as completed.

In addition to submitting jobs to the queue (the commands for which we will cover here soon), you may also being an interactive Slurm session. This will allocate you a compute node with the resources you need as it becomes available. You may then interact with the compute node live, and any commands you issue will be executed immediately instead of being placed in a queue. This is useful for testing out new installations and for running quick jobs that do not require much processing time.

In the coming sections we'll talk a little more about the basic Slurm commands.

## Initial Setup and Basic Scripts

### Logging in

First, if you haven't already, talk to your supervisor about getting an HPC account set up. When the administrators set up a new account, a folder bearing your username will be created in the `/home/` directory of the **head node**. This home directory is where most of your work on the HPC will take place; it is where you will install local dependencies, where you will mostly place Slurm shell scripts and output logs, and where files your jobs create will be placed.

You will also be given a folder in the `/scratch/` directory with more storage space. This is not backed up, and so is best used for temporary storage of large quantities of research data.

Once your account is set up, connect to the UCT VPN (if you are off campus) and type the following command to ssh into your HPC account:

`ssh abcdef001@hpc.uct.ac.za`

Type in the password you have been provided by the administrator, and there you go! You are logged in to the UCT HPC.

### Example Slurm Script

Let's assume you have a python file that you want to run named `python_script.py`. How does one go about submitting this as a job?

You should see a very similar example script to the one shown below in your home directory already. Let's go through it and talk about what each line means.

```
#! /bin/sh
#SBATCH --account eleceng
#SBATCH --partition=ada
#SBATCH --time=10:00:00
#SBATCH --nodes=1 --ntasks=4
#SBATCH --job-name="yourjob"
#SBATCH --mail-user=abcdef001@myuct.ac.za

module load software/module1

python3 python_script.py
```

1.  #! /bin/sh
    You have probably seen this before. It simply specifies what shell to use when processing the commands written in the rest of the file. *sh* is the default shell for most Linux machines.
2.  #SBATCH --account eleceng
    The #SBATCH commands give the Slurm processor some information about what resources you are demanding, and whether you are allowed them! The --account parameter specifies what user group you belong to. Your administrator should tell you this.
3.  #SBATCH --partition=ada
    This is the partition of the HPC you are accessing. Most accounts are set up on *ada* currently.
4.  #SBATCH --time=10:00:00
    This is called the *walltime* of your job. It is an upper limit for how long your job can run before it is kicked off the processing pipeline.
5.  #SBATCH --nodes=1 --ntasks=4
    These parameters specify how many nodes are needed to run your job and how many parallel tasks may run respectively. Unless you have very specific needs, you can leave these as default.
6.  #SBATCH --job-name="yourjob"
    As you may expect, this is simply a name for your job that will show up in the queue. This is public so try and pick something appropriate.
7.  #SBATCH --mail-user=abcdef001@myuct.ac.za
    The administrators will want your email attached to your job so that they may contact you if anything goes wrong.
8.  module load software/module1
    This line will load one of the many premade software package *modules* on the cluster. We will talk more about this later, but essentially it is a software environment that contains common dependencies you may need.
9.  python3 python_script.py
    Finally, we run the python file as per normal. Note that this `python3` points to the python installation within your loaded module, not your local installation if you have one.


### Submitting and Tracking Jobs

The lines we talked about above should all be saved inside a .sh file, named something like **batch_script.sh**. Then, to submit your job to the queue, you may type:

`sbatch batch_script.sh`

This should give you a confirmation message in the terminal along with a **job ID number**. You should save this number.

Let's say your job ID is 1110001. As your job is running, you can type:

`squeue 1110001`

to check the status of your job. It will show you info such as runtime so far, status (pending, running, completed etc.), and other related information to help you keep track of your job.

If you simply want to test out a few commands or a quick-running script, you may wish to sidestep this whole process of writing a .sh file and submitting the job to the queue. To start an interactive job, type:

`sintx`

This will allocate you a compute node where you can execute commands directly. You can still load modules normally with `module load`.

## Modules

Now, one of the main issues you can run into when using the HPC is setting up your dependencies. This can be a blockage because your /home/ directory is only allocated **20 Gb** of storage space. Often, this is not enough.

The way to circumvent this limitation is to choose a pre-existing module that has as many of your common dependencies already installed as possible. You may view the premade modules by typing:

`module avail`

There are quite a few! Most have descriptive names relating to what's in them. Once you find one that has a dependency you need, you may use it by typing the command:

`module load module_name`

We discussed this a bit before, but to expand on what this does: loading a module builds and runs a *Singularity container*. It's basically Docker for HPCs. It will build and store all dependencies in a single image, upon which you can run your own code. Said dependencies will then be available for inclusion in your program.

If you need an image created for you, and are having trouble installing your dependencies locally, you can get in touch with one of the administrators and ask them to install one for you.

## References and Further Reading

[Iowa State University Slurm and HPC Guide](https://www.hpc.iastate.edu/guides/introduction-to-hpc-clusters)
