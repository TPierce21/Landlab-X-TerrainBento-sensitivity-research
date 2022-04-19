# Landlab-X-TerrainBento-sensitivity-research

# This repository contains all of the codes used in the research paper 'Studying the sensitivity of the pre-processing parameters of a new analysis framework through the characterisation of evolving landscapes' 

# The main base of the code is taken from the TerrainBento repository [https://github.com/TerrainBento/terrainbento] and runs over the top of the Landlab package on Python 


# Some additional post-processing as been requird due to the lack in precision of the output models in the im.show function provided by Landlab in Python. As such the post processing has been transferred over to MATLAB and all the necessary files to convert output files and the recquired MATLAB function necessary for calcualting the sub-basin number and channel lenght. 

# The additional TopoToolBox package has been used to analyse the output files for some of the necessary terrain statistics for the statistical comparison.
