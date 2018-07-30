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

Reported Experiments can be reproduced in the following way:
```
python -m experiments.rerun with experiment_id=<insert here> -u
```

For all tables and examples in the paper, there are corresponging jupyter notebooks that show on which experiments they are based upon. You will find all the details behind the experiment using the notebook [Experimental Details](https://nbviewer.jupyter.org/github/ethz-asl/modular_semantic_segmentation/blob/publish/Experimental%20Details.ipynb).

# Library

The package is devided into 3 parts:

- `xview/models`: implementation of methods
- `xview/data`: implementation of data interfaces
- `experiments`: scripts that implement different functionalities for experiment, following the [sacred](https://github.com/IDSIA/sacred) framework.

## Models
Models are implemented following the sklearn interface, while context handling is necessary due to tensorflow semantics:

```
from xview.models.simple_fcn import SimpleFCN as FCN
from xview.data import Synthia
dataset = Synthia
# custom configs for the model
config = {'num_classes': 10,
            'dropout_probability': 0.2}
with FCN(data_description=dataset.get_data_description(), **config) as net:
    # Add data-loading to the graph.
    data = dataset(<config>)
    # Train the network for 10 iterations on data.
    net.fit(data.get_trainset(), 10)
    # Alternatively, load existing weigths.
    net.load_weights(<path to weights checkpoint>)
    net.import_weights(<path to npz file with stored weights>)
    # Now you can use it to produce classifications.
    semantic_map = net.predict({'rgb': <rgb image blob>, 'depth': <depth image blob>})
    evaluation = net.score(data.get_testset())
```

Any implementation of a model should inherit from `base_model.py`, which implement basic sklean interfaces such as `.fit()`, `.score()`, `.predict()` etc. aswell as training procedures and graph building.

Model implementation should be split up into a method that defines a series of tensorflow-ops mapping input to output and a class inheriting from `base_model.py` that handles all the functionality around this such as data piping etc.
In this way, models can make use of the simple network definitions without building the whole model-object.
See `xview/models/simple_fcn.py` for an example.

## Data
Data interfaces return `tf.dataset` objects, which are documented [here](https://www.tensorflow.org/api_docs/python/tf/data/Dataset). Optionally, also numpy blobs can be produced (be careful with memory usage). All data interfaces inherit from `xview/datasets/data_baseclass.py`, which should be consulted for the interface details.

## Experiments
The `experiments/utils.py` contains basic functions to interact with the sacred storage service, i.e. to load data from previous experiments and to store data from the current experiment.
