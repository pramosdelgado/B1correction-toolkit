# B1correction-toolbox
This toolbox was designed with the purpose of correcting for B<sub>1</sub> inhomogeneities inherent of transmit-receive (transceive) surface RF coils used in magnetic resonance imaging.

This code repository implements three algorithms for B<sub>1</sub> correction (sensitivity-based, model-based, hybrid) shown in the peer-reviewed publication: 

> Ramos Delgado P, Kuehne A, Periquito J, et al. B<sub>1</sub> Inhomogeneity Correction of RARE MRI with Transceive Surface Radiofrequency Probes. Magnetic Resonance in Medicine 2020; 00:1-20. https://doi.org/10.1002/MRM.28307 

Use of these algorithms is allowed given proper citation of the original work and in accordance with the license file.

## Table of Contents
-	Installation Guide
-	Getting Started
-	Reproducing my results
-	Contact
- License, authorship and citations

## Installation Guide
This toolbox has been developed using MATLAB R2010a and should run in this MATLAB version as well as in newer ones (tested also in MATLAB R2018a and R2019b). If you encounter any trouble or error, you can let me know using GitHub (https://github.com/pramosdelgado/B1correction-toolbox/issues) or contact me at ramosdelgado.paula@gmail.com. 

### Prerequisites
1.	CPU: ran without problem on an Intel® Core™ i7-7500U CPU @ 2.70 GHz 2.90GHz with 8GB RAM
2.	MATLAB R2010a or newer
3.	MATLAB toolboxes: 
    - Image Processing Toolbox
    - Symbolic Math Toolbox <sup>1</sup>
    - Polyfitn Toolbox (MATLAB File Exchange) a copy is provided with this toolbox <sup>2</sup>
4.	Custom-designed toolboxes:
    - RAREmodel_fitting-toolbox 
    - B1correction-toolbox
    - validityAssessment_and_plotting-toolbox

<sup>1</sup> The toolbox provides a replacement of the symbolic polynomial functions, in case your MATLAB does not have the required toolbox

<sup>2</sup> The version available in this toolbox corresponds to the version available in 2018. There might be further improvements in the future. Users should check for possible updates.

### Adding the toolboxes to your path
The custom-designed toolboxes must be added to your path so that MATLAB can access them. To do that, enter the following commands in the MATLAB Command Window:
```
addpath(genpath(‘O:\Users\pramosdelgado\RAREmodel_fitting-toolbox’));
addpath(genpath(‘O:\Users\pramosdelgado\B1correction-toolbox’));
addpath(genpath(‘O:\Users\pramosdelgado\validityAssessment_and_plotting-toolbox’));
```
You may need to replace the paths above by your own path to the toolbox folders.

## Getting started
### General info on the available test data
The directory ```test_data``` corresponds to the acquired data used for the peer-reviewed publication:
> Ramos Delgado P, Kuehne A, Periquito J, et al. B<sub>1</sub> Inhomogeneity Correction of RARE MRI with Transceive Surface Radiofrequency Probes. Magnetic Resonance in Medicine 2020; 00:1-20. https://doi.org/10.1002/MRM.28307

**FOLDER cryoprobe_data:**
- ```uniformData_cryo.mat```: contains the registered T<sub>1</sub> map, the cryoprobe data and the volume RF coil data of the uniform phantom 
- ```exVivoData_cryo.mat```: contains the registered T<sub>1</sub> map, the cryoprobe data and the volume RF coil data of the ex vivo phantom 
- ```inVivoData_cryo.mat```: contains the registered T<sub>1</sub> map, the cryoprobe data and the volume RF coil data of the in vivo mouse 
- ```b1maps_cryo.mat```: calculated B<sub>1</sub> maps of the cryoprobe on a uniform phantom

**FOLDER cryoprobe_dataAUX:**
- ```phantom_sensCorrection_cryo.mat```: contains the registered uniform phantom cryoprobe data and corresponding masks for sensitivity-based B<sub>1</sub> correction 
- ```B1_rx_hybrid_cryo.mat```: contains the B<sub>1</sub><sup>+</sup>-corrected uniform phantom cryoprobe data for hybrid B<sub>1</sub> correction 

**FOLDER rtSUC_data (room-temperature surface RF coil):**
- ```flask_n#_flipbackData_rtSUC.mat```: contains the registered T<sub>1</sub> map, the surface RF coil data and the volume RF coil data (RARE with flipback) of the ```flask_n#```:
   - n1: with low T<sub>1</sub> and 100% water 
   - n2: with low T<sub>1</sub> and 50% water/50% d<sub>2</sub>O 
   - n3:  with high T<sub>1</sub> and 100% water
   - n4: with high T<sub>1</sub> and 50% water/50% d<sub>2</sub>O  
•	```flask_n#_NOflipbackData_rtSUC.mat```: contains the registered T<sub>1</sub> map, the surface RF coil data and the volume RF coil data (RARE without flipback) of the ```flask_n#```:
   - n1: with low T<sub>1</sub> and 100% water 
   - n2: with low T<sub>1</sub> and 50% water/50% d<sub>2</sub>O  
   - n3:  with high T<sub>1</sub> and 100% water
   - n4: with high T<sub>1</sub> and 50% water/50% d<sub>2</sub>O  
•	```b1map_rtSUC.mat```: B<sub>1</sub> map of a uniform phantom

**FOLDER rtSUC_dataAUX (room-temperature surface RF coil):**
- ```phantom_sensCorrection_flipback_rtSUC.mat```: contains the registered uniform phantom room-temperature surface RF coil data for sensitivity-based B<sub>1</sub> correction with flipback
- ```phantom_sensCorrection_NOflipback_rtSUC.mat```: contains the registered uniform phantom room-temperature surface RF coil data for sensitivity-based B<sub>1</sub> correction without flipback
- ```B1_rx_hybrid_rtSUC.mat```: contains the B<sub>1</sub><sup>+</sup>-corrected uniform phantom room-temperature surface RF coil data for hybrid B<sub>1</sub> correction 

**FOLDER RAREmodel_data:**
-	```RAREraw_flipback_modelData.mat```: contains the RARE data (mean SI data, T<sub>1</sub> relaxation times and the FA values associated with different reference powers) acquired with a volume RF coil using RARE with flipback 
-	```RAREraw_NOflipback_modelData.mat```: contains the RARE data (mean SI data, T<sub>1</sub> relaxation times and the FA values associated with different reference powers) acquired with a volume RF coil using RARE without flipback

### Available tools

**FOLDER RAREmodel_fitting-toolbox:**
-	```model_fitting_tool.m```: sensitivity-based B<sub>1</sub> correction method 
-	```fitDataWithPolynomeFAT1_PRD.m```: model-based B<sub>1</sub> correction method 
-	```FOLDER PolyfitnTools```: contains Polyfitn Toolbox (MATLAB File Exchange, version available in 2018)
-	```FOLDER SymbolicPolynomials```: contains the needed symbolic polynomial functions, in case your MATLAB does not have installed the Symbolic Math Toolbox 

**FOLDER B1correction-toolbox:**
-	```sensitivity_correction.m```: computes a sensitivity-based B<sub>1</sub> correction 
-	```modelBased_correction.m```: computes a model-based B<sub>1</sub> correction
-	```hybridCorrection.m```: computes a hybrid B<sub>1</sub> correction 
-	```calculateB1correction.m```: function that performs the model-based B<sub>1</sub> correction method 
-	```calculateB1Pluscorrection.m```: function that performs the model-based B<sub>1</sub><sup>+</sup> correction method to ultimately compute a hybrid B<sub>1</sub> correction

**FOLDER validityAssessment_and_plotting-toolbox:**
-	```central_profile_plotter.m```: central profile plotting tool 
-	```PIUcalculation_uniform.m```: percentage integral uniformity calculator/plotting tool for the uniform phantom (cryoprobe)
-	```PIUcalculation_exVivo.m```: percentage integral uniformity calculator/plotting tool for the ex vivo phantom (cryoprobe)
-	```PIUcalculation_inVivo.m```: percentage integral uniformity calculator/plotting tool for the in vivo phantom (cryoprobe)
-	```quantification_T1contrast_validation.m```: computes relative errors on T<sub>1</sub> contrast and quantification to compare the performance between the three B<sub>1</sub> correction methods and the original image (no correction) against a reference image (volume resonator) 


## Reproducing my results
### Available results data 
The directory ```results_folder``` corresponds to the resulting data shown in the peer-reviewed publication:

> Ramos Delgado P, Kuehne A, Periquito J, et al. B<sub>1</sub> Inhomogeneity Correction of RARE MRI with Transceive Surface Radiofrequency Probes. Magnetic Resonance in Medicine 2020; 00:1-20. https://doi.org/10.1002/MRM.28307 

**FOLDER RAREmodels:**
- ```RAREmodel_flipback_allOrders.mat```: calculated RARE model with flipback with polynomial orders 1<sup>st</sup>-8<sup>th</sup> 
- ```RAREmodel_NOflipback_allOrders.mat```: calculated RARE model without flipback with polynomial orders 1<sup>st</sup>-8<sup>th</sup> 

Take into account that the 7<sup>th</sup> order was used in the publication.

**FOLDER B1correction:**

**SUBFOLDER cryoprobe:**
- ```sensCorrection_uniform.mat```: sensitivity-based B<sub>1</sub>-corrected uniform phantom (cryoprobe data)
- ```modelBasedCorrection_uniform.mat```: model-based B<sub>1</sub>-corrected uniform phantom (cryoprobe data)
- ```hybridCorrection_uniform.mat```: hybrid B<sub>1</sub>-corrected uniform phantom (cryoprobe data)
- ```sensCorrection_exVivo.mat```: sensitivity-based B<sub>1</sub>-corrected ex vivo phantom (cryoprobe data)
- ```modelBasedCorrection_exVivo.mat```: model-based B<sub>1</sub>-corrected ex vivo phantom (cryoprobe data)
- ```hybridCorrection_exVivo.mat```: hybrid B<sub>1</sub>-corrected ex vivo phantom (cryoprobe data)
- ```sensCorrection_inVivo.mat```: sensitivity-based B<sub>1</sub>-corrected in vivo mouse (cryoprobe data)
- ```modelBasedCorrection_inVivo.mat```: model-based B<sub>1</sub>-corrected in vivo mouse (cryoprobe data)
- ```hybridCorrection_inVivo.mat```: hybrid B<sub>1</sub>-corrected in vivo mouse (cryoprobe data)

**SUBFOLDER rtSUC:**
- ```sensCorrection_flask_n#_flipback.mat```: sensitivity-based B<sub>1</sub>-corrected validation phantom (cryoprobe data, with flipback)
- ```modelBasedCorrection_flask_n#_flipback.mat```: model-based B<sub>1</sub>-corrected validation phantom (cryoprobe data, with flipback)
- ```hybridCorrection_flask_n#_flipback.mat```: hybrid B<sub>1</sub>-corrected uniform validation (cryoprobe data, with flipback)
- ```sensCorrection_flask_n#_NOflipback.mat```: sensitivity-based B<sub>1</sub>-corrected validation phantom (cryoprobe data, without flipback)
- ```modelBasedCorrection_flask_n#_NOflipback.mat```: model-based B<sub>1</sub>-corrected validation phantom (cryoprobe data, without flipback)
- ```hybridCorrection_flask_n#_NOflipback.mat```: hybrid B<sub>1</sub>-corrected uniform validation (cryoprobe data, without flipback)
- ```validationPhantoms_data_flipback.mat```: contains the original data, corrected data (model-based, hybrid, sensitivity using RARE with flipback) and reference data of all n=4 validation phantoms ready for error computation.
- ```validationPhantoms_data_NOflipback.mat```: contains the original data, corrected data (model-based, hybrid, sensitivity using RARE without flipback) and reference data of all n=4 validation phantoms ready for error computation.

Check validation phantom notation.

**FOLDER validityAssessment_and_plotting:**

**FOLDER central_profile_plotter**
- ```profiles_uniform_7pix_normalizedDATA.mat/.png```: contain the normalized profile data for a 7-pixel central line on the uniform phantom (cryoprobe) and the corresponding PNG image
- ```profiles_exvivo_7pix_normalizedDATA.mat/.png```: contain the normalized profile data for a 7-pixel central line on the ex vivo phantom (cryoprobe) and the corresponding PNG image
- ```profiles_invivo_7pix_normalizedDATA.mat/.png```: contain the normalized profile data for a 7-pixel central line on the in vivo mouse (cryoprobe) and the corresponding PNG image

**FOLDER PIU_calculation:**
- ```PIU_values_uniform.mat/.png```: contain the PIU data of the 5 internally-tangential circular ROIs drawn on the uniform phantom (cryoprobe) and the corresponding PNG image
- ```PIU_values_exVivo.mat/.png```: contain the contains the PIU data of the ROIs corresponding to cortex and basal ganglia/thalamus (left/right) drawn on the ex vivo phantom (cryoprobe) and the corresponding PNG image
- ```PIU_values_inVivo.mat/.png```: contain the PIU data of the ROIs corresponding to cortex and basal ganglia/thalamus (left/right) drawn on the in vivo mouse (cryoprobe) and the corresponding PNG image

**FOLDER quantification_T1contrast_validation:**
- ```quant_T1contrast_errors_flipback.mat```: contains the relative errors on T<sub>1</sub> contrast and quantification of the three B<sub>1</sub> correction methods and the original image (no correction) against a reference image (volume resonator) using RARE with flipback
- ```quant_T1contrast_errors_NOflipback.mat```: contains the relative errors on T<sub>1</sub> contrast and quantification of the three B<sub>1</sub> correction methods and the original image (no correction) against a reference image (volume resonator) using RARE without flipback


## Contact
You are more than welcome to contact me at ramosdelgado.paula@gmail.com for any doubts on the post-processing or the publication. 

Also, don’t hesitate to let me know about any possible errors you might encounter using GitHub (https://github.com/pramosdelgado/B1correction-toolbox/issues) or my email address.

## License, authorship and citations
All the code is licensed under GNU GPLv3. 

The main author of this code is Paula Ramos Delgado.

The fitting tool used for signal intensity modelling was modified from:

> John D'Errico (2018). polyfitn https://www.mathworks.com/matlabcentral/fileexchange/34765-polyfitn), MATLAB Central File Exchange.

The code used for T<sub>1</sub> analysis was developed by João S. Periquito. For more information or assistance on this, please contact João Periquito (Github user: JoaoPeriquito).

