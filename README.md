# Running SVRTK in Gadgetron (Work-in-progress)

Gadgetron (https://github.com/gadgetron) is an open-source software framework designed for the reconstruction of medical images. The framework offers a versatile platform for developing data processing pipelines that handle medical image data, guiding it through a sequence of modular components called "Gadgets." These Gadgets enable the transformation of raw data into fully reconstructed images. Gadgetron encourages the creation and sharing of reconstruction modules, facilitating the integration of new Gadgets into the system. The framework primarily supports C/C++ for Gadget implementation, while also including wrapper Gadgets to incorporate modules developed in the Python scripting language.

Integrating the SVRTK reconstruction within the Gadgetron framework allows to perform the slice-to-volume reconstruction process (conversion of 2D image slices into a complete 3D volume), becoming a modular component within Gadgetron's data processing pipelines. This integration allows exporting the raw acquisitions to an external server to first be reconstructed into 2D image slices, stored in the NIFTI format, and once all HASTE sequences have been acquired the SVRTK docker is launched to perform slice-to-volume reconstruction. The main advantages of this implementation are the automatization of SVRTK, reducing the workload of radiographers, and the availability of the resulting 3D reconstructions in the duration of the fetal scan.

The repository and the code were created by Sara Neves Silva.

![diagram-svrtk-gadgetron](https://github.com/SVRTK/gadgetron-svrtk-integration/assets/72754856/1b1f3e79-8cca-40cb-9a35-d956d70f8415)

## Requirements

To set up the Gadgetron development environment, please follow the instructions provided here https://github.com/hansenms/gadgetron/tree/hansenms/conda-install.

## Testing the Pipeline Outside of the Scanner

Please refer to the instructions from https://github.com/gadgetron/GadgetronOnlineClass to get familiarized with the Gadgetron framework. Follow the steps below to test the Gadgetron pipeline in your machine before running it in the scanner.

### Step 1: 
Open a terminal in your Ubuntu machine and activate the Gadgetron development environment. In this terminal, convert the raw Siemens data to ISMRMRD format (required by Gadgetron):

Example:
```bash
siemens_to_ismrmrd -f meas_MID00335_FID24113_haste_cor_wholeuterus.dat --skipSyncData -x IsmrmrdParameterMap_Siemens_NX50.xsl -z 2 -o meas_MID00335_FID24113_haste_cor_wholeuterus.h5
```
Make sure to use the provided parameter style sheet (IsmrmrdParameterMap_Siemens_NX50.xsl) if converting a dataset acquired in a Siemens scanner of software version NX50. If the dataset was acquired in a scanner of version below NX50, please use the IsmrmrdParameterMap_Siemens_NX.xsl style sheet.

### Step 2: 
Copy the converted ISMRMRD dataset (with .h5 extension) to /miniconda3/envs/gadgetron/share/gadgetron/config.

### Step 3: 
Copy the provided configuration gadgetron_svrtk.xml file /miniconda3/envs/gadgetron/share/gadgetron/config.

### Step 4:
Copy the Python script nifti_python_gadgetron_haste.py into /miniconda3/envs/gadgetron/share/gadgetron/python.

### Step 5:
Open a second terminal and also activate the Gadgetron development environment. Make sure both terminals are open in the same directory - this should be /config directory where both the .xml pipeline file and dataset are stored. Initialize gadgetron as follows:

```bash
gadgetron
```
### Step 6: 
You are good to go! You can now run the Gadgetron SVRTK pipeline as follows:

Example:
```bash
gadgetron_ismrmrd_client -f meas_MID00335_FID24113_haste_cor_wholeuterus.h5 -C gadgetron_svrtk.xml -o meas_MID00335_FID24113_haste_cor_wholeuterus_r001.h5
```
