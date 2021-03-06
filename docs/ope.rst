================================================
Overview
================================================


Setup
------

We consider a general contextual bandit setting.
Let :math:`r \in [0, R_{\mathrm{max}}]` denote a reward or outcome variable (e.g., whether a fashion item as an action results in a click).
We let :math:`x \in \calX` be a context vector (e.g., the user's demographic profile) that the decision maker observes when picking an action.
Rewards and contexts are sampled from the unknown probability distributions :math:`p (r \mid x, a)` and :math:`p(x)`, respectively.
Let :math:`\calA:=\{0,\ldots,m\}` be a finite set of :math:`m+1` actions.
We call a function :math:`\pi: \calX \rightarrow \Delta(\calA)` a *policy*.
It maps each context :math:`x \in \calX` into a distribution over actions, where :math:`\pi (a \mid x)` is the probability of taking action :math:`a` given :math:`x`.

Let :math:`\calD := \{(x_t,a_t,r_t)\}_{t=1}^{T} ` be historical logged bandit feedback with :math:`T` rounds of observations.
:math:`a_t` is a discrete variable indicating which action in :math:`\calA` is chosen in round :math:`t`.
:math:`r_t` and :math:`x_t` denote the reward and the context observed in round :math:`t`, respectively.
We assume that a logged bandit feedback is generated by a behavior policy :math:`\pi_b` as follows:

.. math::
  \{(x_t,a_t,r_t)\}_{i=1}^{T} \sim \prod_{i=1}^{T} p(x_t) \pi_b (a_t \mid x_t) p(r_t \mid x_t, a_t),

where each context-action-reward triplets are sampled independently from the product distribution.
Note that we assume :math:`a_t` is independent of :math:`r_t` conditional on :math:`x_t`.

We let :math:`\pi(x,a,r) := p(x) \pi (a \mid x) p(r \mid x, a)` be the product distribution by a policy :math:`\pi`.
For a function :math:`f(x,a,r)`, we use :math:`\E_{\calD} [f] := |\calD|^{-1} \sum_{(x_t, a_t, r_t) \in \calD} f(x_t, a_t, r_t)` to denote its empirical expectation over :math:`T` observations in :math:`\calD`.
Then, for a function :math:`g(x,a)`, we let :math:`g(x,\pi) := \E_{a \sim \pi(a|x)}[g(x,a) \mid x]`.
We also use :math:`q(x,a) := \E_{r \sim p(r|x,a)} [ r \mid x, a ]` to denote the mean reward function.


Estimation Target
-------------------------
We are interested in using the historical logged bandit data to estimate the following *policy value* of any given *evaluation policy* :math:`\pi_e` which might be different from :math:`\pi_b`:

.. math::
    V (\pi_e) := \E_{(x,a,r) \sim \pi_e (x,a,r)} [r] .

where the last equality uses the independence of :math:`A` and :math:`Y(\cdot)` conditional on :math:`X` and the definition of :math:`\pi_b(\cdot|X)`.
We allow the evaluation policy :math:`\pi_e` to be degenerate, i.e., it may choose a particular action with probability 1.
Estimating :math:`V(\pi_e)` before implementing :math:`\pi_e` in an online environment is valuable because :math:`\pi_e` may perform poorly and damage user satisfaction.
Additionally, it is possible to select an evaluation policy that maximizes the policy value by comparing their estimated performances without having additional implementation cost.
