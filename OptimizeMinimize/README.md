#scipy.optimize.minimize  
  
method : str or callable, optional
Type of solver. Should be one of

‘Nelder-Mead’ (see here)
‘Powell’ (see here)
‘CG’ (see here)
‘BFGS’ (see here)
‘Newton-CG’ (see here)
‘L-BFGS-B’ (see here)
‘TNC’ (see here)
‘COBYLA’ (see here)
‘SLSQP’ (see here)
‘trust-constr’(see here)
‘dogleg’ (see here)
‘trust-ncg’ (see here)
‘trust-exact’ (see here)
‘trust-krylov’ (see here)
custom - a callable object (added in version 0.14.0), see below for description.
If not given, chosen to be one of BFGS, L-BFGS-B, SLSQP, depending if the problem has constraints or bounds.  
  
  
    
      
##Notes  


This section describes the available solvers that can be selected by the ‘method’ parameter. The default method is BFGS.

Unconstrained minimization

Method Nelder-Mead uses the Simplex algorithm [1], [2]. This algorithm is robust in many applications. However, if numerical computation of derivative can be trusted, other algorithms using the first and/or second derivatives information might be preferred for their better performance in general.

Method Powell is a modification of Powell’s method [3], [4] which is a conjugate direction method. It performs sequential one-dimensional minimizations along each vector of the directions set (direc field in options and info), which is updated at each iteration of the main minimization loop. The function need not be differentiable, and no derivatives are taken.

Method CG uses a nonlinear conjugate gradient algorithm by Polak and Ribiere, a variant of the Fletcher-Reeves method described in [5] pp. 120-122. Only the first derivatives are used.

Method BFGS uses the quasi-Newton method of Broyden, Fletcher, Goldfarb, and Shanno (BFGS) [5] pp. 136. It uses the first derivatives only. BFGS has proven good performance even for non-smooth optimizations. This method also returns an approximation of the Hessian inverse, stored as hess_inv in the OptimizeResult object.

Method Newton-CG uses a Newton-CG algorithm [5] pp. 168 (also known as the truncated Newton method). It uses a CG method to the compute the search direction. See also TNC method for a box-constrained minimization with a similar algorithm. Suitable for large-scale problems.

Method dogleg uses the dog-leg trust-region algorithm [5] for unconstrained minimization. This algorithm requires the gradient and Hessian; furthermore the Hessian is required to be positive definite.

Method trust-ncg uses the Newton conjugate gradient trust-region algorithm [5] for unconstrained minimization. This algorithm requires the gradient and either the Hessian or a function that computes the product of the Hessian with a given vector. Suitable for large-scale problems.

Method trust-krylov uses the Newton GLTR trust-region algorithm [14], [15] for unconstrained minimization. This algorithm requires the gradient and either the Hessian or a function that computes the product of the Hessian with a given vector. Suitable for large-scale problems. On indefinite problems it requires usually less iterations than the trust-ncg method and is recommended for medium and large-scale problems.

Method trust-exact is a trust-region method for unconstrained minimization in which quadratic subproblems are solved almost exactly [13]. This algorithm requires the gradient and the Hessian (which is not required to be positive definite). It is, in many situations, the Newton method to converge in fewer iteraction and the most recommended for small and medium-size problems.

Bound-Constrained minimization

Method L-BFGS-B uses the L-BFGS-B algorithm [6], [7] for bound constrained minimization.

Method TNC uses a truncated Newton algorithm [5], [8] to minimize a function with variables subject to bounds. This algorithm uses gradient information; it is also called Newton Conjugate-Gradient. It differs from the Newton-CG method described above as it wraps a C implementation and allows each variable to be given upper and lower bounds.

Constrained Minimization

Method COBYLA uses the Constrained Optimization BY Linear Approximation (COBYLA) method [9], [10], [11]. The algorithm is based on linear approximations to the objective function and each constraint. The method wraps a FORTRAN implementation of the algorithm. The constraints functions ‘fun’ may return either a single number or an array or list of numbers.

Method SLSQP uses Sequential Least SQuares Programming to minimize a function of several variables with any combination of bounds, equality and inequality constraints. The method wraps the SLSQP Optimization subroutine originally implemented by Dieter Kraft [12]. Note that the wrapper handles infinite values in bounds by converting them into large floating values.

Method trust-constr is a trust-region algorithm for constrained optimization. It swiches between two implementations depending on the problem definition. It is the most versatile constrained minimization algorithm implemented in SciPy and the most appropriate for large-scale problems. For equality constrained problems it is an implementation of Byrd-Omojokun Trust-Region SQP method described in [17] and in [5], p. 549. When inequality constraints are imposed as well, it swiches to the trust-region interior point method described in [16]. This interior point algorithm, in turn, solves inequality constraints by introducing slack variables and solving a sequence of equality-constrained barrier problems for progressively smaller values of the barrier parameter. The previously described equality constrained SQP method is used to solve the subproblems with increasing levels of accuracy as the iterate gets closer to a solution.

Finite-Difference Options

For Method trust-constr the gradient and the Hessian may be approximated using three finite-difference schemes: {‘2-point’, ‘3-point’, ‘cs’}. The scheme ‘cs’ is, potentially, the most accurate but it requires the function to correctly handles complex inputs and to be differentiable in the complex plane. The scheme ‘3-point’ is more accurate than ‘2-point’ but requires twice as much operations.

Custom minimizers

It may be useful to pass a custom minimization method, for example when using a frontend to this method such as scipy.optimize.basinhopping or a different library. You can simply pass a callable as the method parameter.

The callable is called as method(fun, x0, args, **kwargs, **options) where kwargs corresponds to any other parameters passed to minimize (such as callback, hess, etc.), except the options dict, which has its contents also passed as method parameters pair by pair. Also, if jac has been passed as a bool type, jac and fun are mangled so that fun returns just the function values and jac is converted to a function returning the Jacobian. The method shall return an OptimizeResult object.

The provided method callable must be able to accept (and possibly ignore) arbitrary parameters; the set of parameters accepted by minimize may expand in future versions and then these parameters will be passed to the method. You can find an example in the scipy.optimize tutorial.

New in version 0.11.0.
