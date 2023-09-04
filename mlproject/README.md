### On Juwels Booster
1. Set python to version 3.9. For this load the following modules:
```
ml --force purge
ml use $OTHERSTAGES
ml Stages/2022
ml GCCcore/.11.2.0
ml Python/3.9.6
```

2. Create a virtual environment and activate it:
```
python -m venv <venv-name>
source <venv-name>/bin/activate
```

3. Clone this Git repository and checkout the `main` branch

```
git clone https://github.com/saragrau4/maelstrom-radiation.git
git checkout main
```

3. Install ap4 dependencies with pip. 
```
pip install -r requirements_wo_modules.txt
```

4. Add the following code at the end of the virtual enviroment activate file `<venv-name>/bin/activate`:
```
BASE_DIR="<absolute path to ens10 directory>"

# expand PYTHONPATH
export PYTHONPATH=${BASE_DIR}/baselines:$PYTHONPATH

# add environment variables
export HDF5_USE_FILE_LOCKING=FALSE
export LC_CTYPE=en_US.UTF-8
export LC_ALL=en_US.UTF-8
```

### Preprocess ENS10 dataset

In case you need to preprocess the ENS10 dataset follow these steps:

1. Load the following modules and activate the virtual environment you created
```commandline
module load Stages/2022  GCCcore/.11.2.0 Python/3.9.6 CUDA/11.5 numactl/2.0.14 cuDNN/8.3.1.22-CUDA-11.5 ecCodes/2.22.1 parallel/20210722 GCC/11.2.0 OpenMPI/4.1.1 CDO/2.0.2;
source <b>/path/to/&lt;venv-name&gt;</b>/bin/activate;
```
2. Download the ENS10 dataset from this [link](http://spclstorage.inf.ethz.ch/projects/deep-weather/ENS10/) including the EFI files:
3. Preprocess the data by executing the script `ens_preprocessing.sh` and subsequently `reanalysis_preprocessing.sh`. Both scripts can be found in the folder `ens10/baselines/utils`. Ensure that when you run the scripts, your current working directory is the one containing the ENS10 data. If it's not, adjust the environment variables `ENS10_FOLDER`, `OUTPUT_FOLDER`, and `ERA5_FOLDER` accordingly.

### On the Mantik platform

Set up a project in Mantik to enable the execution of your experiment. For a step-by-step guide, refer to the quickstart tutorial available [here](https://mantik-ai.gitlab.io/mantik/ui/quickstart.html)

### On your local machine

1. Set `PreRunCommand` in `unicore-config-venv.yaml` to the path of your virtual enviroment

<pre><code>   PreRunCommand:
    Command: > 
      module load Stages/2022  GCCcore/.11.2.0 Python/3.9.6 CUDA/11.5 numactl/2.0.14 cuDNN/8.3.1.22-CUDA-11.5 ecCodes/2.22.1 parallel/20210722 GCC/11.2.0 OpenMPI/4.1.1 CDO/2.0.2;
      source <b>/path/to/&lt;venv-name&gt;</b>/bin/activate;
</code></pre>

2. Run your experiment with mantik
```
mantik runs submit <absolute path to maelstrom-radiation/mlproject directory> --backend-config unicore-config-venv.yaml --entry-point main --experiment-id <experiment ID> --run-name <run name> -v
```
> **_NOTE:_** When you run your application, please ensure that the local copy of the 'ens10' repository on 'Juwels Booster' includes all the latest changes.