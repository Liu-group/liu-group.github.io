---
layout: post
comments: true
title: Tutorial 13. Interactive -- Harmonic Oscillator in 1D
---

### Learning objectives
* Try the interactive python code to plot the wave function of Harmonic Oscillator (1D).
* Play with the different parameters and try answering the questions asked at the end of this tutorial.

## Background
1D harmonic oscillator is a simple, but very important model for chemists, because its relashion with diatomic molecule vibration. The potential of the 1D harmonic oscillator is a quadratic function with respect to the position $x$:

$$ V(x) = \frac{1}{2} m\omega^2 x^2 $$

where $m$ is the mass of the harmonic oscillator, $\omega=\sqrt{\frac{k}{m}}$ is the angular frequency, and $k$ is the force constant.

### Solution
The eigenvalues of the Hamiltonian of 1D Harmonic oscillator are very simple:

$$ E_v = \hbar \omega (v+\frac{1}{2}),\ \ v=0,1,2,\dots $$,
where $v$ is an integer quantum number.

However, the corresponding eigenfunctions are relatively complicated:

$$\psi_v(x) = \frac{1}{(2^vv!)^{\frac{1}{2}}}(\alpha/\pi)^{\frac{1}{4}}H_v(\sqrt{\alpha x})e^{-\frac{1}{2}\alpha x^2}, \ \ v=0,1,2,\dots$$

where $\alpha$ is defined as

$$ \alpha = \frac{m\omega}{\hbar}$$

and $H_v(x)$ are Hermite polynomials, for which we don't have a intuition about the shapes.

Because the complexity of the formula, visualization is important to deepen the students' understand of the Harmonic Oscillator wave functions.

## Try and learn

Run the following code to see how the Harmonic Oscillator wave function shape changes with the quantum number $v$, angular frequency $\omega$, and mass $m$ (all can be tuned interactively).

**Instructions**
* First, click "Activate" to activate the code block.
* Once you see the buttons to "run" at the bottom left corner of the code block, click "run" to run the code.
* Please be patient. Starting the kernel can be slow sometimes.
* You will see a plot with three panels. The upper is the $v$-th order Hermite polynomial, the middle is $\psi_v$, and the lower is $\vert\psi_v\vert ^2$.
* Play with the slider to select the quantum number you want. $v$ can be any non-negative integer. In this demo, we allow $n$ up to 20.
* Enter the mass (in atomic unit) and angular frequency in the text boxes. In this demo, we allow $m$ and $\omega$ to be any float number between 0.1 and 100 just for demonstration purposes. Theoretically, they can be any positive real number.


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

# Define constants (We use atomic units for everything in this demo)
hbar = 1

# Define the interactive widgets

# Quantum number v can be any integer between 0 and 20
v_options = widgets.IntSlider(
    value=0,
    min=0,
    max=20,
    step=1,
    description=r'$v$:',
    disabled=False,
)

# Max Quantum number vmax to be plotted, can be any integer between 0 and 20
vmax_options = widgets.IntSlider(
    value=4,
    min=0,
    max=20,
    step=1,
    description=r'$v_{max}$:',
    disabled=False,
)

# Angular frequency omega can be any float value between 0.01 and 10 (atomic unit) inputed by the user
omega_options = widgets.BoundedFloatText(
    description=r'$\omega$:',                             
    value=2.0,
    min=0.01,
    max=10.0,
    padding = 8)

# Mass m can be any float value between 0.1 and 10 (atomic unit) inputed by the user
m_options = widgets.BoundedFloatText(
    description=r'$m$:',                             
    value=0.5,
    min=0.01,
    max=10.0,
    padding = 8)

# Function that return hermite polynomial Hv(sqrt(alpha)x) for given m, omega,v
def Hv_func(alpha,v):

    # Set up the Hermite polynomial
    domain = [-1,1]
    window = [-np.sqrt(alpha),np.sqrt(alpha)]
    coeff = [0]*v
    coeff.append(1)
    return np.polynomial.hermite.Hermite(coeff,domain=domain, window=window)

fig,(ax1,ax2,ax3) = plt.subplots(3,sharex=True)
plt.subplots_adjust(hspace=0)

