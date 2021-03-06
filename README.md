2019-thunderstorms-analysis

# Lagrangian Analysis of Thunderstorms in Switzerland
## Analysis Tools for Investigating Thunderstorm Initiation Conditions

Author:

Thomas M. Lanz | 14-119-564 | MSc in Climate Sciences | OCCR - University of Bern | thomas.lanz@students.unibe.ch

## Introduction and Research Question
The aim of the analysis tools is the investigation of atmospheric conditions and processes for thunderstorm initiation. Such atmospheric conditions for the evolution of deep convective cells are an unstable stratification of the atmosphere, a substantial amount of ground level moisture and an instability triggering process like convergence or lifting (Wallace & Hobbs, 2006). These conditions represent the basic ingredients for thunderstorm initiation. In general, a thunderstorm is defined by the American Meteorological Society (AMS) as '[...]a local storm, invariably produced by a cumulonimbus cloud and always accompanied by lightning and thunder, usually with strong gusts of wind, heavy rain, and sometimes with hail' (AMS, 2012). Despite their frequent occurrence and damage potential (MeteoSwiss, 2018; Nisi et al., 2016; Trefalt et al., 2018), the initiation of thunderstorms is still incompletely understood. The goal of the analysis tools is to fill this research gap and to answer the following research question: 

How far can the analysis tools unveil the atmospheric conditions and processes (basic ingredients) responsible for thunderstorm initiation?

This coding project is conducted in the framework of the seminar 'Geodata Analysis and Modelling' (FS2019-438745) and is settled within a master thesis project.

## About the Analysis Tools
For reaching the prediscribed aim of this project, the analysis tools consists of five different tools for investigating the conditions and processes of the atmosphere: horizontal maps, soundings, vertical cross-section, maps of trajectories and temporal evolution along trajectories. The figures created with these tools serve as basis for the analysis of thunderstorm initiation and the answering of the research question.

The programming was done in IDE Jupyter Notebook (v6.0.3) with the IPython Kernel (v7.13.0) and with package and environment management by Conda. For each of the five analysis tools, a notebook document was created (also python files are provided in a seperate folder). In the following section the workflow of the analysis tools is described.

## What the Analysis Tools Do and How to Use
For the analysis tools data from the Weather Research and Forecasting (WRF) model (v4 ARW) (Powers et al., 2017) and from the Lagranto program (Sprenger & Wernli, 2015) is used, which takes input data from the WRF model. The WRF data is in netCDF file format and the Lagranto output data in ASCII file format. 

The following subsections provide information regarding the useage of the codes. For background information of the scientific methods and more information regarding the data used in this project, see 'scientific_background.pdf'. Example figures of each analysis tool are presented in the subsequent Results section.

### Horizontal Maps
This analysis tool produces horizontal (2D) maps of different variables. Supported variables for the plotting function (horizontal_map) are updraft, reflectivity, helicity, pw, cape, cin, ctt, temperature_surface, wind_shear, updraft_reflectivity, rh, omega, pvo, avo, theta_e, water_vapor, uv_wind and divergence. Besides the selection of the desired variable_name for the function, more input parameters need to be defined like date, start_hour, end_hour, pressure_level, subset, initiation, save and gif. The first five parameters (variable_name included) need a specific input value (e.g. variable_name="divergence", date="2018-05-30", start_hour=16, end_hour=17, pressure_level=850), where the remaining parameters need Boolean values (True or False). Before using the plotting function, the definition of some other variables in first section of the code is necessary (data_dir, save_dir, subset_extent and initiation_location). The horizontal_map function iterates with a 5 minutes time step over a list of files from start_hour until end_hour and creates figures of each time step. If the gif parameter is set True, a gif is created from all the generated figures of the interation process.

