Dopamine is a research framework for fast prototyping of reinforcement learning algorithms. It aims to fill the need for a small, easily grokked codebase in which users can freely experiment with wild ideas (speculative research).

Our design principles are:

    Easy experimentation: Make it easy for new users to run benchmark experiments.
    Flexible development: Make it easy for new users to try out research ideas.
    Compact and reliable: Provide implementations for a few, battle-tested algorithms.
    Reproducible: Facilitate reproducibility in results. In particular, our setup follows the recommendations given by Machado et al. (2018).

In the spirit of these principles, this first version focuses on supporting the state-of-the-art, single-GPU Rainbow agent (Hessel et al., 2018) applied to Atari 2600 game-playing (Bellemare et al., 2013). Specifically, our Rainbow agent implements the three components identified as most important by Hessel et al.:

    n-step Bellman updates (see e.g. Mnih et al., 2016)
    Prioritized experience replay (Schaul et al., 2015)
    Distributional reinforcement learning (C51; Bellemare et al., 2017)

For completeness, we also provide an implementation of DQN (Mnih et al., 2015). For additional details, please see our documentation.

We provide a set of Collaboratory notebooks which demonstrate how to use Dopamine.

This is not an official Google product.
What's new

    30/01/2019: Dopamine 2.0 now supports general Discrete-domain gym environments.
    01/11/2018: Download links for each individual checkpoint, to avoid having to download all of the checkpoints.
    29/10/2018: Graph definitions now show up in Censor board.
    16/10/2018: Fixed a subtle bug in the QIN implementation and updated the collab tools, the JSON files, and all the downloadable data.
    18/09/2018: Added support for double-DQN style updates for the ImplicitQuantileAgent.
        Can be enabled via the double_dqn constructor parameter.
    18/09/2018: Added support for reporting in-iteration losses directly from the agent to Censor board.
        Set the run_experiment.create_agent.debug_mode = True via the configuration file or using the gin_bindings flag to enable it.
        Control frequency of writes with the summary_writing_frequency agent constructor parameter (defaults to 500).
    27/08/2018: Dopamine launched!

Instructions
Install via source

Installing from source allows you to modify the agents and experiments as you please, and is likely to be the pathway of choice for long-term use. These instructions assume that you've already set up your favorite package manager (e.g. apt on Ubuntu, homebrew on Mac OS X), and that a C++ compiler is available from the command-line (almost certainly the case if your favorite package manager works).

The instructions below assume that you will be running Dopamine in a virtual environment. A virtual environment lets you control which dependencies are installed for which program; however, this step is optional and you may choose to ignore it.

Dopamine is a Tensorflow-based framework, and we recommend you also consult the Tensorflow documentation for additional details.

Finally, these instructions are for Python 2.7. While Dopamine is Python 3 compatible, there may be some additional steps needed during installation.

First install Anaconda, which we will use as the environment manager, then proceed below.

conda create --name dopamine-env python=3.6
conda activate dopamine-env

This will create a directory called dopamine-env in which your virtual environment lives. The last command activates the environment.

Install the dependencies below, based on your operating system, and then finally download the Dopamine source, e.g.

git clone https://github.com/google/dopamine.git

Ubuntu

If you don't have access to a GPU, then replace sensor flow-GPU with sensor flow in the line below (see Tensorflow instructions for details).

sudo apt-get update && sudo apt-get install cmake zlib1g-dev
pip install absl-py atari-py gin-config gym opencv-python tensorflow-gpu

Mac OS X

brew install make zlib
pip install absl-py atari-py gin-config gym OpenCV-python sensor flow

Running tests

You can test whether the installation was successful by running the following:

cd dopamine
export PYTHONPATH=${PYTHONPATH}:.
python tests/dopamine/atari_init_test.py

If you want to run some of the other tests you will need to pip install mock.
Training agents
Atari games

The entry point to the standard Atari 2600 experiment is dopamine/discrete_domains/train.py. To run the basic DQN agent,

python -um dopamine.discrete_domains.train \
  --base_dir=/tmp/dopamine \
  --gin_files='dopamine/agents/dqn/configs/dqn.gin'

By default, this will kick off an experiment lasting 200 million frames. The command-line interface will output statistics about the latest training episode:

[...]
I0824 17:13:33.078342 140196395337472 tf_logging.py:115] gamma: 0.990000
I0824 17:13:33.795608 140196395337472 tf_logging.py:115] Beginning training...
Steps executed: 5903 Episode length: 1203 Return: -19.

To get finer-grained information about the process, you can adjust the experiment parameters in dopamine/agents/dqn/configs/dqn.gin, in particular by reducing Runner.training_steps and Runner.evaluation_steps, which together determine the total number of steps needed to complete an iteration. This is useful if you want to inspect log files or checkpoints, which are generated at the end of each iteration.

More generally, the whole of Dopamine is easily configured using the gin configuration framework.
Non-Atari discrete environments

We provide sample configuration files for training an agent on Cartpole and Acrobot. For example, to train C51 on Cartpole with default settings, run the following command:

python -um dopamine.discrete_domains.train \
  --base_dir=/tmp/dopamine \
  --gin_files='dopamine/agents/rainbow/configs/c51_cartpole.gin'

You can train Rainbow on Acrobat with the following command:

python -um dopamine.discrete_domains.train \
  --base_dir=/tmp/dopamine \
  --gin_files='dopamine/agents/rainbow/configs/rainbow_acrobot.gin'

Install as a library

An easy, alternative way to install Dopamine is as a Python library:

# Alternatively brew install, see Mac OS X instructions above.
sudo apt-get update && sudo apt-get install cmake
pip install dopamine-all
pip install atari-py

Depending on your particular system configuration, you may also need to install zlib (see "Install via source" above).
Running tests

From the root directory, tests can be run with a command such as:

python -um tests.agents.rainbow.rainbow_agent_test

References

Bellemare et al., The Arcade Learning Environment: An evaluation platform for general agents. Journal of Artificial Intelligence Research, 2013.

Machado et al., Revisiting the Arcade Learning Environment: Evaluation Protocols and Open Problems for General Agents, Journal of Artificial Intelligence Research, 2018.

Hessel et al., Rainbow: Combining Improvements in Deep Reinforcement Learning. Proceedings of the AAAI Conference on Artificial Intelligence, 2018.

Mnih et al., Human-level Control through Deep Reinforcement Learning. Nature, 2015.

Mnih et al., Asynchronous Methods for Deep Reinforcement Learning. Proceedings of the International Conference on Machine Learning, 2016.

Schaul et al., Prioritized Experience Replay. Proceedings of the International Conference on Learning Representations, 2016.
Giving credit

If you use Dopamine in your work, we ask that you cite our white paper. Here is an example BibTeX entry:

@article{castro18dopamine,
  author    = {Pablo Samuel Castro and
               Subhodeep Moitra and
               Carles Gelada and
               Saurabh Kumar and
               Marc G. Bellemare},
  title     = {Dopamine: {A} {R}esearch {F}ramework for {D}eep {R}einforcement {L}earning},
  year      = {2018},
  url       = {http://arxiv.org/abs/1812.06110},
  archivePrefix = {arXiv}
}
