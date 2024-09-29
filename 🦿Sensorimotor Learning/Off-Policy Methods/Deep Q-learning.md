In non-discrete spaces, we cannot simply keep a table of $Q$-values around. 

## Function Approximation

Instead, we try to approximate the $Q$-function using, say, a neural network $Q_{\phi}$, parameterized by $\phi$. It takes in a (state, action) pair and spits out an estimated value. 

For the discrete action case, we could instead take in a state and spit out a distribution over actions (this is what happens in [[⛺SPRL Homepage|SPRL]]). 

> [!example] (Fully-fitted Q-iteration algorithm)
> First, collect a dataset of tuples $(s_{i},a_{i},r(s_{i},a_{i}),s_{i}')$ using some policy.
> 
> We need to set a target $y_{i}=r(s_{i},a_{i})+\gamma \max_{a'}Q_{\phi}(s',a')$. In the discrete case, simply maximize over the distribution. In the continuous case, we could gradient ascent over $a$ (fixing $s$) to maximize the output of the NN.  
> 
> Then, we will update the network parameters by taking a gradient step in the direction towards minimizing the sum of differences between $Q_{\phi}(s_{i},a_{i})$ and $y_{i}$, via the loss function
> $$
> \min_{\phi}\sum_{i}^{}\| Q_{\phi}(s_{i},a_{i})-y_{i} \|^{2}. 
> $$
> 
> Repeat the target and parameter update steps until convergence.

For the rest of this section, we will be going through various potential issues with this technique and address them.

## Mini-Batch Updates

The dataset could be large, so we can't update $\phi$ in one-shot. Instead, train gradually on mini-batches, a technique called the ==online Q-iteration== algorithm.

If your batch has size $1$, we can write this out as taking an action $a_{i}$ to get a tuple $(s_{i},a_{i},r_{i},s_{i}')$and then updating
$$
y \leftarrow r(s_{i},a_{i})+\gamma \max_{a'}Q(s_{i}',a'),\quad
\phi\leftarrow\phi-\alpha \frac{ \partial Q_{\phi}(s_{i},a_{i}) }{ \partial \phi } (Q_{\phi}(s_{i},a_{i})-y_{i}).
$$
This is just stochastic gradient descent.

## Moving Target Problem

One issue with $Q$-learning is that the targets $y_{i}$ are always changing. This is in contrast to supervised learning, where the targets don't change so we can be relatively sure that gradient descent will converge.

Another implementation detail: you want to set
$$
\phi\leftarrow \phi-\alpha \frac{ \partial Q_{\phi} }{ \partial \phi } (s_{i},a_{i})\left( Q_{\phi}(s_{i},a_{i})-r(s_{i},a_{i})-\gamma \max_{a'}Q_{\phi}(s',a') \right). 
$$
You want to make sure the gradient doesn't pass through the target; we want to be updating $Q_{\phi}(s_{i},a_{i})$, not the target itself (which does also depend on $Q_{\phi}$).

### Replay Buffers

This can help you stay out of local minima: we keep a memory of past experiences, and randomly sample from it to train the neural network, encouraging diversity. Then, we use this new neural network to collect samples and add it to our memory. 

### Target Networks

One idea is to have a target network that only periodically gets updated (set to the $Q$-value network that we are training). This prevents the targets from moving as we update $Q$.

Or we can do slow updates to the target network, where $\phi'\leftarrow \tau \phi+(1-\tau)\phi'$ for a small $\tau$. 

## Overestimation Bias

Over-estimated $Q$-values leads to worse performance. This is why $Q$-learning appears to be less sample-efficient than other methods, even though we might expect it to be better (unlike on-policy methods where you have to throw out data each step, you can keep data around in a replay buffer, e.g.). 

Why are $Q$-values over-estimated? Well, we often take a max over $Q$-values! There are errors in the approximation, even though they should be correct on-average (NNs can be universal unbiased function approximators). The max takes the biggest error as the truth, unfortunately.

> [!example] (Double Deep $Q$-Network)
> The idea is that when we compute our target
> $$
> y=r+Q_{\phi}\left( s,\arg\max_{a}Q_{\phi}(s,a) \right),
> $$
> we are using the same network to both select an action and evaluate it, causing over-estimation. The idea of DDQN is to have *two* neural networks, one responsible for the internal action selection and the other responsible for the external evaluation.
> 
> We hope that the errors of these two different neural networks are uncorrelated, so we don't get the same over-estimation bias.
> 
> Practically, action selection will be done with the $Q$-network in-training, and evaluation will be done with the target network.

## Large Action Spaces

$Q$-values would remain unconstrained if we don't see any of those actions in the data. 

If we are doing online $Q$-learning, we really only care if the $Q$-values are being underestimated by the network; overestimation will lead to sampling in the data and we can correct for it. 

However, if we are doing offline $Q$-learning (data is just handed to us), then overestimation is still also an issue.

One way to fix this is to change the neural network architecture to a ==dueling network==, where instead of outputting a vector $Q(s,a)$, it outputs $V(s)$ and $A(s,a)$ in separate heads, and then sum $Q(s,a)=V(s)+A(s,a)$. This is based on the intuition that most actions from the same state should have about the same value, but we can adjust for certain actions through the advantage.

## Continuous Action Spaces

One way to get best actions $a_{t}$ from our $Q$-network is to do backpropagation. However, this is rather slow. A better technique is to train a ==policy network== that predicts the best action given the state. 

This actually looks quite similar to [[Advantage Actor-Critic Method|Actor-Critic methods]], where there is an actor (the policy network) and a critic (the $Q$-network).

However, when we were doing A2C methods, the rewards were non-differentiable and therefore we couldn't backprop through them... all we could do is reweight gradients using advantages, and then train the value network using TD error.

Here, however, we are approximating rewards via the $Q$-function, which is differentiable. This allows us to backprop through it.

> [!example] (Deep Deterministic Gradients, DDPG)
> We optimize the critic parameters $\theta_{q}$ via TD error:
> $$
> \min_{\theta_{q}}\left( Q_{\theta_{q}}(s_{t},a_{t})-r_{t}-\gamma Q_{\theta_{q}}(s_{t},a_{t}) \right), 
> $$
> and we optimize the actor parameters $\theta_{\mu}$ via
> $$
> \theta_{\mu}^{k+1}=\theta_{\mu}^{k}+\eta\cdot \nabla_{\theta_{\mu}}(\mu_{t}) \cdot \nabla_{\mu_{t}}Q_{\theta_{q}}(s_{t},\mu_{t}). 
> $$


This often doesn't work directly since it is too greedy: we need to have some more exploration. The idea is to add some Gaussian noise $n_{t}\sim \mathcal{N}$ to the output of the policy during data collection (updates do not take noise into account).

Here's some more pseudocode, courtesy of [OpenAI](https://spinningup.openai.com/en/latest/algorithms/ddpg.html):

![[DDPG.png|center|512]]

A more modern algorithm is called ==Twin Delayed DDPG== (TD3) and it implements some additional tricks:

* It actually learns *two* $Q$-functions instead of once, and uses the smaller of the two $Q$-values to form the targets in the Bellman error loss functions.
* It updates the policy and target networks less frequently than the $Q$-function. The paper recommends two $Q$-function updates per policy update.
* Adds noise to the target action, to make it harder for policy to exploit $Q$-function errors.

---

**Next:** [[Forward and Reverse KL]]