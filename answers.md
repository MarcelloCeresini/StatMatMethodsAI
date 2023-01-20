# Trying to answer to "Questions of the Oral Exam"

## Linear Algebra

- Random Matrix:
  - The behavior seems erratic, with a slow growth when n increases.
  - The condition number in 2-norm and the one in ∞-norm are correlated, they grow and decrease together, but the one with ∞-norm is bigger. They are correlated because both are the product of the norm of a matrix by the norm of its inverse, with different norms. Since all matrix norms are equivalent, they will always differ maximum by a multiplicative constant.
  - The definition of ill-conditioning states that a matrix is ill-conditioned if its condition number is large, where "too large" means roughly log(k)> the precision of matrix entries. Given what we stated above, the definition of ill-conditioning is a property of the problem, not of the specific norm used to compute the condition number.
  - There is a strictly positive relationship between the condition number and the relative error of the solution.
- Vandermonde Matrix:
  - The behavior is completely different from the random matrix: the condition number grows exponentially until it reaches a value of ~ 1e22 and then plateaus.
- Hilbert Matrix:
  - The behaviro is very similar to the one of the vandermonde matrix, but the plateau stops at ~1e20. Both are ill-conditioned matrices

## PCA and LDA

- From the two 2d plots, LDA has a much stronger ability to separate clusters. The reason is that it also exploits the labels of the data to increase between-cluster distances and decrease the inter-cluster distances. PCA instead only can exploit the fact that some combination of features has more relevance and differenciates better the data, without even knowing how many clusters are there.
- The average distances are completely out of scale, PCA ~ 500 and LDA ~ 0.01. The reason is that PCA does not normalize the data, only projects them on their principal components given by the eigenvectors.
- The LDA classifier has ~98% accuracy, while the PCA one has ~84% accuracy. This is because since in PCA the clusters are not well separated, it's more probable that when a new point is projected on its main components it falls closer to another class's centroid than its own.
- The accuracy of PCA grows for growing k, while LDA accuracy does not. The first is explained by the fact that it's probable that the more directions are allowed, the more the projections separate well the data. It's more probable that a combination of more principal directions place far away different labeled samples. LDA in the meanwhile builds a supervised projection matrix that manages to extract all the relevant information about clusters in its firsts components, so even if we add directions the accuracy does not increase. Moreover, it cannot increase monotonically because it would mean that taking all the directions (k=dimensionality of input) would give the best results, that is not the case: the last directions are less and less informative and only introduce noise.

- In SVD, if you compare k-rank dyads for growing k you can see that the first ones are much higher contrast, and seem to represent/reflect more high level features, like light/dark zones in the image, while the later ones seem to be more and more noisy, probably accounting for smaller and smaller details.
- The singular value is the weight term in the weighed sum for the riconstruction of the compressed version of an image. So, the bigger the singular value, the more meaning that dyad has in the image.
- The approximation error decays as fast as the singular values decay. This means that it is true that the first dyads have a bigger importance in the image, because when they are added, the resulting compressed image is much more similar to the real one.

## Optimization

