# Training AI Agents in Game Engine Envs

## Introduction 

One of the main benefits that game engines bring to machine learning is the ability to easily build new environments. These environments can then be used to train AI agents where the interactions can be immediately visualised to observe how well individual agents behave. Perhaps most interestingly, in settings with multiple AI agents, we can observe interesting group dynamics emerge--something that would be impossible notice without the visualisation aspect.

There are at least two general purpose game engines--Unity and Godot--that have plugins to interface with ML frameworks such as PyTorch. 

Unity's [ML-Agents](https://github.com/Unity-Technologies/ml-agents): 
Unity is the most widely used game engine today both amongst hobbyists and professionals who use it to create anything from small games to AAA-titles. Consequently, one of its greatest advantages is that it has very good documentation and a plethora of tutorials are available online. One drawback of Unity is that it is not open source, so a commercial license could be required. Also, even with a commercial license, the Unity has been changing its licensing fee structures in ways that have resulted in large user backlash. 

Godot's [RL-Agents](https://github.com/edbeeching/godot_rl_agents): 
Godot is an open source game engine whose popularity has been growing consistently over the years. Nevertheless, its user base is still much smaller compared to the likes of Unity, so the documentation can be somewhat lacking. Furthermore, the 3D rendering is not quite as advanced as Unity's at this stage. However, given Godot's steady growth with more commercial games being created with it, Godot is likely to become a major challenger to the commercial game engines in the long run.


## General Environment Building

The easiest way to explore how to use game engines for RL-trained agents is explore ready-made example environments:
- Unity Examples: https://github.com/Unity-Technologies/ml-agents/blob/develop/docs/Learning-Environment-Examples.md
- Godot Examples: https://github.com/edbeeching/godot_rl_agents_examples


## Walkthrough: Unity SoccerTwos Environment

We use the provided example ML-Agents environment from Unity called [SoccerTwos](https://unity.com/blog/games/made-with-unity-soccer-robots-with-ml-agents). You do not need to download Unity game engine separately to train and visualise agents, but only clone the [Unity ML-Agents Toolkit](https://github.com/Unity-Technologies/ml-agents). To train your first AI football team, follow instructions below: 

- To set up the environment, follow the tutorial from Hugging Face's Deep RL course: https://huggingface.co/learn/deep-rl-course/en/unit7/hands-on
    - Note that the libraries in the environment can be sensitive to the python version, and so it's best to use the exact version mentioned in the Hugging Face walkthrough.
- You need to download a compiled executable of the SoccerTwos environment in order to train agents. The links to the Mac, Linux, and Windows executables are provided in the Hugging Face tutorial.
    - Note, that to create custom RL environments you would need the full version of Unity in order to make environment changes and compile new executables that are used for environment interactions.

Once, the environment is set up, you are ready to begin your career as an AI coach!

To start training:

```bash
cd ml-agents
mlagents-learn ./config/poca/SoccerTwos.yaml \
    --env=./training-envs-executables/SoccerTwos/SoccerTwos.app \
    --run-id="SoccerTwos-MY-TEAM-NAME" \
    --no-graphics \
    --force
```

To train a reasonably good team, it can take 12-48 hours, but of course everything depends on how complex your models are. The default model for training uses [MA-POCA](https://rlg.mlanctot.info/2021/papers/AAAI22-RLG_paper_32.pdf), whose parameters are defined in `./config/poca/SoccerTwos.yaml`. By tweaking the `.yaml` file you can change the size and layers of the neural network models, and control typical hyperparameters like learning rates and time discounting. For more, see [documentation](https://unity-technologies.github.io/ml-agents/Training-ML-Agents/), or see CLI docs with `mlagents-learn --help`.


#### Useful Tips

To resume an interrupted training from an existing model:

```bash
!mlagents-learn ./config/poca/SoccerTwos.yaml \
    --env=./training-envs-executables/SoccerTwos/SoccerTwos.app \
    --run-id="SoccerTwos-MY-TEAM-NAME" \  # Which model to continue training
    --no-graphics \
    --resume # Continue training 
```

To train a new model using an existing model as a starting point:

```bash
!mlagents-learn ./config/poca/SoccerTwos.yaml \
    --env=./training-envs-executables/SoccerTwos/SoccerTwos.app \
    --run-id="SoccerTwos-MY-NEW-AND-IMPROVED-TEAM" \
    --no-graphics \
    --force \
    --initialize-from "SoccerTwos-MY-TEAM-NAME"
```

