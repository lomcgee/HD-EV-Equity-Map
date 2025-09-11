# HD EVEQ Map README
## Creating EVEQ Environment
First open a command prompt and enter the following steps to create an EVEQ environment

conda create --name EVEQ python=3.12.0

conda activate EVEQ

conda install -c conda-forge gdal

conda install pandas geopandas==1.0 

conda install plotly openpyxl

conda install h3-py h3

conda install -c anaconda ipykernel

conda install jupyter notebook


## Using HD EVEQ Map
HD EVEQ Map interprets proposed optimal charging station locations produced from evi_pro_hd and creates an interactive map which guides equitable placement of charging stations for electric MHDVs. 
Proposed charging station locations appear as red dots on the map and the desired number of nearest census block groups surrounding each charging station is inputted by the user in the cell starting with "#Specify the number of block groups you want to display." The number of block groups is inputted in "block_groups = " and analyzed.

### Downloading Data
The block groups are assigned a Disadvantaged Community (DAC) Index which is created by analyzing 20 equity metrics from an EPA dataset to find the relative level of disadvantage experienced by each community. **Download the EPA dataset on the EPA's Download EJScreen Data webpage, https://www.epa.gov/ejscreen/download-ejscreen-data, by clicking "Download Geodatabase of National EJScreen Data at the Block Group Level."** **Update the path in the lines "zip_path = *path*" and "extracted_path = *path*" to wherever the files are saved on your local machine.** Note: the data used when creating this tool was released in July 2024. 
A higher DAC Index indicates a community is at more of a disadvantage relative to other communities. The block groups are colored using a color scale which corresponds to their DAC Index score. 

DOT and DOE created interim definitions of disadvantaged communities developed for the National Electric Vehicle Infrastructure (NEVI) Formula Program which were mapped. **Download this data off the ANL Electric Vehicle Charging Equity Considerations webpage, https://www.anl.gov/esia/electric-vehicle-charging-equity-considerations, by clicking "DOWNLOAD" under "DAC GIS Shapefiles."** **This file is imported in the cell titled "#Importing DOE/DOT identified DACs data", update the path in the lines "DOEDOT_zip_path = 'dacs_nevi_joint_May2022.zip'" and "DOEDOT_extracted_path = 'dacs_nevi_joint_May2022'" to the path where the files are saved on your local device.** The DOE and DOT identified DACs are displayed as census tracts on the map by areas enclosed with a thick line. Census tracts which are being analyzed but were not identified as DACs by the DOE or DOT are displayed with a medium sized line which is thicker than the smaller block groups dividing lines, but thinner than the identified DAC lines

Information is logged in a file which should automatically be created called EVEQ.log

### Potential Geometry Conversion Error and Fix
When running the code skip the third cell which begins with "gdf = gdf1"
One unusual error sometimes occurred when trying to convert the EPA dataset geometries to the World Geodetic System 1984 (WGS84) in cell 4. WGS84 is the standard world geodetic system. The geometry started in the EPSG 4269 coordinate system which is the North America onshore and offshore system. Usually, this conversion worked without issues, however, it occasionally would turn all the geometries to invalid entries marked as “inf” for every entry. Additionally, if this conversion error occurred, the following statement will generate many times in the EVEQ.log file: "DEBUG:pyproj:PROJ_ERROR: Cannot open https://cdn.proj.org/us_noaa_nvhpgn.tif: schannel: next InitializeSecurityContext failed: CRYPT_E_NO_REVOCATION_CHECK (0x80092012) - The revocation function was unable to check revocation for the certificate." If this occurrs, reload the GeoDataFrame by running the previous cell which was skipped, "gdf = gdf1" and retry the conversion.

### Downloading Charging Station Locations
**Download the station.csv file located on the main branch of this github.** This file is read in the cell starting with "#Importing potential station location data into a geodataframe." Updated files with more proposed station data can be inputted into "OPT_STA_df = pd.read_csv('stations.csv')." **If using the same file, update the path to where the file is saved on your local device.** HD EVEQ Map interprets a csv file which must contain one column titled “hex” which contains hex codes to define geographical locations, and another column titled “built”. Within the “built” column an input of “1” identifies prospective locations where charging stations are planning to be built, and an input of “0” identifies prospective locations where charging stations are not going to be built.  The data is cleaned so that only locations that have been identified as potential site locations (a “built” value of 1) are kept.

### Type of Charging Station Inputs
In the cell beginning with "#Input the type of charging station..." the user can input the type of charging station being built and this will change the guidance the tool outputs. Options here are inputting "PureHDV" if only HDV ports will be available. "MixedPorts" if MHDV and LDV ports will be available. And "" if neither of these options are the case. Note: any mispelling or input other than the first two options with the same case will return the same value as "".

### Saving Note
Clear all outputs before closing/saving the jupyter notebook on a local machine. Saving without clearing outputs will make the final save incredibly large and can cause problems when trying to open the jupyter notebook again.

For any questions contact Liam McGee, lomcgee@calpoly.edu or Nadia Panossian at nadia.panossian@nrel.gov.
