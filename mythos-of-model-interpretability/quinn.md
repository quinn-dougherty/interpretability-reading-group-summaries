# Quinn's summary of Mythos of Model Interpretability

As of the writing in 2017, we are told that _calls_ for interpretability in machine learning are not grounded in a _definition_ of interpretability. If I have a linear regression `y ~ m * x + b`, I can say that the value of `m` measures change in `y` _relative_ to change in `x`. I like to reason about models in this way, and I'm upset that I can't reason about neural nets in this way - with weights in a neural net, I don't see clear response to wiggling the way I do in a simple linear regression. This intuition does not suffice to study this thing called "interpretability". 

To begin from square one, we need to build up a _desiderata_ so that we know when something is "interpretable" or not. Some considerations for this desiderata are that 1. we have metrics to evaluate models based on predictions and ground truth, and we find that those metrics are not enough to _characterize_ a model, and 2. we care about properties of models that we struggle to model formally, such as ethicality and legality. Lipton will provide 5: trust, causality, transferability, informativeness, and fair & ethical decision-making. 

### The desiderata

1. **Trust**: we want to know not only that the model is accurate but that the dataset isn't lending legitimacy to what would be criticized as a bias if a human was making the same predictions the old fashioned way, at least that's what I could extract from the example given of police allocation based on crime rates; we don't tend to trust models that say "send lots of cops to neighborhood x" because we know that a positive feedback loop of more cops -> more arrests made -> higher arrest rate in model -> more cops -> etc. can run away from us. I don't think this example portrays what Lipton is trying to say with this section, when you observe the following quote 
> we might care not only about _how often a model is right_ but also _for which examples it is right_. If the model tends to make mistakes in regions of input space where humans also make mistakes, and is typically accurate when humans are accurate, then it may be considered trustworthy in the sense that there is no expected cost of relinquishing control.
I think an adjacent word to trust is _legitimacy_, but that is also a word that feels heavier than it is. A _user_ of a model should petition for it's legitimacy not by pleading to properties of the model, but by showing the **social process that created the dataset and into which they want to implement decisions is rigorous, sensitive, and free of runaway feedback loops**. 
2. **Causality**: supervised learning learns association. Given a static dataset that you can't intervene in, it can be very difficult to ascertain direction of the causal graph absent any subject matter expertise or common sense. 
> One might hope, however, that by interpreting supervised learning models, we could generate hypotheses that scientists could then test experimentally.
3. **Transferability**: when a model's deployment data is significantly different from it's testing data, and the model doesn't totally crash and burn, we say that the model is transferable. In this way, I think transferability is similar to robustness/OODR. A particularly pathological case of transferability failure is the adversarial case. The principle example given was credit scoring, which is subject to goodharting. Lipton claims about credit scores
> The fact that individuals actively and successfully game the rating system may invalidate its predictive power.
4. **Informativeness**: it's all well and good to train a model and press the "predict" button, but can you gain information by consulting the model, specifically perhaps it's internals?
> The most obvious way that a model conveys information is via its outputs. However, it may be possible via some procedure to convey additional information to the human decision-maker
5. **Fair & ethical decision-making**: it has been claimed that conformance to ethical standards can not be ascertained without explanations, presumably of "what a model is thinking". 
> Conventional evaluation metrics such as accuracy or AUC offer little assurance that a model and via decision theory, its actions, behave acceptably. ... algorithmic decisions should be _contestable_. So in order for such explanations to be useful it seems they must (i) present clear reasoning based on falsifiable propositions and (ii) offer some natural way of contesting these propositions and modifying the decisions appropriately if they are falsified.

These desiderata together describe what we would want from "interpretable machine learning". 

### Properties of interpretable models

