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
- The accuracy of PCA grows for growing k, while LDA accuracy does not. The first is explained by the fact that it's probable that the more directions are allowed, the more the projections 

ciccio

finished.
