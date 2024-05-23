
# File Formats

## DICOM

File suffix: .dcm or .dicom

Python Package: https://github.com/pydicom/pydicom

DICOM images are the most common images used by scanner and PACS system vendors.  Typically a DICOM images is a single 2D image and has a very large header containing patient, scanner and imaging information.  A multi-slice or volume scan is typically represented as a set of 2D images in a single directory.  To convert a DICOM directory into a NIFTI file one can use [https://github.com/rordenlab/dcm2niix/releases](https://github.com/rordenlab/dcm2niix/releases). 

  
```bash
$ pip install pydicom
```


```python
import pydicom

image = pydicom.load('filename.dcm')

```

## NIFTI
File suffix: .nii or .nii.gz

Python Package: https://nipy.org/nibabel/

This is one of the most common file formats to use for medical image processing.  It has a small header (348 bytes) and followed by the data. It will encode 2D, 3D or 4D data into the one file which makes it easy to copy the data.  The header contains only imaging information (pixel size, image shape, geometry information) and does not have the ability to hold patient information (other than a short description field which is typically empty).  This helps with de-identification.

This is a typical common (or preferred) format for most medical image processing. 

If it ends in ".gz" it is a compressed NIFTI file. All programs will read either uncompressed (.nii) or compressed (.nii.gz) NIFTI files. There is no need to uncompress the file first.


```bash
$ pip install nibabel
```

```python
import nibabel as nib

n = nib.load('filename.nii.gz')

print(n) # header

data = n.get_fdata()
```


## MHA

File suffix: .mha

Python Package: https://pypi.org/project/MedPy/

MHA Files come primarily from ITK software and contain both meta-data and the binary image data. They are able to contain both 2D and 3D data. They are less common for standard medical imaging but are the typical output from ITK software.

`$ pip install medpy`

## NRRD
File suffix: .nrrd

Python Package: [https://pypi.org/project/pynrrd/](https://pypi.org/project/pynrrd/)

Nrrd is a simple file format designed to support scientific visualization and image processing involving N-dimensional raster data. Nrrd stands for "nearly raw raster data". The header is text with one field per line and the images are binary data.

## JPG / PNG / TIFF

* Most common file format available, digital photography
* Lossy or lossless compression

`$ pip install imageio`


## BRIK

“.brik” file format used primarily with AFNI software

## MINC

File suffix: .mnc

Python Package: https://nipy.org/nibabel/

The MINC file format came out of McGill and is a simple file format based on NetCDF.