1. **Transparency**: the opposite of opacity. White box. We know how it works. 
   - **Simulatability**: this is transparency at the level of the entire model. Can you store what the model is doing in your head? This is a subjective quantity, implied by limits of human cognition. For example, a decision tree can be simulatable if it doesn't have "too many" rules. Simulatability comes in two types: with respect to space (how big is the model) and with respect to time (how long does it take to run). 
   - **Decomposability**: here is transparency at the level of individual components. Compositionality is when if you understand parts and you understand how parts are glued together then you gain understanding of the whole for free. A "part" of a model can be an input, parameter, calculation, or split rule in a decision tree. This path to interpretability is not robust to arbitrary preprocessing and feature engineering, as a variable transformed into information that's best for the machine is sometimes not best for us. 
   > associations between flu risk and vaccination might be positive or negative depending on whether the feature set includes indicators of old age, infancy, or immunodeficiency
   - **Algorithmic transparency**: here is transparency at the level of the training algorithm. Intuitively, least squares regression having a closed form solution gives us a clear sense of "what the algorithm is doing" and zeros in on the true minimum of the error surface, while resorting to iterative descent in the deep learning case provides a level of indirection between algorithm and result. 
2. **Post-hoc interpretability**: what else can the model tell me? 
> Some common approaches to post-hoc interpretations include natural language explanations, visualizations of learned representations or models, and explanations by example 
   - **Text explanations**: "_humans often justify decisions verbally_". It would be nice for machines to generate their own verbal justifications of decisions. Some work is done training a justifier language model alongside an inference or behavior model of some kind to accomplish this. . 
   - **Visualization**: model internals are subject to visualization, model internals can be data about which we tell stories. 
   - **Local explanations**: a saliency map is a notion of what a model is _focusing on_. For a classifier, we can take the gradient of an output with respect to given input vector matching up with the right class. Intuitively, the gradient suffices to describe "focus" because it shows you where the steepest direction is, so it shows you where the model is intending to go next. A saliency map is a local explanation because disturbing a single pixel can result in a very different saliency map. 
   - **Explanation by example**: sometimes you can explain behavior on an input by appealing to a more intuitive input-output pair nearby. This can be formalized with k-nearest neighbors. 

### Discussion section
Lipton raises four points at the end of the paper. 
1. _Linear models are not strictly more interpretable than neural networks_. Under algorithmic transparency, linear models rule, however if you have too many features they lose simulatability and if you do a lot of feature engineering they lose decomposability. (For a linear model to be competitive with a neural network, lots of feature engineering may be required). As for post-hoc explanation, neural networks are the better tool. 
2. _Claims about interpretability much be qualified_. Say not that your model is interpretable, say that it is interpretable with respect to which desiderata, or whether it's transparent or post-hoc explainable. Transparency can be shown directly while post-hoc interpretability may need to fix clear objectives.
3. _In some cases transparency may be at odds with the broader aims of AI_. If your regime of interpretability asks us not to use any black boxes at all, we necessarily lose the ability to make systems with even narrowly superhuman performance. 
4. _Post-hoc interpretations can mislead_. Under pressure from demands to placate human preferences, engineers could find themselves optimizing for misleading but plausible results. 

### Future work and contributions of this paper

Areas of potential (as of this writing 2017) 
- **richer loss functions and performance metrics** to "mitigate the discrepancy between real-life and ML objectives" (e.g. sparsity-inducing regularizers and cost-sensitive training)
- **Reinforcement learning** can directly model interaction between models and environment. 
- **Fairness' bottleneck is articulating precise definition of success** so ML researchers may not be able to strictly help (unless they're also involved in whatever social process ought to be articulating fairness' definition of success). 

"Interpretability" is unified under no definition. The connection between interpretability and contestability is under-addressed. This paper provides a taxonomy to proceed. It's desiderata and methods _classify_ past work and _point at_ future work. 

## Quinn's opinion

- I think trust doesn't belong in the desiderata. Trust is property of the social process in which a model is embedded, not a property of the model. 
- it's interesting to claim that transferability (which I think sounds a lot like OODR) is a requisite of interpretability. It seems unrelated to me. I don't know if goodharting (which presents itself as a special class of adversarial OODR failure) is really so antisynchronized with interpretability.
- I missed why exactly NNs have a "clear advantage" in post-hoc explanation. 
