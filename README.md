# GenRis
Simulating the probabiliy of picking clones and unique genotypes in phytoplankton populations to estimate Genotype Richness.

You can run this simulation in python with the following commands:

> import os

> os.getcwd() # check your current working directory

> os.chdir('path/to/folder/with/GenRiS.py') # set working directory where the GenRiS.py file is located and where the output files will be generated


> from GenRiS import genris

> genris(numgenos = v, days = w, growthrate = x, sd = y, samplesize = z)


Alternatively, if you want to run the script from a terminal, you can use:

> python3 -c 'from GenRiS import genris; genris(numgenos = v, days = w, growthrate = x, sd = y, samplesize = z)'

The GenRiS.py file needs to be stored in the same folder, where you want to create your output files.
The simulation permutates through a range of different population sizes (number of genotypes = numgenos), samplesizes (number of cells that will be picked) and standard deviations of the normally distributed growth rate function (sd). Larger sd will result in larger differences between growth rates of individual genotypes. Ranges have to be expressed as lists (e.g. numgenos = [200, 400, 600, 800, 1000, 1500, 2000, 3000]), while discrete values can be filled in directly.
Only two variables with ranges of values are allowed per run! 
The growth rate has to be expressed in divisions per day. Each genotype will initially be represented by one cell reflecting a population at the very beginning of the growth season. The variable "days" refers to the time period of interest, in which the algal population grows exponentially.
This will create a table and a heatmap showing the mean probability of picking clones across the range of your selected variables (numgenos, samplesize or sd).

If you're interested in estimating the genotype richness for a specific observed proportion of clones, you have to add the size of your sample (in which you counted the clones) to the end of the variable list (e.g. sample = 100). The variable list for this run has to contain ranges of numgenos and samplesize, which includes the size of your actual sample! This will create an output called "Probs_for_Sample.csv" containing the probability of picking clones in a sample with your specified size for the selected numbers of genotypes (numgenos) from 500 repeated picking events. This table will be the input for python script "CurveFitting.py".

Examples:

> from GenRiS import genris

> genris(numgenos = [200, 400, 600, 800, 1000, 1500, 2000, 3000, 5000, 10000], days = 60, growthrate = 0.2, sd = 0.025, samplesize = [50, 100, 150, 200, 300, 400, 500], sample = 150)


> from GenRiS import genris

> genris(numgenos = 2000, days = 60, growthrate = 0.2, sd = [0.021, 0.023, 0.025, 0.027, 0.029], samplesize = [50, 100, 150, 200, 300, 400, 500])

# 




# CurveFitting

Fitting a curve through the mean probabilities of picking clones in a range of population sizes and a set sample size to estimate genotype richness based on an observed proportion of clones.

You can run this simulation in python with the following commands:

> import CurveFitting

> CurveFitting.curve_fitting(numgenos = x, prop_clones = y)

The GenRiS.py file needs to be stored in the same folder where the "Probs_for_Sample.csv" file from the GenRis simulation is saved and where you want to create your output files.
The range of population sizes (numgenos) has to be identical with numgenos in the GenRis simulation that created the "Probs_for_Sample.csv" output file. This script will create a plot of the curves fitting the mean probabilities and σ. It will also provide the R^2 value of the mean curve fitting, the average number of expected genotypes in your sample, and the lower and upper confidence intervals (σ) to this estimate.


Example:

> import CurveFitting

> CurveFitting.curve_fitting(numgenos = [200, 400, 600, 800, 1000, 1500, 2000, 3000, 5000, 10000], prop_clones = 0.153)