- GD without backtracking has very different behaviors depending on the functions being optimized. In particular, for convex and symmetric functions, since the gradient is always in the direction of the global minimum, any (non astronomical) value of $\alpha$ will make GD converge, faster if alpha is bigger. In non symmetric functions instead, expecially where the loss landscape is "valley-like", non-backtracing GD shows its deficiencies: for low alpha values it's difficult for it to converge, but for high ones it starts bouncing back and forth on the ridges of the valley. These problems are solved with backtracking, that reduces the step size until the function actually lowers. In fact, in all cases backtracking converges without many problems, and actually faster than many of the others because it uses large steps where the curvature is constant.
- The convergence speed is in all cases linear, and the reason is that the gradient becomes lower and lower near zero.
- In function 1 convergence is achieved before if the starting point is closer to the real minimum, the tolerances are higher (but accuracy will suffer) and with higher step size.
- In function 2 the choice of the starting iterate has a much bigger impact: the closer it is to the center of the valley, the easier convergence will be. Here higher step sizes not always allow convergence.
- In function 3 there is another variable: the dimensionality of the problem. In this case, since the $A$ matrix is an ill-conditioned matrix, the n-dimensional space created with it will not have a well-behaved curvature. In fact we can see that while for n=5 normal GD with step size =1e-1 converges faster than any other combination, at n=10 instead the same step size causes divergence. This can be caused by the fact that with growing n the vandermonde matrix gets terms in the order of $x^n$, creating higher and higher ridges with bigger gradients.
Backtracking always converges in less than 100 steps, but it needs more and more steps, probably because it's more difficult to navigate higher dimensional ill-conditioned spaces.
- In function 4 we introduce a regularization parameter that helps to smooth out the valleys, and in fact the standard GD converges more easily. We can also see that with higher lambda convergence is easier, and fixed-step methods seem to be more advantaged.
- In function 5 we can see one of the biggest disadvantages of GD: the fact that it always converges to (the nearest) local minimum. This means that if a function isn't convex the starting point highly influences the result.

- The logistic regression classifier highly benefits from higher training set dimensions (with diminishing returns). However, when the training set is too big with respect to the test set, the variance in the data can bring down the accuracy.
- When you change the digits that need to be classified, accuracies vary (even if in a small way). Most probably similar digits (as [2,3]) are more difficult to classify.
- We can see that SGD takes less epochs with respect to the standard GD to get lower losses and also higher accuracies. This means that SGD is simply more efficient in its data use, and definately faster than GD. The problem is that when the batch_size becomes too small the gradient updates become erratic and risk deviating too much from the correct path to the minimum. This is mainly a problem in non convex or ill-posed problems.
- The accuracy of both SGD and GD is around 97% for digits [2,3], while PCA is around 90%, and LDA is 97%
- Three digits requires a different model (softmax instead of sigmoid) and gradient (k-dimensional instead of 1-dimensional for each sample). For digits [0,6,9], PCA scores lowest at 93%, LDA at 97% and the regressor is almost at 99%.

## MLE and MAP

- When theta is the MLE solution under gaussian assumptions, we can see that both the test and training error are high for low k (simply the model cannot fit the data --> underfitting), then decrease for k~k_true, and then the training error decreases while the test error increases (overfitting). Finally the error diverges to high values, meaning that there are some numerical problems in the fit of the curve.
- For MAP instead there is a much bigger range of k for which the solution actually fits well the data, even when k>>k_true. This is caused by the regularizer effect of the posterior on the parameters.
- Varying lambda can have different effects depending on the relation between k and k_true: for k lower than k_true the effects are neglegible, while for k higher than k_true the effects are drastic: for example for k>>k_true while the theta found by MLE diverges and cannot fit the data, even adding a lambda=1e-3 allows to fit the data very well.
- When k increases, the relative error on the weights found by MLE increases a lot, while MAP stays relatively lower for much higher values of k.
- For increasing N_train we can see that the relative error on the weights is not strictly affected, but the distance between train error and test error reduces a lot, meaning that the generalization capabilities of the model are higher.
- The solutions computed by iterative methods like GD and SGD, when compared to the ones found by MLE, are much more stable with growing k.

- For poisson noise we can see that the behavior is similar, but it is more sensitive to the variation in k.
- MAP in this case can alleviate less the problems with growing k, but still does a better job in allowing the higher complexity model to avoid divergence and overfitting
- Same thing applies as before when varying lambda, but results are less stable, meaning that probably poisson noise is more difficult to account for
- If we increase the N_train points we see that the difference between train and test loss stays nearly the same, decreasing less than in the case of gaussian noise. Maybe it is because poisson noise has a variance that dependes on the x(?)
- In this case gradient descent methods are NOT more stable than direct methods, but when they do converge they tend to have lower error with respect to the direct methods.
