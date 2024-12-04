## Recurrent Neural Networks

You've learned about these; they have memory through a hidden state. You can make them deep by stacking many on top of each other.

You can unroll the RNN through time in order to do backpropagation through time. During training you should probably have a fixed maximal window through which to pass gradients back. Note that this means that multiple gradients on your weights need to be accumulated in each backward step.

> [!warning]
> Old gradients often get forgotten! This is because the hidden update matrix $W$ gets exponentiated which makes it hard to pass information. The gradients can vanish or explode.

## Long Short Term Memory (LSTMs)

These are a special kind of RNN designed to avoid forgetting. The default behavior is to not forget an old state. Instead of forgetting by default, the network has to *learn to forget*.

Here's a picture of the architecture. Note that each yellow block contains trainable weight and bias parameters. 

![[lstm_arch.png|center|512]]

Let's break it down. The main memory through-line is called the cell state:

![[lstm_cell_state.png|center|384]]

The first unit tells the LSTM what parts of the cell state to forget via
$$
f_{t}=\sigma \left( W_{f} \cdot [h_{t-1},x_{t}]+b_{f} \right).
$$
Low values passing through the sigmoid result in forgetting; high values result in remembering.

![[lstm_forgetting.png|center|384]]

The second unit tells the LSTM what parts of the cell state to write to via $i_{t}$, and what to write via $\tilde{C}_{t}$. The equations are
$$
\begin{align}
i_{t}&=\sigma \left( W_{i}\cdot [h_{t-1},x_{t}]+b_{i} \right),  \\
\tilde{C}_{t}&=\tanh \left( W_{C}\cdot[h_{t-1},x_{t}]+b_{C} \right).
\end{align}
$$

![[lstm_writing.png|center|384]]

Then, we need to actually forget the old memory and write the new ones. This updates the cell state via
$$
C_{t}=f_{t} * C_{t-1}+i_{t} * \tilde{C}_{t}.
$$

![[lstm_update.png|center|384]]

Finally, we need to decide what to output and what to store in the hidden state. These are
$$
\begin{align}
o_{t}&=\sigma \left( W_{o}\cdot[h_{t-1},x_{t}]+b_{o} \right), \\
h_{t}&=o_{t}*\tanh(C_{t}).
\end{align}
$$

![[lstm_output.png|center|384]]

> [!idea]
> The cell state only gets multiplied by values in $[0,1]$, which avoids problems with exploding/vanishing weight exponentials.

## Sequence Modeling

An autoregressive model takes some prefix and predicts a distribution for the next token. You can generate text by sampling this distribution and feeding the longer prefix back into the model for the next token.

Training is done via teacher forcing: you pass in a sequence of correct text and the model gives you a distribution for each token (as a next token from its prefix), and you can build a loss by using the real answers against these distributions.

Another way of sampling is via ==beam search==, which you learned about in NLP.

> [!idea] (Fast weights and slow memory)
> One way of conceptualizing memories in neural networks is that "slow memories" are stored in the weights via the training process, while "fast memories" are the activations on a given input (i.e. you construct some representation of the data point).
> 
> You might also want to try fast weights or slow memory. ==Hypernets== are nets that output weights of another net. ==Code books== use tensors of activations that are learned (backpropagated to).

---

**Next:** [[Representation Learning I]]
