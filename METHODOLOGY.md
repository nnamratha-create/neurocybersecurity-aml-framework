# Formal Mathematical Framework: Adversarial Electrophysiological Manipulation

## 1. The Signal Generation Boundary
Let the raw neural telemetry stream be modeled as a continuous multi-channel time-series matrix:

$$X \in \mathbb{R}^{C \times T}$$

Where:
* $C$ represents the finite set of physical spatial acquisition nodes (EEG electrode channels).
* $T$ represents discrete temporal measurement boundaries dictated by the sampling frequency ($f_s$).

## 2. The Adversarial Objective Function
An adversary manipulating the telemetry path seeks to compute a continuous perturbation matrix $\mathcal{E} \in \mathbb{R}^{C \times T}$ to generate a compromised signal stream:

$$\tilde{X} = X + \mathcal{E}$$

The generation of $\mathcal{E}$ is executed by solving an optimization constraint configured to maximize the classification loss function $\mathcal{L}$ of the target deep BCI neural decoder model $f(\cdot)$, while keeping the perturbation sub-clinical (invisible to traditional baseline signal filters) via an $L_\infty$-norm bound ($\epsilon$):

$$\max_{\mathcal{E}} \mathcal{L}\left(f(X + \mathcal{E}), y_{\text{true}}\right) \quad \text{subject to} \quad \Vert\mathcal{E}\Vert_\infty \le \epsilon$$

Where $y_{\text{true}}$ represents the verified user intent (e.g., motor intent mapping). If $\epsilon$ is kept below the background noise floor of human neural physiology ($\approx 0.5\mu\text{V} - 2\mu\text{V}$), standard frequency band filters (Delta, Theta, Alpha, Beta) fail to classify the attack as an anomaly.

## 3. Defense Verification Matrix
To intercept this attack, we introduce an Unsupervised Isolation Boundary. Instead of modeling static amplitude thresholds, incoming signal windows are mapped to an isolation pathway depth space $h(x)$. The anomaly score $s(x, n)$ for an extracted signal epoch $x$ is defined as:

$$s(x, n) = 2^{-\frac{\mathbb{E}(h(x))}{c(n)}}$$

Where:
* $\mathbb{E}(h(x))$ is the average path length across a forest of isolation trees.
* $c(n)$ is the average path length of an unsuccesful search in a Binary Search Tree built with $n$ nodes.
* A convergence toward $s \to 1$ triggers an immediate telemetry lockout, isolating the physical feedback loop before neuromodulation impulses are delivered back to the motor cortex.
*
