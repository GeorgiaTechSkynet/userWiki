> By the time SkyNet became self-aware it had spread into millions of computer servers all across the planet. Ordinary computers in office buildings, dorm rooms, everywhere. It was software, in cyberspace. There was no system core. It could not be shut down. The attack began at 6:18 P.M. just as he said it would. Judgment Day. The day the machines could finally recognize pictures of cats. 

This document is an introduction to our SkyNet cluster with some pointers to more general introductory material where useful.

# Skynet Cluster: Background and Philosophy

SkyNet is a GPU-intensive cluster shared between the labs listed below. The goal is to pool resources (money, time, attention) to create and maintain the best compute cluster in academia for modern AI/ML research (including sub-fields like vision, NLP, robot learning). 

Our operating hypothesis is that fragmentation is bad. Becoming the top AI/ML group in academia requires having access to the best compute cluster. You simply can not execute ambitious large-scale projects with a ragtag assembly of 1 GPU in this student's desktop and 2 GPU in that student's desktop and 1 AWS instance spun-up for a deadline -- we believe those resources are better utilized and leveraged if pooled. 

All nodes and storage units in SkyNet are all centrally managed so individual labs/students don’t have to worry about maintaining individual nodes. PIs contribute compute nodes, storage space, and admins proportional to the size of their group (basically a multiplicative factor of number of skynet users they expect to onboard, e.g. 4-8 GPUs/student and 10-20TB/student). 

The SkyNet philosophy is that members own GPU-time and storage (as opposed to viewing it as a union of sub-clusters corresponding to the different labs). SkyNet runs SLURM, and each lab gets a dedicated job queue with capacity = their contributed number of GPUs. So you always get what you pay for. 

In addition, there is a general ("overcap") queue where any participating member can submit jobs and use any number of free currently-unutilized GPUs; hence, the name "overcap" because you can go over your cap. However, these jobs may be terminated when someone with priority shows up. Thus, you should create preemptable jobs, where the termination signal can be caught to save checkpoints. An example of this is provided below. 

This arrangement provides the value in pooling -- labs are guaranteed their GPUs, with potential access to a much larger resource when it’s free. 

CoC TSO does most of the initial setup. Then a team of RS/postdocs + students (with appropriate representation from different labs) take over. We have standard scripts that makes sure all nodes have the same software installed. If someone wants a particular software installed, they just ask their lab admin representative to talk to the other admins to make sure it doesn’t break things.


### Labs

| **Lab**                         | **Lab Admins**                  | **Slurm Account**    |
|----------------------------------|---------------------------------|----------------------|
| Irfan Essa's Lab                 | Nikolai Warner                  | essa-lab             |
| James Hays' Lab                  | Jim James                       | hays-lab             |
| Zsolt Kira's Lab                 | Ram Ramrakhya                   | kira-lab             |
| Judy Hoffman's Lab               | Simar Kareer                    | hoffman-lab          |
| Wei Xu & Alan Ritter's Lab       | Junmo Kang                      | nlprx-lab            |
| Sehoon Ha's Lab                  | Maksim Sorokin                  | ha-lab               |
| Mark Riedl's Lab                 | Glenn Matlin                    | ei-lab               |
| Sonia Chernova's Lab             | Kartik Ramachandruni            | rail-lab             |
| James Rehg's Lab                 | Max Xu                          | rehg-lab             |
| Chris MacLellan's Lab            | Lane Lawley                     | tail-lab             |
| Danfei Xu's Lab                  | Shuo Cheng                      | rl2-lab              |
| Harish Ravichandar's Lab         | Jiazhen Liu                     | ravichandar-lab      |
| Anqi Wu's Lab                    | Chengrui Li                     | wu-lab               |


# SkyNet Machines

\<computer_name\>.cc.gatech.edu

SkyNet consists of 3 types of nodes: 

1. Compute nodes 
  - Configurations vary, but typically a pre-assembled 28-core, 384GB RAM, 8-GPU compatible unit (GPUs need to be purchased separately) with on-site maintenance. 
  - SkyNet has a mix of Titan X, Titan Xp, RTX 2080 Ti, RTX 6000's, and A40's. We find V100 to not be worth the price. 
  - Current Count: 284 GPUs, 2056 CPUs
2. Head nodes -- {sky1, sky2}
  - For logging in and submitting jobs via SLURM
  - NOT for running compute intensive things
  - Current count: 2
3. Fileservers and RAID storage units
  - Configurations vary, but typically 160-360TB storage each. 
  - All compute nodes mount the same shared space (home directories for users, shared space for datasets, etc) from file-servers that talk to RAID boxes (which are backed up). 
  - Current count: 3 RAID units (spinning disk), 1 SSD RAID (to be delivered). 

