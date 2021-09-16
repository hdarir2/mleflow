# mleflow
MLEFlow/Torflow/sbws implementation Guide
+ Reproducing some of the figures in the paper "MLEFlow: Learning from History to Improve Load Balancing in Tor"

Github Repo: https://github.com/hdarir2/mleflow

Dependencies:
Python 3.6 or newer versions
numpy
scipy
matplotlib
bisect
math
lambertw
sorted containers
decimal

Step 1: Import the ground truth cpaacities of the relays considered

Step 2: Run blocks 1 to 12 in the notebook in order to define the required weight vectors, path generating functions (one relay and three relays paths), observation calculation, discretization of the capacities set.

Step 3: Run the algorithm of choice. 

Notation: 
-lambdas= rate of arrival of users to the Tor network
-t= number of epochs
-w0= initial weight vector estimate
-noise= 0 (if the observation we want to consider are noiseless); 1 (if the observations are noisy= observation multiplied by a normal random variable ~N(1,0.5) truncated between 0.7 and 1.3)
-bw= vector containing the capacity of each relay in the network
-bwguard= vector containing the capacities of all relays that can be used in first position
-bwmiddle= vector containing the capacities of the relays that can only be used in the second position (without taking into consideration the guard relays that can also be used in the second position)
-bwmiddle2= vector containing the capacities of all the relays that can be used in the second poisition
-bwexit= vector containing the capacities of all relays that can be used in the third position

Algorithms for one relay paths networks:
- Torflow_P: torflow_onerelay function; run with and without noise.
- MLEFlow_Q: stochasticpoisson_onerelay function; run with and without noise.
- MLEFlow_CF: iterativepoisson_onerelay function; run with and without noise.
- sbws*: sbws_onerelay function; run with and without noise.

*Error between estimation of each algorithm and ground truth calculated ad plotted in a box plot.
*Using estimates, generate paths of one relays; compute their bandwidths using circ_bw and compare.

Algorithms for one relay paths networks:
- Torflow_P: torflow function; run with and without noise.
- MLEFlow_Q: stochasticpoisson function; run with and without noise.
- MLEFlow_CF: iterativepoisson function; run with and without noise.
- sbws*: sbws function; run with and without noise.

*Error between estimation of each algorithm and ground truth calculated ad plotted in a box plot.
*Using estimates, generate paths of one relays; compute their bandwidths using circ_bw and compare.
