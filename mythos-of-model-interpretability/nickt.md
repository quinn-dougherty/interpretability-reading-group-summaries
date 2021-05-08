# The Mythos of Model Interpretability

[paper](https://arxiv.org/abs/1606.03490)

We often consider "interpretability" as an intuitively meaningful concept, but in fact people mean many different things when they use the word. This paper tries to make the term interpretability more precise by describing many of these concepts. What are the goals of interpretability research? What does it mean for a model to be interpretable? And how have people tried to make models more interpretable?

One goal of interpretability research is to build "trust" with a model, although there are several notions of what this means as well. It might mean that the model will perform well given new data that it hasn't seen before of the same kind as its test data. It might mean that humans feel subjectively comfortable with the model and expect it to make good decisions on their behalf. 

We might also want interpretability to give us concrete information about transferability - whether a model's decisions will generalize gracefully to new situations. New situations might even originate from a model's actions (when the model is an active agent within an environment), or might come from other agents exploiting a model's inner workings. One example of the latter is FICO credit scores, where the model is public, and people actively game the system to give themselves better scores without changing their "credit-worthiness".

We also might want to extract information from how models work. The model could suggest hypotheses that we can scientifically test for causal relationships, and there might be more general information that we can extract from how a model makes a good decision. Lastly, interpretability research might give us a way to understand whether a model is making decisions using fair or ethical reasoning, and contest these decisions when it is not.

What do we mean when we say that a model is interpretable? This concept can itself be broken down into two non-mutually exclusive ideas - transparency and post-hoc explanation.

Transparency refers to our level of insight about the internals of the model. One sense of this simulatability - whether a human can produce the same prediction of the model within "reasonable" time, although this is clearly somewhat subjective. Another sense of interpretable model is decomposability, whether or not you can break down how a model works in terms of intuitive explanations. We might also care about our knowledge about the learning algorithm ("algorithmic transparency"). Is a learning procedure guaranteed to converge? Are there properties of these solutions that we find attractive?

Post-hoc explanation instead refers to producing some useful information about a model's decision-making without much insight into how the model works. Some interpretability research tries to produce natural language explanations of how a model works, potentially using two connected models - one for producing predictions and another for explaining those predictions. Researchers might try to intricately visualize the representations or concepts learned by a model. They might also try to have the model parse the space of training examples, potentially inferring which training examples were most important for a given prediction.

Overall, there are several concepts under the umbrella of interpretability, and clarifying these concepts helps us to understand the term interpretability as well. For example, it's often claimed that linear models are "more interpretable" than artificial neural networks, though a linear model with many heavily pre-processed features might not satisfy many (if any) of the requirements above.


### NICK'S OPINION
I find the notion of "trust" to be one of the central appeals of interpretability research, with most of the other goals being ways to build trust. You might think of trust as being separated into "subjective trust" - whether people feel comfortable with a model's predictions in the future, and "justifiable trust" - whether or not the subjective version is actually justified in some sense.

I also think that humans are used to thinking in procedures/models that operate at a different level of abstraction than quantifiable models, and in that sense it's slightly misleading to call them entirely non-transparent outside of post-hoc explanation. You could imagine that a recipe for baking a cake is someone's model for that behavior that's both simulatable and decomposable.