### Sounding
The sounding is an analysis tool for investigating the vertical distribution of atmospheric physical properties (e.g. temperature, pressure, wind, etc.) and represents the WRF model data in a similar way (thermodynamic diagram) like measurement data from an real world atmospheric sounding (e.g. balloon sounding). For a selected location (lat, lon) and time (date), the analysis tool generates a skew T-log p diagram, based on the variables derived from WRF data file (filename). Because the WRF model data lacks some of these specific variables (e.g. pressure, dewpoint temperature or wind speed), these required variables need to be computed in advance (by wrf-python function getvar()) and added back to the WRF dataset. Afterwards, the variables are selected for a specific location and some further variables need to be calculated with MetPy functions. Finally, the code generates a figure according to the specific layout of a skew T-log p diagram (see Results).

### Vertical Cross-Section
Vertical cross-sections show a vertical slice of the atmosphere along a line with a specified start (start_lat, start_lon) and end point (end_lat, end_lon). The analysis tool is represented by a plotting function (cross_section), which supports the following variables: vertical_velocity, rh, omega, absolute_vorticity, theta_e and reflectivity. The only input parameters left to define are date, time and save, if a saving of the figure is desired (default save=False). Before the cross_section function can be used, the data and save directory need to be adjusted according to the respective setting of the user. After interpolation and removing of white space between terrain height and contour of the variable, the code finally creates a vertical cross-section figure with filled mountain area.

### Maps of Trajectories
The analysis tool for mapping trajectories uses the output data (trajectories) from the Lagranto program. With the help of the lagranto_plotting function, the desired variables (water_vapor, updraft or height) along the trajectories can be plotted on the background of terrain height contours (greys). The start_time and end_time of the calculated trajectories need to be indicated as input parameters, as well as Booloean value True, if a subset or a saving of the figure is requested. Furthermore, a bunch of trajectories can be selected according to their height level (pbl, 5 or 10) or otherwise, all available trajectrories will be plotted. Prior to executing the function, some more variables need to be set for defining the Planteary Boundary Layer (PBL) height. The number of plotted trajectories in the figure is adjustable by specifying number_trajs_plot. In addition, the trajectory data and save directory need to be specified, as well as the pattern of trajectory starting points (e.g. 'single' or 'area'), the location of thunderstorm initiation and the extent of subset. After the definiton of all necessary input parameters, the function can be executed and a horizontal map of trajectories is generated.

### Temporal Evolution along Trajectories
This analysis tool creates a figure of the temporal evolution (time since initiation on the x-axis) of the chosen variable along the trajectories (variable values on the y-axis). As already mentioned in the subsection before, also the function temporal_evolution_trajectories is capable of seperating trajectories in bunches of different heights ('pbl', '5' or '10', default = 'all'). Besides the directory of the trajectory data and the desired save directory, delta time (dt in minutes) between the data files needs to be specified as well. If only a portion of trajectories should be included in the figure, then the number of plotted trajectories (number_trajs_plot) can be varied according to the requests of the user. Further, some variables for getting the PBL height need to be set. For the analysis and comparison between different vertical trajectory bunches, the 10th and 90th percentile and mean for each bunch of trajectories is computed and inidicated with colored lines on the figure (see legend for labeling). With all needed variables and input parameters defined, the function is ready for creating a figure of the temporal evolution along trajectories.

## Results
This section shows selected results of the respective analysis tools. A variety of figures for different variables and input parameters are presented. This should imply the large amount of possible combinations of input parameters and variables for plotting with these analysis tool functions.

### Horizontal Maps
#### horizontal_map("updraft", "2018-05-30", 16, 17, save=True)
<img src="results/horizontal_map_updraft_2018-05-30_16:20.png" width="500" height="400">


#### horizontal_map("temperature_surface", "2018-05-30", 16, 17, subset=True, save=True)
<img src="results/horizontal_map_temperature_surface_subset_2018-05-30_16:20.png" width="500" height="400">


#### horizontal_map("wind_shear", "2018-05-30", 16, 17, subset=True, save=True)
<img src="results/horizontal_map_wind_shear_subset_2018-05-30_16:20.png" width="500" height="500">


