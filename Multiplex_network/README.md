## On the design of integral multiplex control protocols in nonlinear network systems with delays
### Introduction
This repository collects the supporting code for the manuscript **On the design of integral multiplex control protocols in nonlinear network systems with delays** ([see this link](https://arxiv.org/abs/2206.03535)). In the manuscript, we designed an integral control protocol delivered via multiplex architecture for a network system to ***reject polynomial disturbances*** and guarantee the ***non-amplification of the residual disturbances***. We capture the fulfillment of these requirements via a ***scalability*** property of the network. There are two sets of validations given in the manuscript, including *in-silico validation* (Figure 3, 4) and *experimental validation* (Figure 5, 6). Here we report all the code, data and information to replicate all our results in the manuscript. The code can also be used to apply our methodology to different settings. If you use the code, please reference our work (see references).

### Contents
The following list of files can be found in the repository, which reproduce the parameters/figures in the manuscript.
- **multiplex_optimization.m**

This code solves a convex optimization problem to obtain the gains for the multiplex control protocol (see Appendix B in the manuscript for details of the optimization problem). The optimal solution is made visualizable in the plot (marked with 'x') produced by the code which collects all the feasible solutions to the optimization problem. At the same time, the gains ($k_0$, $k_1$, $k_2$, $g_0$, $g_1$, $g_2$) will be displayed in the command window. Knowing the value of $g_0$, $g_1$, $g_2$, we can select the control gains for the delayed couplings according to $g_0=k_0^{(\tau)}k^\psi$, $g_1=k_1^{(\tau)}k^\psi$, $g_2=k_2^{(\tau)}k^\psi$, e.g. let $k^\psi=0.1$, then $k_0^{(\tau)}=10g_0$, $k_1^{(\tau)}=10g_1$, $k_2^{(\tau)}=10g_2$. Directly running the code gives the control gains for the *in-silico validation*, to get the gains for *experimental validation*, uncomment the four additional constraints as instructed in the code. 

- **in_silico_validation.m**

In this simulation, a total number of 1860 robots make up a formation of 30 concentric circles. For a given circle $i$ in the formation, there are $4i$ robots in the circle. The robot 1 and 3 in the inner circle are affected by the disturbances $d_1(t) = \[0.04+0.4\sin(0.5t)e^{-0.1t}, 0.04+0.4\sin(0.5t)e^{-0.1t}]$, $d_3(t) = \[-0.05t+0.4\sin(0.5t)e^{-0.1t}, -0.05t+0.4\sin(0.5t)e^{-0.1t}]$, respectively. Throughout all the simulations, we perturb the same robots (robot 1 and 3) with the same disturbances ( $d_1(t)$ and $d_3(t)$ ). The hand position deviation of the robots are recorded in Figure 4 in the manuscript.

- **max_hand_position_deviation.m**

This code simulates the case when the number of robots in the formation are increased, starting with a formation of 1 circle (4 robots) to a formation with 30 concentric circles (1860 robots). We recorded at each simulation the maximum hand position deviation for each robot on a given circle and finally plotted the largest deviation on each circle across all the simulations in Figure 3.

- **robotarium**
  - **robotarium_validation.m**
  - **posdev_hardware.mat**
  - **stepsize_hardware.mat**
  - **all other utilities to get the code run**

**robotarium_validation.m** provides a high-fidelity simulation in Matlab for the hardware experiments thanks to the utilities from Robotarium. Directly run the code and it will give a plot of hand position deviation for 12 robots in a formation of 2 concentric circles, with control gains obtained from **multiplex_optimization.m** and disturbances as described in **in_silico_validation.m**. In order to run the hardware experiments, comment all the plot commands in the code, i.e. line 203 and onwards, then submit it to [Robotarium](https://www.robotarium.gatech.edu/dashboard) (only submit the **robotarium_validation.m** code). 
During the hardware validation progress, the knowledge of step size of the hardware infrustructure is required to implement the multiplex layers of the integral control protocol. To this aim, we embedded the _tic-toc_ function in the code and reported the average step size from 10 sets of experiments in Figure 5. The average step size is around 0.033s which is consistent with the nominal value provided in Robotarium documents. However, some variability also occurs due to hardware implementations. This varaibility is handled as described in the referenced paper: we imposed the control gains of the integral actions to be smaller than the gains of the proportional action. This was done by solving the optimisation problem via **multiplex_optimization.m** this time uncommenting the following additional constraints in the code: $k_0 - 2k_1\ge0$, $k_0-2k_2\ge0$, $g_0-2g_1\ge0$, $g_0-2g_2\ge0$. We provide together with the code the average step size and average hand position deviation for all the robots across 10 sets of experiments in **stepsize_hardware.mat** and **posdev_hardware.mat** for readers' convenience. They will be loaded automatically and plotted once the code is run. We also refer the readers to Figure 6 in the manuscript for the plot in the *experimental validation* part.

### Results
- **Disturbances**

For better illustration, the animation of disturbances affecting the robots can be seen at [disturbances affecting robot 1 and 3](https://public.flourish.studio/story/1593251/). Notice that $d_1(t)$ contains a constant term and $d_3(t)$ includes a ramp component. However, both are rejected by the proposed integral multiplex controller, see next the animation of the robot hand position deviation and the videos from experiments.

- **Hand position evolvement via Flourish**

Please refer to [flourish data visualization](https://public.flourish.studio/story/1572969/) for a dynamical view of the robots' average hand position deviation from 10 sets of hardware experiments. 

- **Video from Robotarium simulator**

This video reports the robots' behaviour in the high fidelity simulator.

https://user-images.githubusercontent.com/55105896/171680439-bd8c03b4-e378-4364-b191-affb2b8533f8.mp4



- **Video from Robotarium hardware**

This video reports the robots' behaviour in one of the experiments we run via the Robotarium hardware. The figures in the referenced paper were obtained from a set of 10 experiments.

https://user-images.githubusercontent.com/55105896/171673274-713b7240-6b49-4799-97ef-2fa57fcc68e1.mp4


### Author and contributer
Shihao Xie (shihao.xie1@ucdconnect.ie)

### Reference
*Shihao Xie, Giovanni Russo. On the design of integral multiplex control protocols in nonlinear network systems with delays, 2022 ([arXiv](https://arxiv.org/abs/2206.03535)).*
