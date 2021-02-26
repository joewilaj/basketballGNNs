# nbaGNNs

This project is an implementation of several graph neural network models for link prediction on the weighted, directed NBA point-differential
graph for the 2013-2019, 2021 seasons. Open src/models.py to select a year and day range for testing or to adjust hyperparameters. Run src/models.py train and test a model.
Model predictions for each model for the 2021 season can be found in the predictions folder. The prediction printed is (Home Score - Away Score).

The input to the models are graphs representing the state of the season on a given day: One graph has nba offenses and defenses as nodes, and edges representing interactions between them, weighted and directed according to several game statistics. The other graph has teams as nodes and its weighted directed edges represent Vegas point spreads. [Four Factors statistics](https://www.basketball-reference.com/about/factors.html) and point differential are weighted the most in the offense/defense graph. All graphs are row normalized to facilitate random walks. Game statistics were obtained from basketball-reference via https://github.com/roclark/sportsipy, and Vegas lines were obtained from https://www.kaggle.com/erichqiu/nba-odds-and-scores. As a preproccessing step, The Oracle Adjustment is applied as described in [_An Oracle method to predict NFL games_](http://ramanujan.math.trinity.edu/bmiceli/research/NFLRankings_revised_print.pdf) (2012 Balreira, Miceli, Tegtmeyer) to enhance the random walks on the graphs. 

Next, [_node2vec_](https://snap.stanford.edu/node2vec/) (2016, Grover, Leskovec) is applied to the graphs to compute a feature representation of all offense and defense nodes, and all teams in the Vegas graph. Then the graphs along with the node2vec representations are passed to one of 4 graph convolutional layers described in [_Diffusion Convolutional Neural Network_](https://arxiv.org/pdf/1511.02136.pdf) (2016, Atwood, Towsely), [_Graph Attention Networks_](https://arxiv.org/pdf/1710.10903.pdf) (2018 Velickovic, Cucurull, Casanova, Romero, Lio, Bengio), [_Graph Neural Networks with Convolutional ARMA Filters_](https://arxiv.org/pdf/1901.01343.pdf) (2019 Bianchi, Grattarola, Livi, Alippi), and [_How Powerful are Graph Neural Networks?_](https://arxiv.org/abs/1810.00826) (2018, Hu, Leskovec, Jegelka, Xu). These layers are implemented using [_spektral_](https://github.com/danielegrattarola/spektral).

After the new node representations are computed, the representation of both offenses and defenses, along with both teams' representation in the Vegas graph, are passed to a regression neural network to predict the score differential of a game. The model is tested during the selected year and day range, and its win percentage against the spread and against the moneyline are printed along with its MSE for the games in the testing range. 

[Set up the environment](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-from-an-environment-yml-file) using deepnba.yml:

conda env create -f deepnba.yml