Compute Machines: 

For a complete list of nodes see this [spreadsheet](https://bit.ly/2UcJReC)
(CAREFUL: don’t accidentally edit)

Infrastructure Machines:

- sky1 - Head Node. Log in to this machine to run slurm jobs and for any long term non-compute-intensive processes (tmux, jupyter notebook, visualization server, etc.).
- multivac - File server with 40 cores, 125G RAM, and 180T storage.
- minivac (by Irfan) - File server with 256GB RAM and 360TB storage. IceBreaker 4936 - 360TB RAW
- microvac - Read optimized file server for shared datasets with 40 cores, 256GB RAM and 160TB storage.
- galactic-ac (by Judy and Zsolt) - File server
- cosmic-ac (by Wei and Alan) - File server
- universal-ac (by Dhruv and Devi) - NVMe file server
- planetary-ac (by Sehoon Ha) - NVMe file server
- optimus - Web hosting (including Mechanical Turk)
- marvin - Slurm controller
- vicki - Backup slurm controller (also a compute node)


# How to Get Access

A few things need to happen to add users to skynet. As a new user you should make sure these things happen.

1. Email the College of Computing's IT Group, TSO (helpdesk@cc.gatech.edu) with your Buzzport login user id, indicate that you're a new student in our lab and ask for an access to the skynet cluster.
    * After they respond you should be able to log in to all of the above machines over ssh using your existing GT user id (for example, pburdell3) and password (NOT your CoC account, which you can get [here](https://support.cc.gatech.edu/resources/forms/coc-account-request-form)). If you're not familiar with ssh, [here's](https://chrisjean.com/ssh-tutorial-for-ubuntu-linux/) a quick tutorial.
    * Create an ssh-key and copy it onto the cluster (this will enable you to login without typing a password).  If you don't already have an ssh-key, create one with `ssh-keygen -t rsa -b 4096 -o -a 100`.  As long as you are on a personal machine, you can leave the key passphrase blank.  Then copy it to the cluster with `ssh-copy-id <gt_user_id>@sky1.cc.gatech.edu`.  Now you should be able to ssh without a password!
2. Contact your lab admin (see [above](http://optimus.cc.gatech.edu/wiki/skynet#labs)) to be added to the #skynet-users slack channel and the appropriate slurm lab account.
    * The slack channel is for discussing general skynet issues like status updates, performance issues, user notifications, and sometimes skynet usage policy changes.
3. Read this wiki page to understand how to use skynet appropriately. Note that you MUST use SLURM. ([this page](http://optimus.cc.gatech.edu/wiki/skynet))
4. Add your name, username, and lab account association (see [below](http://optimus.cc.gatech.edu/wiki/skynet#extras_labs)) to the [user spreadsheet](https://docs.google.com/spreadsheets/d/1HJQJd6Vpc3_iGcstge1JklQL1mLj0ht9JsbOgu8ROnA/edit?usp=sharing).

# Quickstart guide to skynet
If you're a new skynet user, this is the section for you!  Here we'll cover getting connected and running your first jobs, with templates / examples for you to follow.

## Getting Connected to Skynet

First, on your **local machine** let’s set up some aliases

Run `vim ~/.ssh/config` and paste the following text (make sure to fill the placeholders <USERNAME> and <LAB NAME>

```bash
Host sky2
    Hostname sky2.cc.gatech.edu
    User <USERNAME>
Host sky1
    Hostname sky1.cc.gatech.edu
    User <USERNAME>
    SetEnv SKY_VSC_LAUNCH="--gpus a40:1 --cpus-per-task 15 --partition <LAB NAME>"
    ForwardX11Trusted yes
    ForwardAgent yes
```

Now open up your vscode (you might need to restart), and in the left hand bar select "Remote Explorer".  You can now click `sky1` or `sky2` to connect!

When you connect to sky1 or sky2, it's important to note that your vscode session is not attached to a gpu.  In some cases, like when you want to use a vscode jupyter notebook with a gpu attached, make sure to use this [tool](https://github.com/apjez/vscode-slurm-launcher)

## How does storage work?

DO NOT put much in your home directory. This only has 30GB storage, and if it fills up completely your account might freeze.  Below we make a symlinked directory called `flash` which points to your lab's storage.  Make sure to look up your lab stroage detailed below!

```bash
mkdir <lab storage>/<gt username>
ln -s <lab storage>/<gt username> ~/flash
```

For instance I run

```bash
mkdir /srv/hoffman-lab/flash9/skareer6
ln -s /srv/hoffman-lab/flash9/skareer6 ~/flash
```

## Moving VSCode Installation [Optional]

By default VSCode remote server will install itself on your skynet home directory.  This has limited storage and is quite slow.  Moving it to flash can help!  Set the following vscode setting on your local machine (click Code>Settings)
```
"remote.SSH.serverInstallPath": {
  "sky": "your storage directory on flash ie /coc/flash<x>/yourusername",
}
```


## Now how do I run jobs?

### Critical Alias Setup

SLURM is a job allocation system which allows everyone to share GPUs on skynet.  There are a ton of different ways to launch jobs in SLURM, and this can be overwhelming.  These commands are long and hard to remember, so people often create aliases and tools to make things easier.  Here are the key tools that I use (credits to Naoki Yokoyama)

See [git repo](https://github.com/naokiyokoyama/my_env) and follow the readme instructions.  These aliases will be used in the next sections so set that up before continuing.

### Preinstalled tools
The following are preinstalled onto skynet (just type these commands anywhere)

* User breakdown: `gpu_usage -u`
* Users in specific lab: `gpu_usage -u -p <lab-name>`
* Lab breakdown: `gpu_usage -l`

### Sbatch

Batch jobs run in the background, and put their output in log files for you to read later (see`—-output` and `--error` below).  If you launch 10 batch jobs all at once, each will run in parallel, assuming that our lab’s GPU quota hasn’t been met.  This is great once your code works and you want to run a few jobs.  For debugging see the Interactive Jobs section below

First you need to save an sbatch script.  Here’s a template, where I've included a number of common `#SBATCH` options.  Say this file is called job.sh

```bash

#!/bin/bash
#SBATCH --job-name=<job_name>
#SBATCH --output=<jobname>.out
#SBATCH --error=<jobname>.err
#SBATCH --partition="<lab name here>"
#SBATCH --nodes=1
#SBATCH --ntasks-per-node=1
#SBATCH --cpus-per-task=7
#SBATCH --gpus-per-node="rtx_6000:1"
#SBATCH --qos="short"
#SBATCH --exclude="clippy"
#SBATCH --mem-per-gpu="<quota>"

export PYTHONUNBUFFERED=TRUE
source ~/.bashrc
conda activate <your env>
cd <root dir of your codebase>

srun -u python -u <your script.py here>
```

You can now run this with `sbatch job.sh`

Now say you want to change some hyperparameter and run many jobs.  You can change the last line of the template to 

```bash
srun -u python -u <your script.py here> lr=${lr} gamma=${gamma}
```

And run `sbatch --export=lr=1e-4,gamma=1 job.sh`.  This will fill in all the variables you exported (lr and gamma).

### Interactive Jobs

The easiest way to run an interactive job is to simply run `sbash` once connected to sky1 or sky2, which we set up [before](https://github.com/naokiyokoyama/my_env).  This will allocate you a single gpu to mess around and debug with.  There are many options to this command which you can see in `/mv_env/slurm/slurm_interactive_bash.py`.  
Common use cases:

* Specific GPU: `sbash -g <gpu type here>`.  If you need extra vram, consider `sbash -g a40` or `sbash -g rtx_6000`
* Partition: `sbash -p <partition>`.  For very quick debugging run `sbash -p debug`, which will expire in 4 hours, but will allocate quickly.

### Overcap

Sometimes your lab may reach their gpu usage cap, which you can monitor via this [utility](https://github.com/batra-mlp-lab/slurm_usage_utils).  When your lab is at capacity, you can still launch more jobs in `overcap`!  Note that these jobs can be preempted by other users with higher priority.  All you need to do is set **both** `partition` and `account` to overcap

For sbatch jobs
```bash
#SBATCH --partition=overcap
#SBATCH --account=overcap
```

For an interactive job run `sbash -p overcap -A overcap`

### Job Preemption Auto Resume

Jobs in overcap can be preempted by other users.  But without much work you can get your jobs to automatically resume.  Erik Wijmans has a great example [here](https://github.com/erikwijmans/skynet-overcap-example).  Just import `interruptible_utils.py` into your main training loop.  Then implement the checkpoint logic for your specific codebase!

### Some more nuanced things to know about sbatch jobs

1. A40 nodes have 128 logical cores whereas most other nodes have 56. This means you can use 15 and 7 logical cores per GPU you request respectively.
2. Depending on your codebase you can either have slurm spawn your jobs, or you can spawn them yourself with something like `mp.spawn()`
    1. Slurm: if you want slurm to spawn your jobs, you’ll use the flag `-ntasks=<ngpus>` when using sbatch / srun. If you set `-cpus-per-task=c`, you’ll get `c*ngpus` total cpus. If you’re using an A40 node you should set `-cpus-per-task=15`, and for any other nodes set `-cpus-per-task=7` (regardless of how many gpus you request).
    2. Manual `mp.spawn` : in this case since you manually spawn your jobs, you will ignore `-ntasks` which defaults to 1. You’ll also set `-cpus-per-task=<ngpus * 15>` for an A40 node or `<ngpus * 7>` for another node
3. To check whether you’ve requested the right number of CPUS, you can use the command `squeue -o"%.7i %.9P %.8j %.8u %.2t %.10M %.6D %C"`.

# Expectations and Advice

We expect everyone who uses these machines to be aware of the resources available and use them wisely. In the skynet cluster, the most important resources tend to be storage and GPUs, so we have a few rules around these things.


## Head Node

Generally, **log in to the head node** (identified above) to do any work. Launch slurm jobs, tmux sessions, visualization servers, vim session, etc. on this node. Nobody is prohibited from logging in to other nodes in the cluster, but any jobs (including ssh, tmux, and vim) launched on other machines without using slurm might be killed at any time. This makes maintenance easier because it means most nodes can be rebooted as long as they don't have any slurm jobs running on them.

Anything that can run on compute nodes should be (see the [Slurm](http://optimus.cc.gatech.edu/wiki/preview#expectations-and-advice_software_slurm) section). This includes unzipping tarballs, running a data preprocessing script (even single core one), etc.  The head node is everyone's interface to the cluster and running resource intensive processes degrades everyone's experience. As the cluster continues to grow in size, the headnode simply doesn't have the resources to let everyone "cheat" a little bit here. If every active users of the cluster continuously used just a single CPU core on the headnode, it would be completely overwhelmed.


## Storage

You only need to log in to one of the computing clusters - your home directory is sharing space across the computing and infrastructure machines. When you first log in, you'll notice your home directory is quite small (30G). We'd like everyone to keep large files (data) outside their home directory in their labs specific storage location(s). That way we can prioritize data for backups and recovery. See [here](https://docs.google.com/document/d/1JrksYv1Lv1WuvwOs_FXqnnm9q0CJkVcndvyLqHPc3Xw/edit?usp=sharing) for more context on why storage was switched from shared to lab specific.

### Project Spaces

* cvmlp-lab
  * `/srv/cvmlp-lab/share` (50T) `multivac` fileserver
  * `/srv/cvmlp-lab/share2` (30T) `multivac` fileserver
  * `/srv/cvmlp-lab/scratch` (60T) `multivac` fileserver
  * `/srv/cvmlp-lab/flash1` (79T) `universal-ac` NVMe fileserver

* essa-lab
  * `/srv/essa-lab/share3` (27T) `microvac` fileserver 
  * `/srv/essa-lab/flash3` (97T) `univac` NVMe fileserver

* hays-lab
  * `/srv/hays-lab/share` (50T) `multivac` fileserver
  * `/srv/hays-lab/share2` (30T) `multivac` fileserver
  * `/srv/hays-lab/scratch` (60T) `multivac` fileserver

* kira-lab
  * `/srv/kira-lab/share4` (194T) `galactic-ac` fileserver

* hoffman-lab
  * `/srv/hoffman-lab/share4` (194T) `galactic-ac` fileserver
  * `/srv/hoffman-lab/flash9` (55T) `hflash1` NVMe fileserver

* nlprx-lab
  * `/srv/nlprx-lab/share5` (70T) `cosmic-ac` fileserver
  * `/srv/nlprx-lab/share6` (127T) `koto` fileserver

* ha-lab
  * `/srv/ha-lab/flash2` (79T) `planetary-ac` NVMe fileserver

* tail-lab
  * `/srv/tail-lab/flash10` (18T) `nostalgia` NVMe fileserver

* rl2-lab
  * `/srv/rl2-lab/flash7` (35T) `horizon` NVMe fileserver
  * `/srv/rl2-lab/flash8` (35T) `horizon` NVMe fileserver

* wu-lab
  * `/srv/wu-lab/flash12`


**When you first log in**, create a directory for your user at (at least) one of your lab's project spaces -- `/srv/<lab-name>/<location>/<username>` to store your files. Do NOT create additional folders.

In a typical use scenario you can symlink all data and checkpoints to sub-directory of your project or scratch space created for that purpose (use [ln -s](https://www.cyberciti.biz/faq/creating-soft-link-or-symbolic-link/)).

**Transferring files to/from the cluster**. When transferring files to/from the cluster, it is preferred to use `rsync` instead of `scp`.  `rsync` is faster and puts less load on the servers.  `rsync -ah` can be used as a drop-in replacement for `scp` and `--info=progress2` (or `-P` if `--info=progress2` is not available) will give you a nice progress indicator!

**Note.** You have login to the fileservers as, in some very particular cases, it can be useful to access the raid arrays directly (you should not do so unless you are absolutely certain that it makes sense to do so -- ping your lab admin if you are unsure).  If you do, note that your normal home directory does **not** mount on the fileservers and therefore you should **not** put anything in your home directory on the fileservers.  Further note that, commands like `pip`, `npm`, etc will inadvertently add things to your home directory (i.e. they cache their downloads).

### Datasets [[DEPRECATED]]

Datasets is largely deprecated and bellow is just for historical purposes. Please store datasets in accordance with your lab's policy for storage.

* `microvac` fileserver 
  * `/srv/share/datasets` (25T)

Datasets like COCO, VQA, CIFAR, etc. are kept here to avoid unnecessary duplication.  You are welcome and encouraged to add new datasets here.  When adding datasets, please remove the zip file/tarball after decompressing as these will needless take a very large amount of disk space otherwise.

### Checking your usage

Each directory in the project spaces, scratch space, and datasets has a `disk-usage.txt` file in it that shows how much disk space that directory is using.  Example output:

```
> cat /srv/share/ewijmans3/disk-usage.txt
Updated: 2020-11-21 15:04
Usage (human-readable):  127.2G
Usage (K): 133284169
```

This file is updated weekly.  (Note that when you create a new directory, this file won't exist until the script runs for the first time).

We don't have a hard limit on space as cutting edge AI research often uses large amounts of disk space.  However, we may tag you in #skynet-users when one of your directories gets large and please cleanup your project spaces proactively -- you hoarding files that will likely never be read again may block someone from starting a new project that demands a large amount of disk space.

### Checking free space

Before you write a lot of data, please check to make sure there is enough space on that partition.  You can see free space via
```
> df -h | grep -E "coc|F"
Filesystem                                            Size  Used Avail Use% Mounted on
multivac.cc.gatech.edu:/pskynet1                       50T   41T  8.7T  83% /coc/pskynet1
multivac.cc.gatech.edu:/pskynet2                       30T   22T  8.2T  73% /coc/pskynet2
microvac.cc.gatech.edu:/zfs/microvac-store3/pskynet3   27T   21T  5.5T  80% /coc/pskynet3
multivac.cc.gatech.edu:/export/scratch                 59T   58T  1.5T  98% /coc/scratch
microvac.cc.gatech.edu:/zfs/microvac-store2/dataset    24T   23T  1.2T  95% /coc/dataset
galactic-ac.cc.gatech.edu:/zfs/data                   194T     0  194T   0% /coc/pskynet4
cosmic-ac.cc.gatech.edu:/zfs/cosmic-ac/project         70T     0   70T   0% /coc/pskynet5
```

(You can also just do `df -h | grep coc` if you already have the header memorized)

The "mounted on" name is what you need to look at to figure out free space.  In general, they are pretty easy to map back to `/srv/`, i.e. `/coc/psknet1` is `/srv/share1`, `/coc/dataset` is `/srv/datasets`, etc.  You can always check with
```
> ls -l /srv
total 0
lrwxrwxrwx 1 root root 12 Apr 30  2019 datasets -> /coc/dataset
lrwxrwxrwx 1 root root 12 Apr 30  2019 scratch -> /coc/scratch
lrwxrwxrwx 1 root root 13 May 18  2017 share -> /coc/pskynet1
lrwxrwxrwx 1 root root 13 Nov 12  2018 share2 -> /coc/pskynet2
lrwxrwxrwx 1 root root 13 Jul  1  2019 share3 -> /coc/pskynet3
lrwxrwxrwx 1 root root 13 Nov 19 18:09 share4 -> /coc/pskynet4
lrwxrwxrwx 1 root root 13 Nov 19 18:10 share5 -> /coc/pskynet5
lrwxrwxrwx 1 root root 14 Feb 11  2020 testing -> /zfs/testshare
```


## Software

Software we use tends to change quickly and it's easy to install, so we've found it not all that useful to maintain central installations of popular libraries. You should install your own versions of [miniconda](https://docs.conda.io/en/latest/miniconda.html)/torch/tensorflow in your user's directory (e.g., with virtualenv).

*Note:* anaconda is huge, unless you know you'll need everything in it, the approach of installing [miniconda](https://docs.conda.io/en/latest/miniconda.html) and then installing just what you need is much quicker.

DO USE VERSION CONTROL. git makes code easy to maintain and share. Also, git != git hosting, but if you need hosting, you can get free private git repositories through bitbucket.org and soon we'll have an internally hosted instance of gitlab, where everyone would be expected to push code to and would be the primary means of sharing code internally.

### VSCode

VSCode has a set of very nice remote development tools, however, VSCode's remote development tools seemingly have a philosophy of "the remote machine can be rebooted if I leak too many processes" and "the remote machine is all for me", neither of which are true for a shared machine. A common culprit is `rg`, the tool VSCode uses for search. Please follow in the instructions [here](https://github.com/microsoft/vscode/wiki/Search-Issues#slow-search-rg-running-for-a-long-time-or-consuming-lots-of-cpumemory) to make sure it doesn't end up searching things it shouldn't, like all of your training data.
Also be careful with VSCode's jupyer integration as it seemingly never shuts down the notebooks it starts.

You can check to see how many processes VSCode has started for your username with `ps x --User $(whoami) -o "pid,cmd" | grep vscode-server | grep -v -E "\sgrep\s"` and kill them with `kill $(ps x --User $(whoami) -o "pid,cmd" | grep vscode-server | grep -v -E "\sgrep\s" | awk {'print$1'})`.

Do not run anything through VSCode's terminal that you care if it gets killed. Due to VSCode's love of leaking processes we often have to just kill all `vscode-server` processes.

### PyTorch

Most people on skynet use PyTorch. PyTorch has one anti-pattern with slurm clusters however. The [`"file_system"`](https://pytorch.org/docs/stable/multiprocessing.html#file-system-file-system) strategy ends up leaking shared memory as the workaround it uses to stop that from leaking doesn't work with slurm. Leaked shared memory is pretty catastrophic as it requires rebooting the node to reclaim it. Do not use this. Ever. 

Use the default [`"file_descriptor"`](https://pytorch.org/docs/stable/multiprocessing.html#file-system-file-system) mode. If you are using code you didn't write, please check it for changing the mode. If you get an error that you ran out of file descriptors, don't change to file_system. If you are sure you aren't just leaking memory, you can raise your descriptor limit to `2^16` with `ulimit -n 65536`. If that isn't high enough, talk to your lab admin.

### Slurm

DO USE SLURM (NOT OPTIONAL)!  Any process you run on a compute node should be through Slurm. If you don't use slurm then GPU usage conflicts might emerge and cause unexpected job failures, or your process may be killed as compute nodes with no running Slurm jobs will be rebooted as needed and without warning.

* [Here's](https://drive.google.com/file/d/0B70NAN5i4ZHSWktDQjNaWWRMdFk/view?resourcekey=0-ZMNo1ie4D0TwbRAmFsZF8A) Stefan's Slurm guide (written when at VT). The same commands should work on Skynet with one minor difference: specifying the GPU type is done via constraints and we are on a more recent version of slurm that directly supports gpus (note that using `--gres=gpu:2` still works), i.e. instead of `--gres=gpu:titan_x:2` to request 2 TitanXPs, you would instead use `--constraint=titan_x --gpus-per-node=2` and `--constraint=2080_ti --gpus-per-node=2` to request 2 2080 Tis.  Slurm constraints work like booleans, so `--constraint="2080_ti|rtx_6000"` would request 2080 Tis and/or RTX 6000's.   One other useful flag is `--gpus-per-task` that specifies the number of GPUs to use per slurm task.
    * In addition to managing GPUs, we also use SLURM to manage CPUs. 
    * This means that when submitting a job, you will also need to specify how many CPUs you want to use (you will get 1 by default).  On average, our nodes have 56/8=7 CPUs per GPU, so our golden ratio for jobs is 6 CPUs per GPU as this will leave a little bit of room for CPU-only jobs on each node.  Going above 7 CPUs per GPU will lead to CPU starvation where we have free GPUs that can’t be scheduled as there are no free CPUs on that node, so please think very carefully before requesting more than 7 (or even 6) CPUs per GPU.
    * Long running and large cpu-only jobs can also be problematic.  The request here is to see if you can either make them short running large cpu-only jobs or long running small cpu-only jobs as cpu-only jobs will only be problematic when they are both.
    * The most straightforward way to specify the number of cpus is with `--cpus-per-task <num_cpus>` or with the abbreviation `-c <num_cpus>`.  So, if you have a 1 GPU job, add `-c 6` (or lower if you don’t actually need all 6 CPUs!) to the script.
    * Note: For those of you using `--ntasks-per-node=<something>`, the number of CPUs will be multiplied by the number of tasks as the number of CPUs is requested per task. So if you are normally doing 1 task per GPU, just do `-c 6`, not `-c 6*<num_gpus>`.
    * Note:  SLURM manages logical cores, not physical cores, so there are physical cores * 2 CPUs according to SLURM as we have hyper-threading.

* [Here](http://optimus.cc.gatech.edu/wiki/slurm-aliases) are some bash aliases that show how to extract useful information from slurm. It may be a good idea to include them in your `.bashrc` of equivalent configuration.

* Do **not** make calls to any slurm utilities (`squeue`, `sbatch`, `sinfo`, any of the commands in [wiki/slurm-aliases](http://optimus.cc.gatech.edu/wiki/slurm-aliases), etc.) in a tight loop (like `watch squeue`).  These result in a remote procedure call being executed on the slurm controller node, which can then become overwhelmed and will beginning killing jobs.

* Do **not** redefine the `CUDA_VISIBLE_DEVICES` environment variable that slurm sets.

* Steps to run interactive jobs using slurm + tmux (so the process doesn’t end when your ssh connection breaks):

  ```
  ssh <headnode> # i.e. sky1
  tmux new -stest
  srun --gres gpu:1 -p debug -J "test" --pty bash
  python main.py
  ```

* Do **not** start tmux within the interactive job.  The tmux session (and anything running within it) will continue to run after the interactive job ends!

* When running CPU-only jobs, you still need to use SLURM.  If you do not specify any gpus, i.e. don't have `--gres` at all, you will not request nor get a GPU.

**GPU types**

* Titan XP, `--constraint=titan_x`

* RTX 2080 Ti, `--constraint=2080_ti`

* Quadro RTX 6000, `--constraint=rtx_6000`

* A40, `--constraint=a40`

You can see what nodes have what GPUs with `sinfo -o "%f %N"`.

A word on GPU type usage: We currently don't restrict GPU usage by what type of GPU your lab contributed. This has considerable benefits to you. It allows you to access new GPUs before your lab has had a chance to purchase any. It also allows you to scale up your usage of that GPU type beyond your lab's contribution as a deadline approaches. The downside is that if people only ever user the newer GPUs -- newer GPUs tend to be faster and have more VRAM, which makes it more desirable -- then the job wait times will skyrocket. The ask here is to be responsible with what GPU types you are requesting. Full training runs on a model that can really make use of newer GPUs? Go for it. But when debugging, running smaller scale experiments, or running experiments aren't that dependent on GPU throughput, older GPUs are likely fine so consider those.


**Slurm Partitions.**

- ``debug``: This is the highest priority partition, it is for day-to-day development.  The maximum runtime in debug is 4 hours and you can only have 5 GPUs in use at once in debug.
- ``short``: The middle priority partition.  The maximum runtime is 2 days.
- ``long``: The lowest priority partition.  The maximum runtime is 7 days.
- ``user-overcap``:  For labs with per user GPU limits (currently only CVMLP), this partition allows you to exceed those but jobs within it will be preempt-able.  For labs without per user GPU limits, this can still be useful as it lets you use your lab's GPUs but won't lock your labmates out.  The maximum runtime is 2 days.
- ``overcap``:  Allows jobs to exceed a given lab's GPU cap but jobs a preemptable.  See the overcap example bellow for usage.  The maximum runtime is 2 days. *Note:* Requires the job to be run in the overcap slurm account. Add `-A overcap` to your job's slurm args.

Note that slurm allows you to submit to multiple partitions at once, i.e. `short,user-overcap` will have a job first fill up your user GPU cap on short and then will spill over into `user-overcap` as needed.

**Slurm Examples.**

- PyTorch Distributed Data Parallel: [skynet-ddp-slurm-example](https://github.com/erikwijmans/skynet-ddp-slurm-example)

- Overcap (use even more GPUs!) and preemptable jobs: [skynet-overcap-example](https://github.com/erikwijmans/skynet-overcap-example) TLDR: Add `-A overcap` to your job invocation and use the `overcap` partition.

**Slurm Tools.**

- [slurm-aliases](http://optimus.cc.gatech.edu/wiki/slurm-aliases) provides a set of commands built on top of Slurm that provide additional info

[comment]: # (- [gpu-use](http://optimus.cc.gatech.edu/wiki/gpu-use) provides real time information about the processes running on GPUs on the cluster.)

**Lab Specific Info.**

- **CVMLP**:  short/long have a limit of 16 GPUs and 112 CPUs (two 8 GPU nodes) per user.  If you want to go over that limit, use the `user-overcap` partition.  Your jobs in that partition will still preempt overcap and still count against CVMLP's gpu cap, but your labmates will be able to preempt them.  We also have very fast flash storage on `/srv/flash1`.  Keep pretty much everything there, code, data, conda, etc.

### Docker

Docker and slurm do not work well together, so docker should be avoided unless absolute necessary.  When working with docker, you will need to manually restrict the GPUs used within the container (you can find what GPUs slurm allocated in `$NV_GPU`).  The recommended way to do this is adding `--gpus \"device=${NV_GPU}\"` (yes those escaped quotes are necessary due to silliness on docker’s end) to your `docker run` invocation.  Using `CUDA_VISIBLE_DEVICES` will not work to manage GPUs for docker as slurm numbers GPUs in a slightly different way than CUDA does (it uses the minor-id aka the device file number).  Non-docker jobs run inside a cgroup and can only see the GPUs they were allocated, so this isn't an issue there, but docker jobs run outside the cgroup.  `NV_GPU` is populated with the GPU UUIDs given to the job, allowing docker to essentially reconstruct that cgroup and have everything work as expected.

Further, you will also need to restrict the CPUs by adding the command line flag `--cpuset-cpus="$(taskset -c -p $$ | cut -f2 -d ':' | awk '{$1=$1};1')"` to your docker invocation.

Note that old docker containers are purged continuously as otherwise docker will fill up `/` on compute nodes, which causes issues.  This means that your container should be fully buildable from a Dockerfile and you should not store anything important the container itself.

## CUDA

Skynet is currently on CUDA 11.2 drivers, which support CUDA toolkit <= 11.x.  When installing things like PyTorch, TensorFlow, and other software that comes compiled with CUDA, you will need to install one compiled against a CUDA version that is *not* greater than Skynet's current supported version.

We have a lot of A40 GPUs, to use those you will need software compiled for CUDA >= 11.1.

## Personal Visualization and Development Servers

If you want to host an internal web server for dev purposes (e.g., jupyter notebook, visdom, tensorboard, etc.) you'll need a TCP port to host it on. Ports 8000:9000 are reserved for these types of applications. Try a port and use another one if the one you tried is already taken. These ports are open within the cluster, but firewalled to the outside world, so you'll need to use an SSH tunnel (e.g., something like [this](https://coderwall.com/p/ohk6cg/remote-access-to-ipython-notebooks-via-ssh)) to view the server on your laptop.


## Frequently encountered issues

- **Extreme CPU load.** We use CPU management through SLURM to attempt to keep CPU load in check and make sure jobs can happily coexist, however, some libraries and some uses won't interact correctly with this.  The most common cause is using python subprocesses where each subprocess does work in parallel (i.e. data augmentation on images with parallel data-loading).  Extreme CPU load will negatively impact other users and can cause nodes to crash. Do not start as many processes without checking the CPU usage of those processes and be wary of libraries that select the number of processes automatically as they almost always start as many processes as there are cores.

- **SLURM + tmux/screen.** Do not start a tmux/screen session inside a SLURM interactive session.  See the SLURM section for more.

- **Stuck distributed data parallel workers.** PyTorch's utilities for starting DDP jobs (launch and spawn) do not work well with SLURM.  SLURM has its own ability to start multi-processes that both provides additional benefits (each process will be assigned a unique set of CPU cores, and that set of cores is setup well assuming 1 process 1 GPU) and provides much more reliable management of those processes.  See the DDP example for how to use this.

- **Slurm is sluggish/jobs randomly dying**.  This is commonly due to `s*` commands (namely `squeue`) being pulled in a tight loop, namely `watch squeue`.  These commands result in RPCs on the main controller node, and if the controller is overwhelmed with RPCs,  Slurm will become noticeably slower in the best case or will begin killing jobs in the worst case.

- **My home directory is full and `rm` doesn't work**.  Find a file you would like to delete, then do `cat /dev/null > /path/to/that/file`.  This will free up some space and then `rm` will work again.

## Getting Help

Contact your lab admin or the #skynet-users channel on the CVMLP slack. Ask about any skynet specific technical or political problems at these places.

# Conclusion

If you want anything else or have any suggestions or questions, let us know.

# Extras

An old version of this document is available on Socrates [here](http://socrates.io/#B9yNTZl/read).

Information for administrators is available [here](https://github.com/GeorgiaTechSkynet/skynet_ansible).
