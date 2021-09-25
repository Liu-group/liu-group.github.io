---
layout: post
comments: false
title: Tutorial 16. Interactive -- Morse Potential and diatomic spectrum database
---

### Learning objectives
* Learn basics about maniputlating datasets with the python package, Pandas
* Learn to fit Morse potential based on experimental vibrational spectrum data
* Try the interactive python code to visualize the Morse potential of low-lying electronic states of diatomic molecules

## Background

In quantum mechanics class, we learn about the Morse potential, which is a widely used approximation to the potential of diatomic molecules.

$$ V(r)=D_e[1-e^{-a(r-r_e)}]^2 $$

How can we relate it to real vibrational spectrum data of diatomic molecules? You might have seen some example from the textbook: given the vibrational spectrum constants $\omega_e$, $\omega_ex_e$ of a diatomic molecule, we can obtain $D_e$ and $a$ in the Morse potential formula, and plot the Morse potential of that molecule.

### Can we do better than just one textbook example?

Yes, we can visualize the Morse potential and the vibrational energy levels of <span style="color:blue">hundreds</span> of diatomic molecules in <span style="color:blue">seconds</span>! That's simple with the following recipe:

* An open-access diatomic spectrum dataset
* Some basic knowledge about dataset manipulation
* Some python code to draw Morse potential curves

These ingredients become available to us only in this era of <span style="color:blue">big data</span>.

## Try and learn

### Run the following code to visualize Morse potentials of hundreds of diatomic molecules based on spectrum data obtained from diatomic database. 

**Instructions**
* First, click "Activate" to activate the code block.
* Once you see the buttons to "run" at the bottom left corner of the code block, click "run" to run the code.
* Please be patient. Starting the kernel can be slow sometimes.
* You will see a plot for the Morse potential of OH (two low lying electronic states).
* Select other diatomic molecules from the dropdown menu to visualize their Morse potentials.

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/4.7.0/css/font-awesome.css" integrity="sha512-5A8nwdMOWrSz20fDsjczgUidUBR8liPYU+WymTZP1lmY9G6Oc7HlZv156XqnsgNUzTyMefFTcsFH/tnJE/+xBg==" crossorigin="anonymous" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/require.js/2.3.4/require.min.js"></script>

