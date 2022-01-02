# Landlab-X-TerrainBento-sensitivity-research

# This repository contains all of the codes used in the research paper 'Studying the sensitivity of the pre-processing parameters of a new analysis framework through the characterisation of evolving landscapes' 

# The main base of the code is taken from the TerrainBento repository [https://github.com/TerrainBento/terrainbento] and runs over the top of the Landlab package on Python 


# TerrainBento code ---basic---



from terrainbento import Basic

from io import StringIO
file_DEM_46 = StringIO

params = {
    # create the Clock.
    "clock": {"start": 0,
              "step": 300,
              "stop": 9e4},

    # Create the Grid
    "grid": {
        "RasterModelGrid": [
            (500, 500),
            {
                "xy_spacing": 10
            },
            {
                "fields": {
                    "node": {
                        "topographic__elevation": {
                            "random": [{
                                "where": "CORE_NODE"
                            }]
                        }
                    }
                }
            },
        ]
    },

    # Set up Boundary Handlers
    "boundary_handlers":{"NotCoreNodeBaselevelHandler": {"modify_core_nodes": True,
                                                         "lowering_rate": -0.001}},
    # Parameters that control output.
    "output_interval": 1e3,
    "save_first_timestep": True,
    "fields":["topographic__elevation"],

    # Parameters that control process and rates.
    "water_erodibility" : 0.001,
    "m_sp" : 0.5,
    "n_sp" : 1.0,
    "regolith_transport_parameter" : 0.02,           
         }

model = Basic.from_dict(params)
model.run()

from landlab import imshow_grid

filenames = []
ds = model.to_xarray_dataset()
for i in range(ds.topographic__elevation.shape[0]):
    filename = "temp_output."+str(i)+".png"
    imshow_grid(model.grid, ds.topographic__elevation.values[i, :, :], cmap="viridis", limits=(0, 10), output=filename)
    filenames.append(filename)
model.remove_output_netcdfs()


import os
import imageio
with imageio.get_writer("regolith_parameter chnage.gif", mode="I") as writer:
    for filename in filenames:
        image = imageio.imread(filename)
        writer.append_data(image)
        os.remove(filename)
        
