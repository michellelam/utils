# Introduction 

This file is to provide a brief tutorial to get a local Open Street Map (OSRM) instance running on a Mac and access it in R. I am building off of [R-Bloggers article](https://www.r-bloggers.com/2017/09/building-a-local-osrm-instance/). 

As of March 2021, I am using macOS Big Sur 11.2.2 and R 3.6.1. 

# Setup 

Open up Terminal and input the following: 

```
git clone https://github.com/Project-OSRM/osrm-backend.git    
cd osrm-backend
```

Check which release you want by visiting the [Github releases](https://github.com/Project-OSRM/osrm-backend/releases). Change the version as necessary.

```
git checkout v5.24.0
```

Install and/or update the following packages as necessary. I am using Homebrew, which you can install by following [these instructions](https://docs.brew.sh/Installation). 

```
brew install boost 
brew install protobuf                                                            
brew install tbb    
brew install libstxxl       
brew install bzip2                                                               
brew install pkg-config  
brew install lua 
brew install geos
brew install curl
```

# Generate Makefiles 

Input the following lines into the Terminal:  

```
mkdir build
cd build
cmake -DLUA_LIBRARY=/usr/local/lib/liblua.5.4.dylib -DLUA_INCLUDE_DIR=/usr/local/include/lua5.4 .
make 
make install
```

Note that you may have a different version of Lua, so you may have to change the input to reflect that.

# Download the data

You can go to the [export page on OpenStreetMap](https://www.openstreetmap.org/export) to download an area of interest. Once you have zoomed in, you press the Export button. You can also download from [Geofabrik](http://download.geofabrik.de/). Put the new file into the `osrm-backend` folder you created.

For the purposes of this tutorial, I will be using `new-york-latest.osrm.pbf` as my file of interest. Run the following code on your file: 

```
osrm-extract new-york-latest.osrm.pbf
osrm-contract new-york-latest.osrm
osrm-routed new-york-latest.osrm
```

Note that in the osrm-routed command, you may want to include the flag `--max-table-size=X` with a size `X` of your choosing, or else you might get the error `The OSRM server returned an error: Error: object of type 'closure' is not subsettable` if the table you are trying to produce exceeds the default maximum.

# Accessing the local OSRM instance in R 

I will be using the package [osrm](https://cran.r-project.org/web/packages/osrm/osrm.pdf). Open up your R and run the following code: 

```
install.packages('osrm') 
library(osrm) 
```

Then you will need a dataframe (or a sf, sp object) that contains the following columns, in order: `ID, longitude, and latitude`. For the purposes of this tutorial, I will name it `ny_df`. Once you have that dataset, run the following code to create a distance matrix: 

```
options(osrm.server = "http://0.0.0.0:5000/")
dist <- osrmTable(loc = ny_df)
```

That's it! This is solely meant to be a bare-bones basic tutorial, so please reference the documentation and other articles for more details.

