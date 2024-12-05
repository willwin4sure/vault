When humans learn, they already have a large set of built-up priors about representations, models, and skills; we talked about this during [[Representation Learning I#Representations|representation learning]]. How can we give deep nets similar access to prior knowledge?

One modern idea is to pretrain the model on various tasks. Hopefully, we can transfer various bits of knowledge to other tasks, e.g.:

* Transferring knowledge about the mapping.
* Transferring knowledge about the outputs.
* Transferring knowledge about the inputs.

## Transferring Knowledge of Mapping

### Finetuning

One method is called ==finetuning==. You pretrain on a lot of data for a certain task $A$, which gives you weights $W$. Then, you maintain all or most of $W$, and train it on another task $B$. The learned *representation* (i.e. the *encoder*) hopefully transfers.

Indeed, if the types of the inputs are the same for $A$ and $B$, most of the early structure will remain the same even if you don't freeze it or keep learning rate low during training. [[Representation Learning II#Self-Supervised Contrastive Learning|Self-supervised contrastive learning]], where your representation is forced to place similar data next to each other, is one useful way of pretraining on large amounts of data.

> [!question]
> What if our input/output dimensions don't match?

We can just glue on some linear (or more complicated) projection maps to change the dimensionality, for example. Another idea is to have input/output "heads" that get swapped out, while the internal body of computation gets transferred. 

If your data is really high dimensional, another idea is to run PCA and take the highest values in the spectra.

> [!question]
> How do we avoid overfitting to the small amount of data? We don't want to forget the original general representation.

Here are a few methods:

* Freeze most of the network, only train final layers.
* Use a small learning rate.
* Perform early stopping.
* Continue training a little on the original data as well, if available.

### Domain Adaptation

You have a source domain and a target domain and wait to align the representation spaces so that you can use the source model. One idea is to use a ==domain adversarial neural network (DANN)==, which first maps the input to some set of features, and then has two heads, one for classes and the other for domains.

Then, we backprop from the classes to train the model and *reverse backprop* from the domains (i.e. flip the gradient into the features). This makes it good at predicting classes and *bad* at predicting the domain. Then, your representation should be insensitive to the domain but still contains useful information for the class.

Another problem is to use *labeled source data* and *unlabeled target data* to train a ==unsupervised domain adaptation== model. One basic way is to just run the pretrained model directly, but pass it through a threshold at the end (e.g. the signal must be strong enough) and treat that as ground truth to train a student model. You can also give the student model the original source labeled data during training. This method is called ==align and distill (ALDI)==. 

## Transferring Knowledge of Outputs

### Knowledge Distillation

Start with a large fixed pretrained teacher model. Learn a student model so the difference between the predictions is small.

> [!idea]
> This often can be better for classification problems where you predict some softmax output. The student can potentially learn more from the distributional output than just the ground truth label (e.g. this is a cat that looks like a tiger).

This is also one reason closed-source models often don't give you the full distribution over tokens, and instead just a sampling. It prevents you from training your own model on it.

### Cross-Modal Distillation

Say you have a teacher with access to RGB images and you want to train a student that has access to depth. Works if you have training data for RGB but not for depth, and want to run on RGBD images.

### Ensemble Distillation

==Ensemble models== are often still better than individual models. Distilling them can reduce computational cost of running them.

### Contrastive Representation Distillation

Suppose you want two models to have aligned intermediate representations. So you can take the logits at some internal layer and make sure they are invariant up to some viewing transformation. This way the student is forced to learn the same representation as the teacher.

### Compression

As an aside, TinyML is another approach to creating smaller models from bigger ones. This includes techniques like pruning and quantization of models.

## Foundation Models

==Foundation models== are generally useful pretrained models. They become more and more useful the more tasks fall within its domain of knowledge, even without finetuning.

These demonstrate few-shot and in-context learning capabilities. One new technique to edge out performance from these models is ==prompt engineering==. One method of doing this systematically is tuning some prefix tokens before your task. Another useful technique is called ==chain-of-thought prompting==. 

Prompting has spilled into other domains as well, e.g. visual prompts that you can add to the edge of images before feeding them into CLIP.

## Transferring Knowledge of Inputs

One idea is to use [[Deep Generative Models|generative models]] as a method of creating stronger data, which we refer to as ==data++==. Generative models are an amazing form of data compression: a basic diffusion model can map some element of latent space $\mathbf{z}\sim \mathcal{N}(0,I)$ into a huge range of possible images. They also allow us to define new operations on data, e.g. interpolation, extrapolation, manipulation, composition, optimization, and labelling. 

We can now use this to our benefit in training models. For example, if you have some image classifier, you can improve its accuracy and robustness by finding that image in the latent space and adding some perturbations within latent space (i.e. the resultant generative outputs) into your dataset.

Note that there will often be directions in your latent space that correspond to meaningful changes to your generated images, e.g. zoom/shift/brighten/darken. Often, a way of getting a space that actually does better at this is using some early layer in the generative model. This is due to the [[Variational Autoencoders#Latent Space|latent space picture]] for VAEs: you might jump across a seam in you original data distribution. However, an intermediate layer may be smoother since it isn't forced to be Gaussian.

Another use of generative models is as a data source for multi-view representation learning. In particular, you first make a generative model that creates different views, and then use these views for contrastive learning.

## Meta Learning

The idea here is to learn to learn fast:

![[meta_learning.png|center|512]]

One of the seminal works in this space is ==Model-Agnostic Meta Learning (MAML)==, which attempts to learn an initialization such that a few steps of finetuning SGD will update it effectively.

Essentially, you have a few tasks. You start with an initialization and test it out by running backprop with SGD on the tasks, and see how good you do on each. Then you backprop *that loss* through to your initialization to update it.

You do this by expanding the normal forward and backward passes of SGD into one large meta-forward pass, which we can then meta-backward through. This is quite expensive.

---

**Next:** [[Scaling Laws]]