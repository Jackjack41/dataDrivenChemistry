AC 209 Data Science Final project 
===================
### (Gifs loading, give a minute, it's worth it)

<img src="http://i.imgur.com/pgXt52x.gif">

(*Ab initio Molecular Dynamics of Molecules doing the Harlem Shake*)

# Contents
- [The Idea](#the-idea) 
- [Data](#data) 
- [Exploration Visualization](#exploration-visualization) 
- [Finding Patterns](#finding-patterns) 
- [Future Directions](#future-directions) 


# The Idea
**Can we "discover" chemistry concepts via patterns in data?** 
*(Obviously, let me explain more about what I mean)*

Molecules are small objects whose chemical behaviour is governs by physcial laws (Quantum Mechancis) and the dynamics between protons and electrons.
<img src="http://fat.gfycat.com/GeneralWarmheartedGopher.gif">

Turns out we can simulate these laws in a computer, solving the Schrödinger equation to obtain molecular properties that you could otherwise obtain via an experiment.

Computational chemistry is finally in a position to conduct studies on hundreds or even thousands of molecules within a reasonable amount of time. That means a lot of data :).

Recently in Ramakrishnan et Al. reported in Nature Scientific Data, ["Quantum chemistry structures and properties of 134 kilo molecules"](http://www.nature.com/articles/sdata201422) a data set 134k calculated molecules. 

Part of the philosophy of calculating molecules on a computer is the "First Principles", "Ab-initio", "Without knowing anything" mind set, we want to predict and model reality (molecular reality that is) without previous knowledge, without emphirical observations (at least to certain degree). 

*For example if a computer had molecules of the skin of tomato, would it know the tomato is red?* By simulating how light hits the molecules and bounces back we can find out the wavelength of this returned light, and find out it is actually within the spectrum of the color "red". **We can predict the tomato is red "without knowing anything"**.

The aim of this project is to play around with this dataset and see what chemistry we can learn.

# Data

Dataset is hosted on [figshare](http://dx.doi.org/10.6084/m9.figshare.978904), it is a few hundred MB's.
Each calculation is in a specially formated text file (xyz file) which cotains the optimized geometry, the energetics and thermodynamic properties for a single molecule. There are in total 134k stable small organic molecules made up of CHONF atoms.
The first step was to parse each file in a very specific way, as indicated in the README.txt included in the data set.

I created a pandas dataframe, with a row for each molecule, containing all the information within the txt file. This data structure can easily be converted to other more information-friendly formats (excel, databases, json). 
A pandas dataframe pickle can be downloaded [here](https://www.dropbox.com/sh/6iysi4w0xmmevlt/AABrTLUFZJvrJPDeDCzuhxJYa?dl=0).
I also computed a few other descriptors not included in the article, such as enthalpies of Atomization and molecular weight.

The entire data set once filtered is a 130830 molecules by 25 features.
![Data Wrangling](http://i.imgur.com/zFEFEX2.png)

# Exploration Visualization
I carried out first an inital exploration of all variables, to see which ones were more correlated, which ones were redundant and also to classify molecules based on their rotation.

The variables to explore were:
* Size: Number of atoms, Molecular Weight, Spatial Extensivity.
* Rotation: Rotational Constants and Clases.
* Energetics: Internal energy, Enthalpy, Gibbs free energy, Zero Point Energy, Atomization Energies.
* Special Properties: Electronic Band Gap, Molar Heat Capacity, Dipole Moment and Polarizability

## Some examples:

**Here we have a random selection of ten molecules**:
![Random Selection of molecules](http://i.imgur.com/6fgfTsT.png)

**Distribution of the number of atoms on each Molecule**:
![Imgur](http://i.imgur.com/6Bqgbu8.png)

**Distribution of the molecular weight, two groups are apparent**:
![Imgur](http://i.imgur.com/r88fSr2.png)

**Violinplots of Rot Constants (A,B,C) for different types of rotors **
= (Asymmetrical, Linear, Oblate, Prolate and Spherical) respectively:
![Imgur](http://i.imgur.com/pyqfg4b.png)

Due to the sheer number of samples, **many variables followed a normal distribution** (like Heat Capacity), these types of variables can sometimes be removed for modelling since there is little variability.
![Imgur](http://i.imgur.com/ggdfHD9.png)

While other properties were much more interesting suggesting possible clusters of molecules here we have the **Distribution of Electronic Band gaps **.
![Imgur](http://i.imgur.com/fjCFqv0.png)

**Distribution of Enthalpies of Atomization ** also displays with some peaks.
![Imgur](http://i.imgur.com/NVRfL8R.png)


# Finding Patterns (Machine Learning!)
Having explored the dataset we can trim and have a better idea of what types of techniques might work.
I decided to tryout to train a molecular classifiy. The criteria was common substructures.



I did explore matching molecules based on similarity. 
For example taking 1000 random molecules and checking out how similar they were (geometry/structure wise)



This goal, which is the most interesting was not met due to time constraints.
The main idea was:
* Search the most common functional groups, chemical substructures that contain key chemical functionality. For this purpose I wanted to use RDKit, but had problems running it. Tried to think how to formulate this with a Kmeans classifier.
* Create a labeling for each molecule based on a molecule having the substructure or not. This would create a membership of a molecule to within different clusters.
* Reduce dimentionality, maybe with a TruncatedSVD or finding the best features by ranking their importance via a random forest classifier.
* See if membership into a cluster would correspond to general differences in the molecular properties.
* Use SVM/Random Forest to create a classifier of molecular substructure based on molecular properties.


# Future Directions
There is certaintly much more work to be done and many venues to explore with data science and Quantum Chemistry.
For the msot part I stuck to the available dataset, yet I feel there could be more interesting data embedded in the calculations which is not included in the data set.
Examples:
* **Frequencies**, only frequencies were reported but amplitudes were not. Using amplitudes and frequencies together you could devise spectra features such as spectral centroids, brightness o cSpectrum.
* **Coulombic Matrix**, The matrix of electromagnetic interaction between different
molecules.
* **Orbital Occupation Numbers**, Occupation numbers for the electrons within the molecular orbitals.

The main idea would be design and find descriptor that can not be found in a classical way, to demonstrate that a quantum calculation has more conection to reality than a classical one. (at the molecular level).

<img src="http://giant.gfycat.com/FairWaryBactrian.gif">
