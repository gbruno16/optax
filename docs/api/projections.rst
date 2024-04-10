Projections
===========

.. currentmodule:: optax.projections

Projections can be used to perform constrained optimization.
The Euclidean projection onto a set :math:`\mathcal{C}` is:

.. math::

    \text{proj}_{\mathcal{C}}(u) :=
    \underset{v}{\text{argmin}} ~ ||u - v||^2_2 \textrm{ subject to } v \in \mathcal{C}.

For instance, here is an example how we can project parameters to the non-negative orthant::

    >>> import optax
    >>> import jax
    >>> import jax.numpy as jnp
    >>> num_weights = 2
    >>> xs = jnp.array([[-1.8, 2.2], [-2.0, 1.2]])
    >>> ys = jnp.array([0.5, 0.8])
    >>> optimizer = optax.adam(learning_rate=1e-3)
    >>> params = {'w': jnp.zeros(num_weights)}
    >>> opt_state = optimizer.init(params)
    >>> loss = lambda params, x, y: jnp.mean((params['w'].dot(x) - y) ** 2)
    >>> grads = jax.grad(loss)(params, xs, ys)
    >>> updates, opt_state = optimizer.update(grads, opt_state)
    >>> params = optax.apply_updates(params, updates)
    >>> params = optax.projections.projection_non_negative(params)

Available projections
~~~~~~~~~~~~~~~~~~~~~
.. autosummary::
    projection_box
    projection_hypercube
    projection_non_negative

Projection onto a box
~~~~~~~~~~~~~~~~~~~~~
.. autofunction:: projection_box

Projection onto a hypercube
~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. autofunction:: projection_hypercube

Projection onto the non-negative orthant
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
.. autofunction:: projection_non_negative
