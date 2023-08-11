Running SVRTK in Gadgetron

Gadgetron is an open-source software framework designed for the reconstruction of medical images. The framework offers a versatile platform for developing data processing pipelines that handle medical image data, guiding it through a sequence of modular components called "Gadgets." These Gadgets enable the transformation of raw data into fully reconstructed images. Gadgetron encourages the creation and sharing of reconstruction modules, facilitating the integration of new Gadgets into the system. The framework primarily supports C/C++ for Gadget implementation, while also including wrapper Gadgets to incorporate modules developed in the Python scripting language.

Integrating the SVRTK reconstruction within the Gadgetron framework allows to perform the slice-to-volume reconstruction process (conversion of 2D images slices into a complete 3D volume), becoming a modular component within Gadgetron's data processing pipelines. This integration allows to export the raw acquisitions to an external server to first be reconstructed into 2D image slices, stored in the NIFTI format, and once all HASTE sequences have been acquired the SVRTK docker is launched to perform slice-to-volume reconstruction. The main advantages on this implementation is the automatization of SVRTK, reducing the workload of radiographers, and the availability of the resulting 3D reconstructions in the duration of the fetal scan.

Requirements

To set up the Gadgetron development environment, please follow the instructions provided here https://github.com/hansenms/gadgetron/tree/hansenms/conda-install.
