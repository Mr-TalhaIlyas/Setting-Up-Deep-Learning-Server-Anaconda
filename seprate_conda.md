Here’s how to install the **full Anaconda** distribution onto your 4 TB drive at `/mnt/hdd/sdd`, so that *all* of Anaconda’s packages, environments, and caches live there:

---

## 1. Create a target directory on your HDD

```bash
sudo mkdir -p /mnt/hdd/sdd/Anaconda3
sudo chown $USER:$USER /mnt/hdd/sdd/Anaconda3
```

---

## 2. Download the full Anaconda installer

Go to [https://www.anaconda.com/products/distribution](https://www.anaconda.com/products/distribution) and copy the Linux installer link, or run for the latest Python 3.x version:

```bash
cd /tmp
wget https://repo.anaconda.com/archive/Anaconda3-2025.06-0-Linux-x86_64.sh
```

> *(Replace `2023.11` with the current release if newer.)*

---

## 3. Run the installer in batch mode to your HDD folder

```bash
bash Anaconda3-2023.11-Linux-x86_64.sh \
  -b  \                    # silent, no prompts  
  -p /mnt/hdd/sdd/Anaconda3  # install prefix  
```

This unpacks the entire \~3 GB distribution under `/mnt/hdd/sdd/Anaconda3`.

---

## 4. Point your shell at the new Anaconda

Add the following to the *top* of your `~/.bashrc` (or `~/.zshrc`), **before** any existing Conda lines:

```bash
# Use HDD-resident Anaconda
export ANACONDA_ROOT=/mnt/hdd/sdd/Anaconda3
export PATH="$ANACONDA_ROOT/bin:$PATH"

# Store all Conda envs and packages on the HDD
export CONDA_ENVS_PATH="$ANACONDA_ROOT/envs"
export CONDA_PKGS_DIRS="$ANACONDA_ROOT/pkgs"
```

Then reload:

```bash
source ~/.bashrc
```

If you want the usual `conda activate` support, initialize Conda’s shell hooks:

```bash
conda init
```

---

## 5. Test your install

```bash
which python         # should point to /mnt/hdd/sdd/Anaconda3/bin/python
conda info | head -n 10
```

Create an env to verify that it goes to the HDD path:

```bash
conda create -n test-env python=3.10
conda activate test-env
conda env list       # see test-env under /mnt/hdd/sdd/Anaconda3/envs
```

---

### Why this works

* **`-p`** in the installer writes *all* files under your chosen prefix.
* By exporting **`CONDA_ENVS_PATH`** and **`CONDA_PKGS_DIRS`**, every new environment and package cache stays on the large disk.
* Your NVMe remains uncluttered, while you have the *entire* Anaconda toolkit (conda-forge, JupyterLab, pandas, scikit-learn, etc.) ready to go.

Feel free to tweak installer versions or paths — but these steps will get the full Anaconda distro running entirely off your 4 TB drive.
