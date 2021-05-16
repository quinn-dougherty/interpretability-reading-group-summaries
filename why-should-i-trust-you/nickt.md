# "Why Should I Trust You?" Explaining the Predictions of Any Classifier (a.k.a. LIME)
[paper](https://arxiv.org/abs/1602.04938)

This paper presents our first two concrete attempts to interpret black-box machine learning models: LIME and SP-LIME. LIME (Local Interpretable Model-(a)gnostic Explanations) attempts to explain the local behavior of a black box model around a local neighborhood, and SP-LIME is designed to summarize many local explanations into a more global understanding of a model.

The basic approach to LIME is to fit a surrogate model to a small neighborhood of the predictions of a more complicated model. The surrogate model is designed to be easily interpretable, so users can gain some insight into the local behavior around a particular prediction.

One way that the surrogate model is designed to be interpretable is by operating on a (potentially separate) set of features that a human selects to be interpretable - called "interpretable components". In a bag of words representation, these components might be the presence or absence of a particular word, whereas for images it might be superpixels that are of the appropriate size to give some intuition to humans. The LIME comes up with a way to transform a presence or absence of these interpretable components into new inputs for the original model that they are trying to interpret. The job of the surrogate model is to relate these interpretable components to the output of the complex model.

The surrogate model is also selected to be one that is much easier to interpret than the original complex model. This paper selects lasso regression models with K maximum nonzero weights (K-LASSO) as the surrogate models to use for all of their experiments. Fitting the surrogate model involves perturbing the input that the user would like to explain using the interpretable component mappings to the original feature space several times, and performing a weighted regression of the interpretable components on the predictions of the model. The weights for the regression are defined by a local neighborhood weighting function (here, a squared exponential kernel around the original feature vector of the original sample). The coefficients or weights of this sparse linear model convey an intuition about which components are used for the prediction around the original sample.

SP-LIME attempts to summarize potentially many LIME explanations to give a sense of the global model structure across many local neighborhoods. To do this, they take the weight vectors across different LIME solutions, and try to select a set of explanations that use each of the "important" interpretable components at least once. Interpretable components are considered important if they play a large role in the explanations of several local neighborhoods.

The authors perform several basic experiments to show that LIME fulfills some basic requirements of a model explanation. For each experiment, they fit several models to two sentiment analysis datasets and try to summarize them. They train models restricted to a subset of possible features, and recover these features using the LIME explanations. They randomly label some features as "untrustworthy", and are able to find the untrustworthy predictions that use these features using the LIME explanations. They simulate a model selection task by tweaking their dataset to encourage a type of overfitting, and they are able to select the model that is less overfit. They recruit amazon mechanical turkers (AMTs) to perform a version of a similar model selection task. They have the AMTs improve the model by selecting features to remove using the explanations.

Finally, they purposefully train a misleading model, get AMTs to originally believe it, and then destroy this trust in the model by showing them the model explanations. This example is commonly cited in subsequent work so it's worth some extra summary. The task is to classify huskies vs. wolves within natural images, but they train the classifier to perform this prediction based on whether there is snow in the image. Originally the AMTs believe the model is performing classification correctly, even though they are shown an incorrect prediction where this heuristic fails. Showing the AMTs the explanations demonstrates how the model is performing the task and saves the day.

### NICK'S OPINION
* The notion of "covering" the important variables as a criterion for global understanding seems pretty weak.
* This paper kind of oversells the general nature of SP-LIME, maybe something like a decision tree would be better to consolidate the explanations (ditching coverage)
* They use pretty easy use cases overall. This isn't a fault of the paper. This is a proof-of-principle paper, but I'd be interested to see how this would work to approximate more complicated models in situations where the variables of interest aren't so clear.
* Local approximation might fail if there isn't much variation in the predictions around your selected example
* They mention that it's possible to specify how well the local approximation fits the actual model, but they don't seem to present this anywhere, what would this look like, and how would this metric perform?
