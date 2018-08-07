Corresponding implementations for the ICRA 2018 paper " " by Sven Beck, Mark Pfeiffer, Roland Siegwart and Cesar Cadena.

<table>
  <tr>
     <td>Pedestrian Exit Interaction</td>
     <td>Pedestrian Pedestrian Interaction</td>
  </tr>
  <tr>
    <td>
       <img height="400px" src="https://github.com/sbeck2706/ensemble/blob/master/ped-exit-interaction.png">
    </td>
    <td>
       <img height="400px" src="https://github.com/sbeck2706/ensemble/blob/master/ped-ped-interaction.png">
    </td>
  </tr>
</table>

# Installation

Requires python 3

0. *optional but recommended steps: set up a virtual environment as follows*
Set up a new virtual environment: `virtualenv --python=python3 <virtualenv-name>` and activate it `source <virtualenv-name>/bin/activate`.

1. clone the software into a new directory `git clone  git@github.com:ethz-asl/DIRECTORY` and change into it `cd DIRECTORY`.

2. Install all requirements and the repository software. `pip install -r requirements.txt` and `pip install .`

3. Dependent on your hardware install tensorflow for CPU (`pip install tensorflow`) or GPU (`pip install tensorflow-gpu`).


# Usage

## Reproducing Results

make test-experiments --TEST_DATA_PATH=<INPUT DATA PATH> --SAVE_FIGURES_PATH=<INPUT PATH WHERE FIGURES ARE SAVED>


# Library



## Models

```


## Data


## Experiments
