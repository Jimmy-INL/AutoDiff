# AutoDiff
Automatic Differentiation via Contour Integration

## Motivation: 

There has previously been some back-and-forth among scientists about whether biological networks such as brains
might compute derivatives. The standard counter-argument is that backpropagation isn't biologically plausible
but partial derivatives are very useful for closed-loop control so we are faced with a fundamental question we
can't ignore. How might large branching structures in the brain and other biological systems compute derivatives?

After some reflection I realised that an important result in complex analysis due to Cauchy, the Cauchy Integral Formula, 
may be used to compute derivatives with a simple forward propagation of signals using a monte-carlo method.

**Minimal implementation in the Julia language:**

```julia
function mc_nabla(f, x::Float64, delta::Float64)

  ## automatic differentiation of holomorphic functions in a single complex variable
  ## applied to real-valued functions in a single variable

  N = round(Int,2*pi/delta)

  ## sample with only half the number of points: 
  sample = rand(1:N,round(Int,N/2)) 
  thetas = sample*delta

  ## collect arguments and rotations: 
  rotations = map(theta -> exp(-im*theta),thetas)
  arguments = x .+ conj.(rotations)  

  ## calculate expectation: 
  expectation = 1.0/(2*pi)*real(sum(map(f,arguments).*rotations))*delta

  return expectation

end
```

**Blog post:**

https://keplerlounge.com/neural-computation/2020/01/16/complex-auto-diff.html

**Jupyter Notebook:**

https://github.com/AidanRocke/AutoDiff/blob/master/cauchy_tutorial.ipynb