@widgets.interact(v=v_options,m=m_options,omega=omega_options)
def update(v,m,omega):
    ax1.cla()
    ax2.cla()
    ax3.cla()

    # Set the range of x values we want to plot
    npoints=2000
    xlower = -10
    xupper = 10

    # obtain the Hermite polynomial fuction
    alpha= m*omega/hbar    
    Hv = Hv_func(alpha,v)

    # obtain the Hermite polynomial curve data for the range of x values
    x,y=Hv.linspace(npoints,[xlower,xupper])


    # Add a title
    plt.sca(ax1)

    plt.title(r'Harmonic Oscillator'+ '\n'
              r'$m$= '+str(m) + ', $\omega$=' + str(omega) +
              r', $\alpha$=' + str(alpha)+ r', $v$=' + str(v))


    #############################################
    # First work on the Hermite polynomial part #
    #############################################
    # Add y Label
    plt.ylabel(r'$H_v(\sqrt{\alpha}x)$')

    # Plot
    plt.plot(x,y,c='b')
    ymax = 2**(v/2.0)*np.sqrt(float(np.math.factorial(v)))/((alpha/np.pi)**0.25)
    ymin = -ymax
    plt.ylim(ymin,ymax)

    # Set grid
    plt.grid(True)


    #####################
    # Then work on Psi  #
    #####################
    y = 1/(2**(v/2.0)*np.sqrt(float(np.math.factorial(v))))*(alpha/np.pi)**0.25*y*np.exp(-1/2.0*alpha*x**2)

    plt.sca(ax2)

    # Add y Label
    plt.ylabel(r'$\psi(x)$')
    # Plot
    plt.plot(x,y,c='b')   

    # Set grid
    plt.grid(True)

    # Calculate energy of the state with quantum number v
    E = hbar*omega*(v+ 1/2.0)
    # Calculate the positive turning point xt_plus based on E
    xt_plus = np.sqrt(2.0/(m*omega**2)*E)
    xt_minus = -xt_plus  

    # Plot the Classical turning points
    plt.plot([xt_minus, xt_minus],[-1,1],c='k')
    plt.plot([xt_plus,xt_plus],[-1,1],c='k')
    #########################
    # Then  work on Psi**2  #
    #########################

    plt.sca(ax3)
    # Add X and y Label
    plt.xlabel(r'$x$ (bohr)')
    plt.ylabel(r'$|\psi(x)|^2$')

    # Plot
    plt.plot(x,y**2,c='r')
    plt.fill_between(x,y**2, color='r',alpha=0.5)
    plt.ylim(0,1.1)

    # Set grid
    plt.grid(True)

    # Plot the Classical turning points
    plt.plot([xt_minus, xt_minus],[-1,1],c='k')
    plt.plot([xt_plus,xt_plus],[-1,1],c='k')
    plt.text(xt_plus,0.5,'classical turning points')

{% endhighlight %}
</pre>

### Plot the probability distribution together with the energy levels and the potential
After visualizing the individual states, we can do something even more exciting. You might have seen some textbook figures plotting the wave functions (or probability distributions) of multiple states and the harmonic potential together, clearly showing how the wave functions extend beyond the classical forbidden area. We can do the same thing here!

**Instructions**
* Run the following code block in your browser
* You may adjust the number of states to display at the same time by tuning $v_{max}$.


<pre data-executable="true" data-language="python">
{% highlight python %}
fig3=plt.figure()

@widgets.interact(vmax=vmax_options,m=m_options,omega=omega_options)
def update3(vmax,m,omega):
    fig3.clf()
    # Set the range of x values we want to plot
    npoints=2000
    xlower = -10
    xupper = 10


    # Let's first plot the parabola
    x = np.linspace(xlower,xupper,npoints)
    Vx = 0.5*m*omega**2*x**2
    plt.plot(x,Vx,c='k')
    plt.xlim(xlower,xupper)


    #Then plot the wave functions shifted in the y direction   
    alpha= m*omega/hbar  
    Emax = hbar*omega*(vmax+ 1/2.0)

    for v in range(0, vmax+1):
        # Calculate energy of the state with quantum number v
        E = hbar*omega*(v+ 1/2.0)
        # Calculate the positive turning point xt_plus based on E
        xt_plus = np.sqrt(2.0/(m*omega**2)*E)
        xt_minus = -xt_plus
        # Plot the energy level line, intersecting with the parabola
        plt.plot([xt_minus, xt_plus],[E,E],c='k')


        # obtain the Hermite polynomial fuction
        Hv = Hv_func(alpha,v)
        # obtain the Hermite polynomial curve data for the range of x values
        # First obtain y as the Hermite polynomial
        x,y=Hv.linspace(npoints,[xlower,xupper])
        # Add the normalization coefficients and the gaussian funciton
        y = 1/(2**(v/2.0)*np.sqrt(float(np.math.factorial(v))))*(alpha/np.pi)**0.25*y*np.exp(-1/2.0*alpha*x**2)
        # Get probability density
        y = y**2
        # Shift the probability density in y direction to align with the energy level
        y = y + E
        plt.plot(x,y,c='r')
        ybaseline = np.array([E]*npoints)
        plt.fill_between(x,y,ybaseline, color='r',alpha=0.5)       
    plt.ylim(0,Emax+1)

    plt.title(r'Harmonic Oscillator  $|\psi(x)|^2$ aligned with energy levels'+ '\n'
              r'$m$= '+str(m) + ', $\omega$=' + str(omega) +
              r', $\alpha$=' + str(alpha)+ r', $v_{max}$=' + str(vmax))

    plt.xlabel(r'$x$ (bohr)')
    plt.ylabel(r'$E$(H$_a$)')
{% endhighlight %}
</pre>
## Question
* Did you observe certain symmetry in the eigenfunctions?
* Where is the maximum of the probability distribution when $v=0$ or other small numbers?
* Where are the maxima of the probability distribution when $v$ takes large numbers, say 15? How does that relate to the result of classical mechanics?
* As $v$ increases, do you see more probability outside the classical region? Or less?
* Do you know how this analytical solution is obtained?
* Apart from the analytical solution, can you solve this problem numerically with the Numerov method?
