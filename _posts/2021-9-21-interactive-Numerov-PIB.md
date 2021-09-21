---
layout: post
comments: true
title: Tutorial 15. Interactive -- Solve Schrödinger Eq Numerically with Numerov method
---
### Learning objectives
* Learn the basics about Numerov method
* Try the interactive python code to solve particle in a box problem numerically
* Understand how grid size can influence the accuracy of grid-based methods.

## Background

Analytical solution to the Schrödinger equation is only available for a few model systems. Most systems need to be solved with numerical methods. One type of numerical method is the grid-based method.


There is an excellent introduction to grid-based methods in the online open-source textbook [<span style="color:blue">**LibreTexts**</span>](https://chem.libretexts.org/Bookshelves/Physical_and_Theoretical_Chemistry_Textbook_Maps/Time_Dependent_Quantum_Mechanics_and_Spectroscopy_(Tokmakoff)/01%3A_Overview_of_Time-Independent_Quantum_Mechanics/1.05%3A_Numerically_Solving_the_Schrodinger_Equation). The [<span style="color:blue">**article**</span>](https://chem.libretexts.org/Bookshelves/Physical_and_Theoretical_Chemistry_Textbook_Maps/Time_Dependent_Quantum_Mechanics_and_Spectroscopy_(Tokmakoff)/01%3A_Overview_of_Time-Independent_Quantum_Mechanics/1.05%3A_Numerically_Solving_the_Schrodinger_Equation) by Prof. Andrei Tokmakoff of the University of Chicago summarized the basic concepts about two grid-based methods: the Numerov method and the Discrete Variable Representation (DVR). Please read that online article to learn about the basic equations used in Numerov method.

Intuitively, the Numerov method is based on finite difference approximation, like other grid-based methods for solving PDE and ODE. It gives us an equation that relates the function value at three neighboring grid points. Given the function value at two initial points, we can propagate and get the function values at the rest grid points one-by-one. We can propagate from the left boundary or the right boundary and get two numerical solutions (let's call them $\psi_\mathrm{left}$ and $\psi_\mathrm{right}$), as shown in the videos below.

* Run Numerov method from left:
<iframe width="425" src="/images/tutorial15/numerov_left.mp4" frameborder="0" loop ></iframe>


* Run Numerov method from right:
<iframe width="425" src="/images/tutorial15/numerov_right.mp4" frameborder="0" loop ></iframe>


This interactive tutorial will use the Numerov method to solve the particle-in-a-box problem in 1 dimension. In the next tutorial, we will use Numerov to solve the Harmonic Oscillator.

### Solution
In this example, we use atomic units. Then $\hbar=1$. Mass unit is electron mass $m_e$=1. The energy unit is Hartree $E_h$=1. The length unit is bohr.

For simplicity, we assume that we work on a particle in a box problem where the particle mass $m=1$ ($m_e$), box length L=1 (bohr), and the range inside the box is $x\in [0,1]$. Hence, we have

$$-\frac{\hbar^2}{2m}\psi''=E\psi$$

for $x\in [0,1]$.

Boundary conditions:

$$\psi(0)=0$$

$$\psi(1)=0$$

**For simplicity**, assume we already know the energy of the first eigenstate is

$$\frac{n^2h^2}{8mL^2}=\frac{(2\pi\hbar)^2n^2}{8mL^2}=\frac{\pi^2}{2}$$

(in bohr). Let's use Numerov to solve the wave function for the first eigenstate. (In our next tutorial for Harmonic Oscillator, we will show how to search for the eigenvalues (energies) if we don't know them in advance.)

The Schrödinger equation can be simplified as

$$ \psi''=-\pi^2 \psi$$

Based on Numerov method, the Schrödinger equation of the following form

$$\psi''=-k^2(x) \psi$$

has the finite difference approximation

$$\psi(x_{n+1}) = 2\psi(x_n)-\psi(x_{n-1})-\delta^2k^2(x_n)\psi(x_n)$$

and in this case $k^2(x_n)=\pi^2$

Writing the code is straightforward, except for one immediate problem:


**Q:** Whether we start the propagation from the left or the right boundary, we only know $\psi(x_0)$, but the propagation needs two known points $\psi(x_0)$ and $\psi(x_1)$ to get the 3rd point $\psi(x_2)$.
**A:** We can choose an arbitrary positive value for $\psi(x_1)$, and later we can normalize the wave function.

## Try and learn

### Run the following code to explore how the grid size can influence the result

**Instructions**
* First, click "Activate" to activate the code block.
* Once you see the buttons to "run" at the bottom left corner of the code block, click "run" to run the code.
* Please be patient. Starting the kernel can be slow sometimes.
* You will see a plot for the numerical solution of the PIB ground state.
* Drag the slider bar to adjust the grid size (how many discrete points to use in the range [0,1])

* Then answer the questions below
    1. Do the numerical solutions obtained from propagating from the left and from the right match with each other? Do they match when the number of grid points is small (say 3)? Do they match when the number of grid points is large (say >50)?
    1. Have you noticed that the y-axis scale is changing? It's because the $\psi$ value at the send initial point is guessed, and the numerical solution is not normalized. How can we handle that?
    1. Read the title of the plot. How does the runtime change with grid size?


<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.css" integrity="sha512-5A8nwdMOWrSz20fDsjczgUidUBR8liPYU+WymTZP1lmY9G6Oc7HlZv156XqnsgNUzTyMefFTcsFH/tnJE/+xBg==" crossorigin="anonymous" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.4/require.min.js"></script>

<script type="text/x-thebe-config">
  {
    requestKernel: true,
    binderOptions: {
      repo: "matplotlib/ipympl",
      ref: "0.6.1",
      repoProvider: "github",
    },
  }
</script>
<script src="https://unpkg.com/thebe@latest/lib/index.js"></script>

<button id="activateButton" style="width: 120px; height: 40px; font-size: 1.5em;">
  Activate
</button>
<script>
var bootstrapThebe = function() {
    thebelab.bootstrap();
}
document.querySelector("#activateButton").addEventListener('click', bootstrapThebe)
</script>

<pre data-executable="true" data-language="python">
{% highlight python %}
%matplotlib widget
import ipywidgets as widgets
import numpy as np
import matplotlib.pyplot as plt
import time

class NumerovSolverPIB:
    def __init__(self, xlower, xupper, npoints=1000):
        self.xlower = xlower
        self.xupper = xupper
        self.npoints = npoints
        self.x = np.linspace(self.xlower,self.xupper,self.npoints)
        self.delta = self.x[1]-self.x[0]
        self.x1 = 1e-2 # A small positive value as the initial guess the psi value at the second grid point
        self.psi_left = None
        self.psi_right = None
        self.k2 = np.pi**2
        self.prob_left = None
        self.prob_right = None

    def propagateNumerov(self,x0,x1,psi0,psi1):
        psi2 = 2*psi1 - psi0 - self.k2*psi1*self.delta**2
        return psi2

    def Numerov_left(self):
        self.psi_left = np.zeros(len(self.x))
        self.psi_left[1] = self.x1
        for i in range(1,len(self.x)-1):
            self.psi_left[i+1] = self.propagateNumerov(self.x[i-1],self.x[i],self.psi_left[i-1],self.psi_left[i])

    def Numerov_right(self):
        self.psi_right = np.zeros(len(self.x))
        self.psi_right[-2] = self.x1
        for i in range(len(self.x)-2,0,-1):
            self.psi_right[i-1] = self.propagateNumerov(self.x[i+1],self.x[i],self.psi_right[i+1],self.psi_right[i])

# Number of discrete points to use in the range
n_options = widgets.IntSlider(
    value=10,
    min=3,
    max=101,
    step=5,
    description=r'npoints:',
    disabled=False,
)

fig1=plt.figure()
@widgets.interact(npoints=n_options)
def update(npoints):
    plt.clf()
    solver=NumerovSolverPIB(0,1,npoints)
    start=time.time()
    solver.Numerov_left()
    solver.Numerov_right()
    end=time.time()
    timetaken=end-start

    # The wave funciton is not normalized.
    prob = np.trapz(np.power(solver.psi_left,2),solver.x)
    plt.title("Numerov solution to PIB\n" + "Time taken: {:.2f} ms".format(timetaken*1000))

    # plot both the lines and points to make it visually better
    plt.plot(solver.x, solver.psi_left, c='b',label=r'$\psi_\mathrm{left}$')
    plt.scatter(solver.x, solver.psi_left, c='b')

    plt.plot(solver.x, solver.psi_right, c='r',label=r'$\psi_\mathrm{right}$')
    plt.scatter(solver.x, solver.psi_right, c='r')
    plt.xlabel(r'$x$ (bohr)')
    plt.ylabel(r'$\psi(x)$')
    plt.legend()

{% endhighlight %}
</pre>

## Let's investigate the integral of the probability density and see how to normalize the wave function
To do this, we are going to use numerical integration of the probability density corresponding to the numerically solved wave function. Please look at the lines below " # Let's calculate the numerical integral of psi**2". We use the Numpy function ```trapz``` to do the numerical integration.

**Instructions**
* Run the following code block in your browser.
* Drag the slider bar to adjust the grid size (how many discrete points to use in the range [0,1]).
* Read the title of the graph and see what the integral of probability density is. Is it 1 (normalized)?

<pre data-executable="true" data-language="python">
{% highlight python %}
fig2=plt.figure()
@widgets.interact(npoints=n_options)
def update2(npoints):
    plt.clf()
    solver=NumerovSolverPIB(0,1,npoints)
    start=time.time()
    solver.Numerov_left()
    solver.Numerov_right()
    end=time.time()
    timetaken=end-start

    # Let's calculate the numerical integral of psi**2
    prob_left = np.trapz(np.power(solver.psi_left,2),solver.x)
    prob_right = np.trapz(np.power(solver.psi_right,2),solver.x)
    plt.title("Numerov solution to PIB, "
              + r"$\int\psi_\mathrm{left}$=" + "{:.2g}".format(prob_left)
              + r", $\int\psi_\mathrm{right}=$" + "{:.2g}".format(prob_right)
              + "\nTime taken: {:.2f} ms".format(timetaken*1000))

    # plot both the lines and points to make it visually better
    plt.plot(solver.x, solver.psi_left, c='b',label=r'$\psi_\mathrm{left}$')
    plt.scatter(solver.x, solver.psi_left, c='b')

    plt.plot(solver.x, solver.psi_right, c='r',label=r'$\psi_\mathrm{right}$')
    plt.scatter(solver.x, solver.psi_right, c='r')
    plt.xlabel(r'$x$ (bohr)')
    plt.ylabel(r'$\psi(x)$')
    plt.legend()
{% endhighlight %}
</pre>

### Now, let's write a better version of Numerov Solver where we normalize the wave functions
**Instructions**
* Please look for the lines below ```# Calculate the integral of probability distribution``` and see how the normalization is done.
* Drag the slider bar to adjust the grid size (how many discrete points to use in the range [0,1]).
* Read the title of the graph and see what the integral of probability density is. Is it 1 (normalized)?
* The exact analytical solution is also plotted in the figure (black line). Start with the most sparse grid: npoint=3, gradually increase the number of grid points and see how well the numerical result matches the exact solution.

<pre data-executable="true" data-language="python">
{% highlight python %}
# An improved version that handles normalization
class NumerovSolverPIB_v2:
    def __init__(self, xlower, xupper, npoints=1000):
        self.xlower = xlower
        self.xupper = xupper
        self.npoints = npoints
        self.x = np.linspace(self.xlower,self.xupper,self.npoints)
        self.delta = self.x[1]-self.x[0]
        self.x1 = 1e-2 # A small positive value as the initial guess the psi value at the second grid point
        self.psi_left = None
        self.psi_right = None
        self.k2 = np.pi**2
        self.prob_left = None
        self.prob_right = None

    def propagateNumerov(self,x0,x1,psi0,psi1):
        psi2 = 2*psi1 - psi0 - self.k2*psi1*self.delta**2
        return psi2

    def Numerov_left(self):
        self.psi_left = np.zeros(len(self.x))
        self.psi_left[1] = self.x1
        for i in range(1,len(self.x)-1):
            self.psi_left[i+1] = self.propagateNumerov(self.x[i-1],self.x[i],self.psi_left[i-1],self.psi_left[i])
        # Calculate the integral of probability distribution
        self.prob_left = np.trapz(np.power(self.psi_left,2),self.x)
        # Normalize the function
        self.psi_left = self.psi_left/np.sqrt(self.prob_left)

    def Numerov_right(self):
        self.psi_right = np.zeros(len(self.x))
        self.psi_right[-2] = self.x1
        for i in range(len(self.x)-2,0,-1):
            self.psi_right[i-1] = self.propagateNumerov(self.x[i+1],self.x[i],self.psi_right[i+1],self.psi_right[i])
        # Calculate the integral of probability distribution
        self.prob_right = np.trapz(np.power(self.psi_right,2),self.x)
        # Normalize the function
        self.psi_right = self.psi_right/np.sqrt(self.prob_right)
fig3=plt.figure()
@widgets.interact(npoints=n_options)
def update3(npoints):
    plt.clf()
    solver=NumerovSolverPIB_v2(0,1,npoints)
    start=time.time()
    solver.Numerov_left()
    solver.Numerov_right()
    end=time.time()
    timetaken=end-start

    # The wave funciton is not normalized.
    prob_left = np.trapz(np.power(solver.psi_left,2),solver.x)
    prob_right = np.trapz(np.power(solver.psi_right,2),solver.x)
    plt.title("Numerov solution to PIB, "
              + r"$\int\psi_\mathrm{left}$=" + "{:.2g}".format(prob_left)
              + r", $\int\psi_\mathrm{right}=$" + "{:.2g}".format(prob_right)
              + "\nTime taken: {:.2f} ms".format(timetaken*1000))

    # plot both the lines and points to make it visually better
    plt.plot(solver.x, solver.psi_left, c='b',label=r'$\psi_\mathrm{left}$')
    plt.scatter(solver.x, solver.psi_left, c='b')

    plt.plot(solver.x, solver.psi_right, c='r',label=r'$\psi_\mathrm{right}$')
    plt.scatter(solver.x, solver.psi_right, c='r')

    # Plot the reference analytical solution
    x = np.linspace(0,1,1000)
    psi = np.sqrt(2)*np.sin(np.pi*x)
    plt.plot(x, psi, c='k',label=r'$\psi$')

    plt.xlabel(r'$x$ (bohr)')
    plt.ylabel(r'$\psi(x)$')
    plt.legend()
{% endhighlight %}
</pre>

## Take home messages
* "Rome wasn't built in a day." Even for this simple example of Numerov solution to PIB, we identify problems and solve them one by one to get a good numerical solution.
* For grid-based numerical methods, the grid size matters!
* There is a trade-off between the accuracy and computational costs for choosing grid size.