#### horizontal_map("rh", "2018-05-30", 16, 17, pressure_level=850, subset=True, initiation=True, save=True)
<img src="results/horizontal_map_rh_subset_850_2018-05-30_16:20.png" width="500" height="400">


#### horizontal_map("divergence", "2018-05-30", 16, 17, pressure_level=850, subset=True, initiation=True, save=True)
<img src="results/horizontal_map_divergence_subset_850_2018-05-30_16:20.png" width="500" height="400">


#### horizontal_map("updraft_reflectivity", "2018-05-30", 16, 17, subset=True, initiation=True, save=True, gif=True)
<img src="results/horizontal_map_updraft_reflectivity.gif" width="500" height="400">


#### horizontal_map("theta_e", "2018-05-30", 15, 17, pressure_level=850, subset=True, initiation=True, save=True, gif=True)
<img src="results/horizontal_map_theta_e.gif" width="500" height="400">
Note: GIF only until time of initiation 16:20 UTC, otherwise GIF file would be too big for the upload. Procedure: Stopped iteration at 16:20, then copied code part out of the dunction (defined save_dir and save_name) and generated GIF separately.


#### horizontal_map("cape", "2018-05-30", 15, 17, subset=True, initiation=True, save=True, gif=True)
<img src="results/horizontal_map_cape.gif" width="500" height="400">
Note: Same approach for GIF creation as described for Theta-E GIF.


#### horizontal_map("cin", "2018-05-30", 15, 17, subset=True, initiation=True, save=True, gif=True)
<img src="results/horizontal_map_cin.gif" width="500" height="400">
Note: Same approach for GIF creation as described for Theta-E GIF.

---

### Sounding
<img src="results/sounding.png" width="500" height="500">

---

### Vertical Cross-Section
#### cross_section('reflectivity', '2018-05-30', '16:25', 47.25, 7.4, 47.25, -1.5, save=True)
<img src="results/cross_section_reflectivity.png" width="600" height="400">
Note: At 16:20 UTC no reflectivity values could be detected in the cross-section, only at 16:25 UTC.


#### cross_section('rh', '2018-05-30', '16:20', 47.20, 7.4, 47.25, -1.5, save=True)
<img src="results/cross_section_rh.png" width="600" height="400">


#### cross_section('theta_e', '2018-05-30', '16:20', 47.25, 7.4, 47.25, -1.5, save=True)
<img src="results/cross_section_theta_e.png" width="600" height="400">


#### cross_section('omega', '2018-05-30', '16:20', 47.25, 7.4, 47.25, -1.5, save=True)
<img src="results/cross_section_omega.png" width="600" height="400">

---

### Maps of Trajectories
#### lagranto_plotting("height", "1620", "1200", save=True)
<img src="results/trajectory_height_1620_1200_area.png" width="600" height="500">
Note: number_trajs_plot = 2


#### lagranto_plotting("water_vapor", "1620", "1200", trajs_bunch_level='pbl', subset=True, save=True)
<img src="results/trajectory_water_vapor_1620_1200_subset_all_area.png" width="600" height="500">
Note: number_trajs_plot = 1


#### lagranto_plotting("updraft", "1620", "1200", trajs_bunch_level='pbl', subset=True, save=True)
<img src="results/trajectory_updraft_1620_1200_subset_pbl_area.png" width="600" height="500">
Note: number_trajs_plot = 1 and changed extent of subset.

---

### Temporal Evolution along Trajectories
#### temporal_evolution_trajectories('height', save=True)
<img src="results/lagranto_evo_height_all_47.25_7.85.png" width="700" height="500">


#### temporal_evolution_trajectories('water_vapor', trajs_bunch_level='5', save=True)
<img src="results/lagranto_evo_water_vapor_5_47.25_7.85.png" width="700" height="500">


#### temporal_evolution_trajectories('updraft', trajs_bunch_level='pbl', save=True)
<img src="results/lagranto_evo_updraft_pbl_47.25_7.85.png" width="700" height="500">


