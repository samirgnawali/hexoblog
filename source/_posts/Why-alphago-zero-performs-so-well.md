---
title: Why AlphaGo zero performs so well
date: 2017-11-06 15:21:18
tags: AlphaGo
keywords:
- AlphaGo
- AlphaGo zero
- Deepmind
disqusIdentifier: vksbhandary-why-alphago-zero-performs-so-well
sitemap: false
coverImage: https://vikasbhandary.com.np/assets/images/alphagozero.png
coverMeta: out
metaAlignment: center
coverSize: partial
autoThumbnailImage: false

---
[Deepmind's](https://deepmind.com/) [AlphaGo](https://deepmind.com/research/alphago/) made a huge leap in history of computer science, when it defeated world's top professional players in march 2016, which was previously believed to be at least a decade away. But it didnt stop there, latest evolution of AlphaGo called ["AlphaGO Zero"](https://www.nature.com/articles/nature24270.epdf?author_access_token=VJXbVjaSHxFoctQQ4p2k4tRgN0jAjWel9jnR3ZoTv0PVW4gB86EEpGqTRDtpIz-2rmo8-KG06gqVobU5NSCFeHILHcVFUeMsbvwS-lxjqQGg98faovwjxeTUgZAUMnRQ) has gone many steps further than its previous version as it has reached superhuman performance, winning 100–0 against the previous champion AlphaGo. 

<!-- more -->
<!-- toc -->
## AlphaGo
Alphago was initially trained on dataset of expert moves of professional GO players. The process of learnig from labled training data is called [Supervised learning](https://en.wikipedia.org/wiki/Supervised_learning). AlphaGO used a 13 layer neural network (NN) called "policy network" to guess next move and similar NN called "value network" to predict the winning rate. After this step, it was made to play with itself, this type of learning is called [Reinforcement learning](https://en.wikipedia.org/wiki/Reinforcement_learning). Then finally Alphago combines both policy and value network using algorithm called Monte Carlo tree search (MCTS) in order to find the next best move. 

## AlphaGo Zero
[AlphaGO Zero](https://deepmind.com/blog/alphago-zero-learning-scratch/) was developed with goal to create AI with superhuman proficiency. AlphGo zero teaches itself GO by self-play reinforcement learning and doenst use the any human knowledge other than domain knowledge.
Other Key differences between AlphaGo zero and  are:
- AlphaGO zero uses one NN instead of two.
- AlphaGO zero uses black and white stones as input instead of hand picked features.
- AlphaGO zero uses simpler Tree search algorithm without using “rollouts”.

In short, the algorithm was made more general and it didn't even used human generated data.

### How well it worked?

{% image center fig-100 https://storage.googleapis.com/deepmind-live-cms/documents/TrainingTime-Graph-171019-r01.gif  "Source Deepmind blog post" %}

AlphaGo Zero outperformed [AlphaGo Lee](https://www.nature.com/articles/nature24270.epdf?author_access_token=VJXbVjaSHxFoctQQ4p2k4tRgN0jAjWel9jnR3ZoTv0PVW4gB86EEpGqTRDtpIz-2rmo8-KG06gqVobU5NSCFeHILHcVFUeMsbvwS-lxjqQGg98faovwjxeTUgZAUMnRQ) only after 36 hours of training, which was trained for several months. After continuous training for 72 hours AlphaGo Zero defeted the version of AlphaGo which defeated Lee Sedol. AlphaGo Lee was trained on a network of machines using 48 tensor processing units (TPUs) whereas AlphaGo Zero was trained on a single machine using 4 TPUs only.


{% image center fig-100 https://storage.googleapis.com/deepmind-live-cms/images/AlphaGo%2520Efficiency.width-1500.png  "Source Deepmind blog  post" %}

### Why it worked?
#### Less complex algorithm
AlphaGo Zero reduces the complexity of overall algorithm by using only self play. Only NN used in AlphaGo Zero combines the roles of both policy network and value network into single architecture. It takes current board position and it's history as input and outputs both move probabilities and winnning probability. 

#### More efficient Tree search algorithm
MCTS in AlphaGo Zero is viewed as a powerful policy improvement operator, as its calculations are used as a way to evaluate and train the NN. MCTS performs a search for winning moves. The policy evaluation estimates the value function from many sampled trajectories. The results of this search is then used to drive the learning of the neural network. So after every game, a new and potentially improved network is selected for the next self-play game.[[1](https://medium.com/intuitionmachine/the-strange-loop-in-alphago-zeros-self-play-6e3274fcdd9f)] 

#### Less Training required
AlphaGo Zero uses less training (3.9 million games vs 30 millions games). The reason behind it is that AlphaGo Zero uses self play for training. The data produced by self-play had more advanced moves as compared to human counterpart. After every iteration, the network selects only the best moves, So it quickly gained the superhuman expert level.


### What AlphaGo Zero's success means?

#### Superhuman AI is possible
AlphaGo Zero is the very first system that can be called as Superhuman Artificial intelligence system. Although AlphaGo and AlphaGo Zero are very domain specific system, they have mastered strategic and tactical [innovations](https://deepmind.com/blog/innovations-alphago/) in the field of GO, which humans will study for many years.

#### Using human Generated Data isn't necessarily the best approach 

AlphaGo Zero started learning from scatch and didnt need human knowledge other than the game rules. Although this might sound strange but by self-play method AlphaGo Zero was able to produce better and more accurate dataset. This demonstrate that a pure reinforcement learning approach is fully feasible without human knowledge or intervention.

#### Shift towards narrow AI to AGI
The most important [goal](https://www.youtube.com/watch?v=WXHFqTvfFSw) of Deepmind was to understand the "knowledge". Now that AlphaGo Zero was successful in gaining superhuman knowledge, it can be easily speculated that more tech companies will now try to implement it in broad domains of problems using more generalized approach. Deepmind has already [announced](https://www.bloomberg.com/news/articles/2017-10-18/deepmind-s-superpowerful-ai-sets-its-sights-on-drug-discovery) that it will be using same approach for drug discovery.