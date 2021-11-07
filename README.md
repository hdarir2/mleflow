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

# High-fidelity Shadow-tor Simulation
- [Modified Shadow-Tor plugin.](https://github.com/ccheng32/shadow-plugin-tor/pull/1)
- [Modified Tor binary with MLEFlow estimation algorithm.](https://github.com/ccheng32/tor/pull/3)
- [Configuration files for MLEFlow and Torflow simulation.](https://github.com/ccheng32/shadow-sim-configs)

If you don't have a working Shadow binary yet, download and build the shadow binary following the steps in this [repository](https://github.com/ccheng32/shadow).

Then follow the following steps to modify and build the custom Tor binary and TorFlow plugin:

  1. Download the Tor source code from this [branch](https://github.com/ccheng32/tor/pull/3). The directory name of the source code is `tor`.
  2. Download the Tor/TorFlow plugin for shadow from this [branch](https://github.com/ccheng32/shadow-plugin-tor/pull/1). 
  3. Change to the `shadow-plugin-tor` directory, or whatever directory was downloaded in step 2.
  3. Build and install with the following command:
     ```
     ./setup build -c -j64 --tor-prefix ../tor/ && ./setup install 
     ```
     Note that the path to the Tor binary source code is `../tor/`. Change that to point to wherever you stored the Tor binary source code.

The test environment configurations can be downloaded from this [repository](https://github.com/ccheng32/shadow-sim-configs). The 3\% network configuration used in the paper is under the `shadowtor-0.03-a-control-2kclients-allrelays-config` directory. The name of the directory can be understood this way:
  - `0.03` means it's a 3\% network.
  - `control` means it uses the MLE algorithm.
  - `2kclients` means the network simulates 2000 clients downloading concurrently.
  - `allrelays` means the authorities in the network uses the MLE algorithm to predict the capacity of all (guard, middle, exit) relays, as opposed to only estimating the exit relays.

`cd` into the desired config directory and run the following command to start the simulation:

```
shadow shadow.config.xml > shadow.log
```

The output of the shadow binary will be stored in `shadow.log`. The outputs of the virtual hosts can be found in `shadow.data/hosts/[HOSTNAME]`.
