+++
title = "Master Thesis: Training LLMs to create Swarm Behavior"
description = "Training LLMs to translate Mission Descriptions in Natural Language into Controller for Robot Swarms."
date = "2025-01-01"
template = "article.html"
+++

# Overview

Designing collective behavior from scratch is difficult. To build robot swarms that solve a desired task, the complex effect of local interaction between all robots needs to be understood and correctly modeled. Then, individual robot controllers can be crafted that then result in an emergent global behavior at the swarm level by interaction with peers. However, decentrally controlled robot swarms have advantages or robustness, scalability and cost effectivenes.

Transformer-based Large Language Models (LLMS), publicly known as _AI_has recently shown surprising capabilites of semantic understanding of domain knowledge they were not-trained on. This has led to prompt-engineering approaches that assign a LLM the robot and task them to come up with the next action.
For swarm robotics, this approach has many downsides. First, LLMs are a very ressource heavy model, which represents a stark contrast to the small, lightweight robotic agents. Second, experiments in swarm robotics have sofar instructed them with known swarm algorithms, faced major challenges in creating convergent behavior or used a top down approach, where the LLM has global knowledge, which is unsuitable to achieve scalability and true collective behavior. Third, collective robotics is a very niche topic, and LLMs have likely not seen many examples durinig training, so prompt engineering is naturally limited.

In my master thesis I suggest training a LLM on examples of local agent behavior and resulting global swarm behavior to create a local-to-global model and represent the missing macro-micro-link for top-down behavior design and understanding. In this approach, the LLM is not the robot controller itself, but takes the place of a meta-model that generates the actual robot controllers according to given constraints.
In the scope of the thesis I fine-tune a pre-trained LLM on a custom artificial dataset of textual descriptions of a scenario and corresponding behavior trees as robot controllers. The dataset builds on top of AutoMoDe-Maple, an evolutionary method for designing robot controllers according to a fitness function. While showing to still beeing a challenge to beat state-of-the-art methods using this approach, I show that the fine-tuned LLM understands enough about collective behavior to create working roboter controllers for scenarios and missions it was not trained on. It has also shown advantages over conventional optimization methods by producing suitable behaviors in response to environmental changes from a single zero-shot inference, while conventional approaches would require time-consuming optimization afterwards.
With what my initial investigation has shown, I strongly believe that the gap w.r.t. state-of-the-art in performance benchmarks is an engineering challenge that can be overcome with sufficient time and effort, and trained LLMs are a suitable meta-method to bridge the macro-micro gap.

A nice-to-have is, that since the input is natural-language text, my approach allows non-expert users to create collective behavior and instruct robot swarms without domain knowledge.

## Example

This example shows a scenario where the model has not seen missions of type _aggregation_ during training. Here, the robot swarm has to meet at the black area on the ground. Note that the E-Puck robot model used here cannot see the ground color from distance and the robots have to rely on the peers in their local environment to efficiently navigate to the correct place.

The yellow ball in the visualization refers to a light the robots can use for orientation. The position of the light however is not necessarily at the black area. This fact is ,in this case, communicated to the LLM by the absolute position. The placement of lights serves as an example of different environmental conditions the LLM _can_ make use of.

The example shows that the LLM was able to create a robot controller that achieves the desired behavior, even without having seen a single aggregation type mission during training. This goal has only understood by combinding the pre-training that makes the LLM understand general language and the fine-tuning on swarm behavior, that allows the LLM to understand the meaning of different local behaviors w.r.t. the global swarm goal.

LLM Input:
```
The circular arena, constructed with 19 walls, has a radius of 3.17 m. The following lights are present in the arena at coordinates: ((-0.36, 0.14)).Evenly positioned around the origin are 14 robots within a radius of 1.20 meters. The goal is for the robots to aggregate at the black circle. There are two areas on the floor: a circle at [-1.47, 1.91] with a radius of 0.53 meters in white, and another circle at [-0.36, 0.14] with a radius of 1.03 meters in black.
Generate the behavior tree that achieves the objective of this mission.
```

Technical output of the LLM, representing a behavior tree:
```
 --nroot 3 --nchildroot 3 --n0 0 --nchild0 2 --n00 6 --c00 0 --p00 0.9199 --n01 5 --a01 2 --p01 0 --n1 0 --nchild1 2 --n10 6 --c10 1 --p10 0.0955 --n11 5 --a11 0 --rwm11 2 --p11 0 --n2 0 --nchild2 2 --n20 6 --c20 0 --p20 0.5555 --n21 5 --a21 2 --p21 0
```

Visualization of the global behavior achieved by the behavior tree created by the LLM. You can see that the robots coherently move to the black area and stay close to each other.

![visualization of aggregation behavior](/media/projects/master-thesis-aggregation-example.gif)


# Git Repositories
 
 - [Dataset](https://github.com/julianjandeleit/swarm_descriptions)
 - [Training and Evaluation](https://github.com/julianjandeleit/thesis_llm_collective_behavior_generation)
 
# References

 - [Swarm Robotics: A Formal Approach by Heiko Hamann](https://link.springer.com/book/10.1007/978-3-319-74528-2)
 - [AutoMoDe-Maple: Kuckling et al.](https://doi.org/10.1007/978-3-030-00533-7_3)