<script type="text/x-thebe-config">
  {
    requestKernel: true,
    binderOptions: {
      repo: "ffangliu/interactive_tutorial_env",
      ref: "main",
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

###########################################
#  First load the dataset
# The dataset is downloaded from: https://rios.mp.fhi.mpg.de/
# Citation: Liu, X., Truppe, S., Meijer, G. et al. The diatomic molecular spectroscopy database. J Cheminform 12, 31 (2020). https://doi.org/10.1186/s13321-020-00433-8
############################################
!wget --trust-server-names https://rios.mp.fhi.mpg.de/export_table.php -O moleculedata.csv

%matplotlib widget
import ipywidgets as widgets
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib as mpl
from scipy.special import factorial
from scipy.special import genlaguerre, gamma

df = pd.read_csv('moleculedata.csv')
# Remove the records where the vibrational spectrum constants have NaN
df = df.dropna(subset=['omega_ex_e (cm^{-1})','omega_e (cm^{-1})'])

# Create the dropdown list of diatomic molecules available
molecule_options=widgets.Dropdown(
    options=df['Molecule'].unique(),
    value='OH',
    description='Molecule:',
    disabled=False,
)

###########################################################################
# Now define the functions to plot Morse potential.
# The code is adapted from: https://scipython.com/blog/the-morse-oscillator/
# Major modifications to the code:
# 1. All member variables of the Morse class are stored in atomic units (energy: Hartree, mass: m_e, length: bohr, hbar=1) to simplify calculations and avoid unit conversions
# 2. Reduced mass as input instead of mA and mB
# 3. Plots have eV used for energy unit
# 4. Plot different electronic states of the same molecule on the same figure, label term symbol of the electronic states along the potential
##############################################################################

# Let's convert cm-1 to Eh (Hartree)
wavenumber2Hartree = 4.55633e-6
mp = 1836 # Porton mass is 1836 mass of electron
angstrom2Bohr = 1.88973
hbar = 1
hartree2eV = 27.2114
bohr2angstrom = 0.529177

class Morse:
    """A class representing the Morse oscillator model of a diatomic."""

    def __init__(self, mu, we, wexe, re, Te):
        """Initialize the Morse model for a diatomic molecule.
        Input arguments:
        mu input reduced mass (mass unit: proton mass).
        we, wexe are the Morse parameters (cm-1).
        re is the equilibrium bond length (Angstrom).
        Te is the electronic energy (cm-1).

        class member variables:(everything in atomic unit)
        mu, in electron mass (standard in atomic unit)
        we, wexe in Hartree
        re in bohr
        De in hartree
        D0 in hatree
        """
        # First of all, convert everything to atomic unit
        # where hbar = 1, me=1, Eh=1
        self.mu = mu*mp
        self.we, self.wexe = we*wavenumber2Hartree, wexe*wavenumber2Hartree
        self.re = re*angstrom2Bohr
        self.Te = Te*wavenumber2Hartree
        self.De = self.we**2*hbar / 4 / self.wexe

        #  Morse parameters, a and lambda.
        self.a = self.calc_a()
        self.ke = 2*self.De*self.a**2
        self.lam = np.sqrt(2 * self.mu * self.De) / self.a / hbar
        # Maximum vibrational quantum number.
        self.vmax = int(np.floor(self.lam - 0.5))

        self.make_rgrid()
        self.V = self.Vmorse(self.r)

    def make_rgrid(self, n=1000, rmin=None, rmax=None, retstep=False):
        """Make a suitable grid of internuclear separations."""

        self.rmin, self.rmax = rmin, rmax
        if rmin is None:
            # minimum r where V(r)=De on repulsive edge
            self.rmin = self.re - np.log(2) / self.a
        if rmax is None:
            # maximum r where V(r)=f.De
            f = 0.999
            self.rmax = self.re - np.log(1-f)/self.a
        self.r, self.dr = np.linspace(self.rmin, self.rmax, n,
                                      retstep=True)
        if retstep:
            return self.r, self.dr
        return self.r

    def calc_a(self):
        """Calculate the Morse parameter, a.

        Returns the Morse parameter, a, in unit bohr^-1

        """
        return self.we*np.sqrt(self.mu/2.0/self.De)

    def Vmorse(self, r):
        """Calculate the Morse potential, V(r).

        Returns the Morse potential at r (in m) for parameters De
        (in J), a (in m-1) and re (in m).

        """

        return self.De * (1 - np.exp(-self.a*(r - self.re)))**2

    def Emorse(self, v):
        """Calculate the energy of a Morse oscillator in state v.

        Returns the energy of a Morse oscillator parameterized by
        equilibrium vibrational frequency we and anharmonicity
        constant, wexe (both in cm-1).

        """
        vphalf = v + 0.5
        return (self.we * vphalf - self.wexe * vphalf**2)

    def calc_turning_pts(self, E):
        """Calculate the classical turning points at energy E.

        Returns rm and rp, the classical turning points of the Morse
        oscillator at energy E (provided in hatree). rm < rp.

        """

        b = np.sqrt(E / self.De)
        return (self.re - np.log(1+b) / self.a,
                self.re - np.log(1-b) / self.a)

    def calc_psi(self, v, r=None, normed=True, psi_max=1):
        """Calculates the Morse oscillator wavefunction, psi_v.

        Returns the Morse oscillator wavefunction at vibrational
        quantum number v. The returned function is "normalized" to
        give peak value psi_max.

        """

        if r is None:
            r = self.r
        xi = 2 * self.lam * np.exp(-self.a*(r - self.re))
        alpha = 2*(self.lam - v) - 1
        psi = (xi**(self.lam-v-0.5) * np.exp(-xi/2) *
               genlaguerre(v, alpha)(xi))
        psi *= psi_max / np.max(psi)
        return psi

    def calc_psi_Normalized(self, v, xi):
        alpha = 2*(self.lam - v) - 1
        psi = (xi**(self.lam-v-0.5) * np.exp(-xi/2) *
               genlaguerre(v, alpha)(xi))
        Nv = np.sqrt(factorial(v) * (2*self.lam - 2*v - 1) /
                     gamma(2*self.lam - v))
        return Nv * psi

    def plot_V(self, ax, **kwargs):
        """Plot the Morse potential on Axes ax."""

        ax.plot(self.r*bohr2angstrom, (self.V+self.Te)*hartree2eV, **kwargs)

    def get_vmax(self):
        """Return the maximum vibrational quantum number."""

        return int(self.we / 2 / self.wexe - 0.5)

    def draw_Elines(self, vlist, ax, **kwargs):
        """Draw lines on Axes ax representing the energy level(s) in vlist."""

        if isinstance(vlist, int):
            vlist = [vlist]
        for v in vlist:
            E = self.Emorse(v)
            rm, rp = self.calc_turning_pts(E)
            ax.hlines((E+self.Te)*hartree2eV, rm*bohr2angstrom, rp*bohr2angstrom, **kwargs)

    def label_levels(self, vlist, ax):
        if isinstance(vlist, int):
            vlist = [vlist]

        for v in vlist:
            E = self.Emorse(v)
            rm, rp = self.calc_turning_pts(E)
            ax.text(s=r'$v={}$'.format(v), x=rp*bohr2angstrom + 0.6,
                    y=(E+self.Te)*hartree2eV, va='center')

    def label_electronstate(self, state_str,ax):
        E = self.Emorse(self.vmax)
        ax.text(s=state_str, x=np.max(self.r)*bohr2angstrom,
                y=(E+self.Te)*hartree2eV-0.2, va='center')

    def plot_psi(self, vlist, ax, r_plot=None, scaling=1, **kwargs):
        """Plot the Morse wavefunction(s) in vlist on Axes ax."""
        if isinstance(vlist, int):
            vlist = [vlist]
        for v in vlist:
            E = self.Emorse(v)*hartree2eV
            if r_plot is None:
                rm, rp = self.calc_turning_pts(E)
                x = self.r[self.r<rp*1.2]
            else:
                x = r_plot
            psi = self.calc_psi(v, r=x, psi_max=self.we/2)
            psi_plot = psi*scaling + (self.Emorse(v)+self.Te)*hartree2eV
            ax.plot(x*bohr2angstrom, psi_plot, **kwargs)


##########################################
# Below is the interactive plotting part #
##########################################

fig= plt.figure()
@widgets.interact(molname=molecule_options)
def update(molname):
    fig.clf()
    mol = df[df['Molecule']==molname]
    states=mol['Electronic state'].values
    ax = plt.gca()
    # Set the default color cycle
    colors=['blue', 'red', 'green', 'cyan', 'orange', 'gray', 'purple', 'brown', 'teal', 'coral']
    i_color=0
    for state in states:
        mycolor = colors[i_color]
        i_color+=1
        we = mol[mol['Electronic state']==state]['omega_e (cm^{-1})'].values[0]
        wexe = mol[mol['Electronic state']==state]['omega_ex_e (cm^{-1})'].values[0]
        re = mol[mol['Electronic state']==state]['Re (\AA)'].values[0]
        Te =  mol[mol['Electronic state']==state]['Te (cm^{-1})'].values[0]
        mu = mol[mol['Electronic state']==state]['Reduced mass'].values[0]

        X = Morse(mu, we, wexe, re, Te)
        X.make_rgrid()
        X.V = X.Vmorse(X.r)

        X.plot_V(ax,color=mycolor)

        X.draw_Elines(range(X.vmax), ax,linewidth=0.5, color=mycolor)
        X.label_electronstate(state,ax)
        #X.plot_psi([2, 14], ax, scaling=2, color=COLOUR1)
        #X.label_levels([2, 14], ax)

        ax.set_xlabel(r'$r\;/\mathrm{\AA}$')
        ax.set_ylabel(r'Energy (eV)')
        ax.spines['top'].set_visible(False)
        ax.spines['right'].set_visible(False)

{% endhighlight %}
</pre>

## Questions
* Read the code and answer the questions below
	1. How do we obtain the binding energy $D_e$ from the vibrational spectrum constants $\omega_e$ and $\omega_e x_e$?
	1. When the spectrum data for multiple electronic states are available, which variable in the data set determines the relative energy of different states?
	1. What is the typical energy difference between electronic states? What is the typical energy difference between vibrational states?
	1. Do you understand the meaning of the Term Symbols that label different electronic states?

# Acknowledgement:

  1. The diatomic spectrum dataset is obtained from this website: [<span style="color:blue">https://rios.mp.fhi.mpg.de/</span>](https://rios.mp.fhi.mpg.de/).

     The associated paper about their work:

     Liu, X., Truppe, S., Meijer, G. et al. The diatomic molecular spectroscopy database. J Cheminform 12, 31 (2020). https://doi.org/10.1186/s13321-020-00433-8

  1. The python class ```Morse``` is adapted from this blog: [<span style="color:blue">https://scipython.com/blog/the-morse-oscillator/</span>](https://scipython.com/blog/the-morse-oscillator/).

     I made some modifications to make it follow the conventions of writing quantum chemistry programs.

     1. All member variables of the Morse class are stored in atomic units (energy: Hartree, mass: m_e, length: bohr, hbar=1) to simplify calculations and avoid unit conversions
     1. Reduced mass is used as input instead of $m_A$ and $m_B$.
     1. Plots use the unit, eV, for energy.
     1. We plot different electronic states of the same molecule on the same figure, label term symbol of the electronic states along with the potential.
