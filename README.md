# nbaGNNs

This project is an implementation of several graph neural network models for link prediction on the weighted, directed NBA point-differential
graph for the 2013-2019, 2021 seasons. Run models.py to select train, and test a model. 

The input to the models are graphs: One graph has nba offenses and defenses as nodes, and edges representing interactions between them, weighted and directed according to several game statistics. The other graph has teams as nodes and its weighted directed edges represent vegas point spreads.Four Factors statistics (https://www.basketball-reference.com/about/factors.html) and point differential are weighted the most in the offense/defense graph. All graphs are row normalized to facilitate random walks. Game statistics were obtained from basketball-reference via https://github.com/roclark/sportsipy, and Vegas lines were obtained from Bovada via https://www.kaggle.com/erichqiu/nba-odds-and-scores. As a preproccessing step, The Oracle Adjustment is applied as described in 'An Oracle method to predict NFL games' (2012 Balreira, Miceli, Tegtmeyer, #http://ramanujan.math.trinity.edu/bmiceli/research/NFLRankings_revised_print.pdf) to enhance the random walks.

Next, node2vec (2016, Grover, Leskovec, https://snap.stanford.edu/node2vec/) is applied to the graphs to compute a feature representation of all offense and defense nodes, and all teams in the Vegas graph. Then the graphs along with the node2vec representations are passed to one of three graph convolutional layers described in 'Diffusion Convolutional Neural Network' (2015 Atwood, Towsely https://arxiv.org/pdf/1511.02136.pdf), 'Graph Attention Networks' (2018 Velickovic, P.; Cucurull, G.; Casanova, A.; Romero, A.; Lio, P.; and Bengio, https://arxiv.org/pdf/1710.10903.pdf),
or 'Graph Neural Networks with Convolutional ARMA Filters' (2019 Filippo Maria Bianchi, Daniele Grattarola, Lorenzo Livi, Cesare Alippi https://arxiv.org/pdf/1901.01343.pdf). These layers are implemented using spektral https://github.com/danielegrattarola/spektral. 

After the new node representations are computed from the GCN layer, the representation of both offenses and defenses, along with both teams' representation in the Vegas graph, are passed to a regression neural network to predict the score differential of a game. The model is tested during the selected year and day range, and its win percentage against the spread and against the moneyline are printed along with its MSE for the testing range. 

