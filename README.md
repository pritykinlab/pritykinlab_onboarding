# Pritykin Lab Onboarding

Ask Yuri for:
1. keycard access to comp space
2. Slack access
3. Argo access
4. Github access


# Set Up Argo
1. Download iterm (some nice features but not necessary)

## Using Argo
Login. Open iterm. Run `ssh {computing_id}@argo.princeton.edu`
1. If you see something about adding a key, just say yes (this just happens on the first try
2. Password instructions through email

## Using the Terminal
1. Know some unix commands. I would not memorize commands, just do what you can to get the job done. Know stuff about directory navigation everything else is extra: https://www.geeksforgeeks.org/linux-tutorial/

## VSCode
### Downloading VSCode
If you don't know how to create a file in terminal, install VSCode.
1. Opne VSCode
2. Go to the extension icon on the left then search for `Remote - SSH`
3. Download `Remote - SSH`
4. Go to the bottom icon with a computer and some circle on the bottom right
5. Add ssh host (there is plus button next to window)
6. New popup and type in you username and password as done for terminal
7. choose the top option for adding a configuration

### Using VSCode
1. create a new conda environment with `ipykernel`. Example: `conda install numpy; conda install ipykernel`
2. `ipykernel` is required in order for jupyter notebook to see your enivornment
3. Open a jupyter notebook
4. In upper right select environment that was created in step 1.
5. Start coding!


## Set up Miniconda on Argo
Read a little about why you would want to create environments: `https://conda.io/projects/conda/en/latest/user-guide/getting-started.html`

Miniconda is the primary way to download packages into a self-contained environments.

https://docs.anaconda.com/free/miniconda/#quick-command-line-install

Go to command line setup and run these sort of commands (find the newer ones):
```
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
```

Reset terminal:
```
conda create -y -n jupyter
conda activate jupyter
conda install anaconda::jupyter
conda install conda-forge::nb_conda_kernels
```

More generally, if you want to create a new environemnt (and this is the first thing I do on a new project/type of script):
```
conda create -n {name_env}
conda activate {name_env}
conda install pandas # search on google the package you want to install then install it
```

Inside base you can run:
```
conda install conda-forge::mamba
```

Mamba is a faster version of conda. You would then just use commands like `mamba install x`

## Running Jupyter Notebooks
Most work can be done by starting a jupyter notebook on a slurm node using `run_jupyter.slurm` (This is in the github). Make sure to run have a conda environment with jupyter running.
```
conda activate jupyter
sbatch run_jupyter.slurm
```

After the sbatch you will get a file `run_jupyter.out`. Open the file to get a command like this:
```
Command to create ssh tunnel:
ssh -N -f -L 8890:argo-33:8890 dl4257@argo.princeton.edu

Use a Browser on your local machine to go to:
localhost:8890  (prefix w/ https:// if using password)
```

On your terminal without connecting to argo access:
```
ssh -N -f -L 8890:argo-33:8890 dl4257@argo.princeton.edu
```

In your browser then got to `https://localhost:8890`

## Notes about Slurm Jobs

See your slurm jobs:
```
squeue -u dl4257
```

Example headers:
```
#!/bin/bash
#SBATCH --nodes=1                # node count
#SBATCH --ntasks=1               # total number of tasks across all nodes
#SBATCH --cpus-per-task=2        # cpu-cores per task (>1 if multi-threaded tasks)
#SBATCH --mem-per-cpu=8GB         # memory per cpu-core (4G is default)
#SBATCH --time=24:00:00          # total run time limit (HH:MM:SS)
#SBATCH --mail-user={}@princeton.edu
#SBATCH -o main.out
```

# FAQ
You can't connect to a jupyter server because address is already in use:
```
bind [127.0.0.1]:8890: Address already in use
channel_setup_fwd_listener_tcpip: cannot listen to port: 8890
Could not request local forwarding.
```
Answer: 
Rerun `ssh -N -f -L 8890:argo-11:8890 {id}@argo.princeton.edu` after failing to open the url. Or run lsof -ti:8890 | xargs kill
