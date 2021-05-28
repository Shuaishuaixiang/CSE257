# Generalized Hindsight for Reinforcement Learning


## Installation

1. Install and use the included Ananconda environment
```
$ conda env create -f environment/[linux-cpu|linux-gpu|mac]-env.yml
$ source activate rlkit
(rlkit) $ python examples/ddpg.py
```
Choose the appropriate `.yml` file for your system.
These Anaconda environments use MuJoCo 1.5 and gym 0.10.5.
You'll need to [get your own MuJoCo key](https://www.roboti.us/license.html) if you want to use MuJoCo.

2. Add this repo directory to your `PYTHONPATH` environment variable or simply
run:
```
pip install -e .
```

3. (Optional) Copy `conf.py` to `conf_private.py` and edit to override defaults:
```
cp rlkit/launchers/conf.py rlkit/launchers/conf_private.py
```

DISCLAIMER: the mac environment has only been tested without a GPU.

For an even more portable solution, try using the docker image provided in `environment/docker`.
The Anaconda env should be enough, but this docker image addresses some of the rendering issues that may arise when using MuJoCo 1.5 and GPUs.
The docker image supports GPU, but it should work without a GPU.
To use a GPU with the image, you need to have [nvidia-docker installed](https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0)).

## Example of training a policy
No relabeling:
```
python launch_gher.py --epochs 1000 --env pointmass2
```

Random relabeling:
```
python launch_gher.py --epochs 1000 --relabel --n_sampled_latents 1 --env pointmass2
```

Advantage relabeling:
```
python launch_gher.py --epochs 1000 --relabel --n_sampled_latents 100 --use_advantages --env pointmass2
```


AIR:
```
python launch_gher.py --epochs 1000 --relabel --n_sampled_latents 100 --use_advantages --env pointmass2 --cache --irl
```



## Visualizing a policy and seeing results
During training, the results will be saved to a file called under
```
LOCAL_LOG_DIR/<exp_prefix>/<foldername>
```
 - `LOCAL_LOG_DIR` is the directory set by `rlkit.launchers.config.LOCAL_LOG_DIR`. Default name is 'output'.
 - `<exp_prefix>` is given either to `setup_logger`.
 - `<foldername>` is auto-generated and based off of `exp_prefix`.
 - inside this folder, you should see a file called `params.pkl`. To visualize a policy, run

```
(rlkit) $ python scripts/run_multitask_policy.py LOCAL_LOG_DIR/<exp_prefix>/<foldername>/params.pkl
```

If you have rllab installed, you can also visualize the results
using `rllab`'s viskit, described at
the bottom of [this page](http://rllab.readthedocs.io/en/latest/user/cluster.html)


This codebase is based on `rlkit`. You can see the original [here](https://github.com/vitchyr/rlkit).
=======
# mujoco-py [![Documentation](https://img.shields.io/badge/docs-latest-brightgreen.svg?style=flat)](https://openai.github.io/mujoco-py/build/html/index.html) [![Build Status](https://travis-ci.org/openai/mujoco-py.svg?branch=master)](https://travis-ci.org/openai/mujoco-py)

[MuJoCo](http://mujoco.org/) is a physics engine for detailed, efficient rigid body simulations with contacts. `mujoco-py` allows using MuJoCo from Python.

## Synopsis

### Install MuJoCo

1. Obtain a 30-day free trial on the [MuJoCo website](https://www.roboti.us/license.html)
   or free license if you are a student.
   The license key will arrive in an email with your username and password.
2. Download the MuJoCo version 1.50 binaries for
   [Linux](https://www.roboti.us/active/mjpro150_linux.zip),
   [OSX](https://www.roboti.us/active/mjpro150_osx.zip), or
   [Windows](https://www.roboti.us/active/mjpro150_windows.zip).
3. Unzip the downloaded `mjpro150` directory into `~/.mujoco/mjpro150`,
   and place your license key (the `mjkey.txt` file from your email)
   at `~/.mujoco/mjkey.txt`.

### Install and use `mujoco-py`
To include `mujoco-py` in another package, add it to your requirements like so:
```
mujoco-py<1.50.2,>=1.50.1
```
To play with it interactively, follow these steps:
```
$ pip install -U 'mujoco-py<1.50.2,>=1.50.1'
$ python
import mujoco_py
from os.path import dirname
model = mujoco_py.load_model_from_path(dirname(dirname(mujoco_py.__file__))  +"/xmls/claw.xml")
sim = mujoco_py.MjSim(model)

print(sim.data.qpos)
# [ 0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.  0.]

sim.step()
print(sim.data.qpos)
# [  2.09217903e-06  -1.82329050e-12  -1.16711384e-07  -4.69613872e-11
#   -1.43931860e-05   4.73350204e-10  -3.23749942e-05  -1.19854057e-13
#   -2.39251380e-08  -4.46750545e-07   1.78771599e-09  -1.04232280e-08]
```

See the [full documentation](https://openai.github.io/mujoco-py/build/html/index.html) for advanced usage.

## Usage Examples

A number of examples demonstrating some advanced features of `mujoco-py` can be found in [`examples/`](/./examples/). These include:
- [`body_interaction.py`](./examples/body_interaction.py): shows interactions between colliding bodies
- [`disco_fetch.py`](./examples/disco_fetch.py): shows how `TextureModder` can be used to randomize object textures
- [`internal_functions.py`](./examples/internal_functions.py): shows how to call raw mujoco functions like `mjv_room2model`
- [`markers_demo.py`](./examples/markers_demo.py): shows how to add visualization-only geoms to the viewer
- [`serialize_model.py`](./examples/serialize_model.py): shows how to save and restore a model
- [`setting_state.py`](./examples/setting_state.py):  shows how to reset the simulation to a given state
- [`simpool.py`](./examples/simpool.py): shows how `MjSimPool` can be used to run a number of simulations in parallel
- [`tosser.py`](./examples/tosser.py): shows a simple actuated object sorting robot application

See the [full documentation](https://openai.github.io/mujoco-py/build/html/index.html) for advanced usage.

## Development

To run the provided unit and integrations tests:

```
make test
```

## Credits

`mujoco-py` is maintained by the OpenAI Robotics team. Contributors include:

- Alex Ray
- Bob McGrew
- Jonas Schneider
- Jonathan Ho
- Peter Welinder
- Wojciech Zaremba
