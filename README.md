# Running SVRTK in Gadgetron (Work-in-progress)

Gadgetron (https://github.com/gadgetron) is an open-source software framework designed for the reconstruction of medical images. The framework offers a versatile platform for developing data processing pipelines that handle medical image data, guiding it through a sequence of modular components called "Gadgets." These Gadgets enable the transformation of raw data into fully reconstructed images. Gadgetron encourages the creation and sharing of reconstruction modules, facilitating the integration of new Gadgets into the system. The framework primarily supports C/C++ for Gadget implementation, while also including wrapper Gadgets to incorporate modules developed in the Python scripting language.

Integrating the SVRTK reconstruction within the Gadgetron framework allows to perform the slice-to-volume reconstruction process (conversion of 2D image slices into a complete 3D volume), becoming a modular component within Gadgetron's data processing pipelines. This integration allows exporting the raw acquisitions to an external server to first be reconstructed into 2D image slices, stored in the NIFTI format, and once all HASTE sequences have been acquired the SVRTK docker is launched to perform slice-to-volume reconstruction. The main advantages of this implementation are the automatization of SVRTK, reducing the workload of radiographers, and the availability of the resulting 3D reconstructions in the duration of the fetal scan.

The repository and the code were created by Sara Neves Silva.

![diagram-svrtk-gadgetron](https://github.com/SVRTK/gadgetron-svrtk-integration/assets/72754856/1b1f3e79-8cca-40cb-9a35-d956d70f8415)

## Requirements

To set up the Gadgetron development environment, please follow the instructions provided here https://github.com/hansenms/gadgetron/tree/hansenms/conda-install.

## Testing the Pipeline Outside of the Scanner

Please refer to the instructions from https://github.com/gadgetron/GadgetronOnlineClass to get familiarized with the Gadgetron framework. Follow the steps below to test the Gadgetron pipeline in your machine before running it in the scanner.

### Step 1: Convert the raw Siemens data to ISMRMRD format (required by Gadgetron):

1) Example:
```bash
siemens_to_ismrmrd -f meas_MID00335_FID24113_haste_cor_wholeuterus.dat --skipSyncData -x IsmrmrdParameterMap_Siemens_NX50.xsl -z 2 -o meas_MID00335_FID24113_haste_cor_wholeuterus.h5
```
