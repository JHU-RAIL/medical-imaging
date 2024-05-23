
# File Formats

## DICOM

### General Info
* DICOM stores 1 image per file
* 3D and 4D datasets are stored in a directory and can have 100s of files
* Filenames are stored with a UID
* Four levels of hierarchy
  * Patient
    Information about the person
  * Study
    Information about “today’s scan”
  * Series
    A series is a single scan method (e.g., diffusion MRI scan)
  * Instance
    The slice information

https://pydicom.github.io/pydicom/stable/

### Python Install

```bash
$ pip install pydicom
```

### Usage in Python

```python
import pydicom

image = pydicom.load('filename.dcm')

```

## NIFTI
* Started out as “Analyze” file format
    .hdr and .img files separate

* Header and data in one file (“.nii” or “.nii.gz”)
  * 348 byte header

  * 4+ dimensions of data can be stored (including pixel “size”)

* Complete set of data types (8 bit to 128 bit)

* Affine coordinate definitions relating voxel index to real world spatial index

`$ pip install nibabel`

## MHA

Associated with ITK

Stores information when processed (e.g., registration or segmentation)
through ITK

`$ pip install medpy`

## JPG / PNG / TIFF

* Most common file format available, digital photography
* Lossy or lossless compression

`$ pip install imageio`

## BRIK

“.brik” file format used primarily with AFNI software

## MINC
* Medical Image NetCDF out of McGill
* “.mnc”
* https://www.mcgill.ca/bic/software/minc