## Conclusion
A selection of results from each analysis tool was presented in the section before. The fact, that many more results could have been shown, reveals the potential of the analysis tools for investigating the conditions and processes of the atmosphere. Of course this potential is dependent on the input data and therein, the available variables. With the data used in this project (see section About the Analysis Tools and 'scientific_background.pdf'), the previously described basic ingredients for thunderstorm intiation (simplified: instability, moisture and trigger) can be analysed in great detail with the generated analysis tools. For each basic ingredient, different tools can be used for the investigation of the respective atmospheric conditions and processes. The tools enable changing the scope from wide to narrow and allow many different combinations of the input parameters. Thus, a strength of the analysis tools is their easy adaptation of configurations and the automatized procedure of creating figures, which helps in procuding a big amount of data in a short amount of time. How far the analysis tool can unveil the basic ingredients is in the end limited by the spatial and temporal resolution of the used data. But the codes of the analysis tools are able to generate the necessary information for the assessment of the conditions and processes in the atmosphere. To conclude and answer the stated research question directly, the analysis tools can unveil the atmospheric conditions and processes for thunderstorm intiation in great spatial and temporal detail, because of their various kinds of results and configurations. But this detailedness is dependent on the used data, which requires high spatial and temporal resolution for the analyis of thunderstorm initiation.

## Thanks
My thanks go to the supervisors of this project Dr. Andreas Zischg (University of Bern), Dr. Pascal Horton (University of Bern) and Dr. Jorge Ramirez (University of Bern) for their contribution during the semester and their flexibility with the submission date. The same people were responsible for a very interesting seminar, which helped me a lot to get going with my coding. 

## References
AMS. (2012, April 25). Thunderstorm - AMS glossary. Retrieved April 2, 2019, from http://glossary.ametsoc.org/wiki/Thunderstorm

MeteoSwiss. (2018, December 21). 2018: Rekordwärme und massive regenarmut. Retrieved March 23, 2020, from https://www.meteoschweiz. admin.ch/home/suche.subpage.html/de/data/blogs/2018/12/ 2018-waermstes-jahr-seit-messbeginn-.html?query=sommer+2018+ schweiz&topic=0&pageIndex=0&tab=search tab

Nisi, L., Martius, O., Hering, A., Kunz, M., & Germann, U. (2016). Spatial and temporal distribution of hailstorms in the alpine region: A long-term, high resolution, radar-based analysis. Quarterly Journal of the Royal Meteorological Society, 142(697), 1590–1604. https://doi.org/10.1002/ qj.2771

Powers, J. G., Klemp, J. B., Skamarock, W. C., Davis, C. A., Dudhia, J., Gill, D. O., Coen, J. L., Gochis, D. J., Ahmadov, R., Peckham, S. E., Grell, G. A., Michalakes, J., Trahan, S., Benjamin, S. G., Alexander, C. R., Dimego, G. J., Wang, W., Schwartz, C. S., Romine, G. S., . . . Duda, M. G. (2017). The weather research and forecasting model: Overview, system efforts, and future directions. Bulletin of the American Meteo- rological Society, 98(8), 1717–1737. https://doi.org/10.1175/BAMS-D- 15-00308.1

Sprenger, M., & Wernli, H. (2015). The LAGRANTO lagrangian analysis tool – version 2.0. Geoscientific Model Development, 8(8), 2569–2586.

Trefalt, S., Martynov, A., Barras, H., Besic, N., Hering, A. M., Lenggenhager, S., Noti, P., Röthlisberger, M., Schemm, S., Germann, U., & Martius, O. (2018). A severe hail storm in complex topography in switzerland - observations and processes. Atmospheric Research, 209, 76–94. https: //doi.org/10.1016/j.atmosres.2018.03.007

Wallace, J. M., & Hobbs, P. V. (2006, March 24). Atmospheric science: An introductory survey [Google-Books-ID: HZ2wNtDOU0oC]. Elsevier.
