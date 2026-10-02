CorrCal: statistical and numerical background

These references explain how CorrCal's likelihood is built and why its optimization reduces to Ax = b problems that conjugate gradient can solve. CorrCal treats the sky as a Gaussian random field and marginalizes it out, which gives the covariance C = N + G(ΔΔᵀ + ΣΣᵀ)Gᴴ. It then minimizes dᴴC⁻¹d, optionally plus log det C, over the gains. The Woodbury identity makes C⁻¹ cheap because the sky terms are low rank. The objective is non-quadratic in the gains. It connects to Ax = b in two ways. First, nonlinear CG applies the logic of linear CG to a general function. Second, Gauss-Newton linearization produces the normal equations (JᴴC⁻¹J)δg = JᴴC⁻¹r at each step. Read in this order: Shewchuk, R&W, Bishop, Boyd, then Sievers 2017 (arXiv:1701.01860).

Shewchuk, An Introduction to the Conjugate Gradient Method Without the Agonizing PainThis is the bridge between minimizing a quadratic form and solving Ax = b. Relevant sections:

§3 shows that the quadratic form ½xᵀAx − bᵀx is minimized exactly where Ax = b.

§7–9 derive linear CG and its convergence.

§12 covers preconditioning.

§13 applies CG to the normal equations, which is the Gauss-Newton case.

§14 covers nonlinear CG, which the original CorrCal code uses through scipy.

Rasmussen & Williams, Gaussian Processes for Machine LearningThis book has the same statistical structure as CorrCal: a latent Gaussian signal marginalized into the covariance, with parameters fit by maximizing the marginal likelihood. Relevant chapters:

Ch. 2 covers the Gaussian model and the predictive and marginal distributions.

Ch. 5, especially §5.4, covers the marginal likelihood as a quadratic term plus log det, and its gradients with respect to covariance parameters. This is the analog of the gain gradients.

Appendix A.3 covers the matrix identities: Woodbury and the determinant lemma, used for the low-rank inverse.

Bishop, Pattern Recognition and Machine LearningThis is the statistics foundation for linear-Gaussian models. Relevant sections:

§2.3, especially §2.3.3, shows that integrating out a linear-Gaussian latent variable adds its covariance to the noise. This is the core move in CorrCal.

§3.1.1 shows that maximum likelihood under Gaussian noise reduces to least squares normal equations.

§3.5 covers maximizing the evidence over hyperparameters.

Boyd & Vandenberghe, Convex OptimizationThis supplies the estimation and numerical linear algebra side. Relevant sections:

§7.1 covers ML estimation for linear measurements with Gaussian noise, and why the estimate is a prewhitened least squares problem.

Appendix C covers numerical linear algebra, including the matrix inversion lemma and block elimination for low-rank structure.

Not yet saved:

Sievers 2017 (arXiv:1701.01860), the primary CorrCal paper.

Liu et al. 2010 (arXiv:1001.5268), on logcal and lincal as linear least squares.

Tegmark 1997 (astro-ph/9611130), on CMB map-making as a GLS problem solved with preconditioned CG.

Hogg, Bovy & Lang (arXiv:1008.4686), on GLS normal equations.

Kay, Estimation Theory, ch. 3 and 15, for Fisher information and Cramér-Rao bounds on the gains, if needed later.