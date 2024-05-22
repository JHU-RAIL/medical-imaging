
# File Formats


## DICOM

DICOM stores 1 image per file

3D and 4D datasets are stored in a directory and can have 100s of files

Filenames are stored with a UID

Four levels of hierarchy

Patient

Information about the person

Study

Information about “today’s scan”

Series

A series is a single scan method (e.g., diffusion MRI scan)

Instance

The slice information

## NIFTI

DICOM stores 1 image per file

3D and 4D datasets are stored in a directory and can have 100s of files

Filenames are stored with a UID

Four levels of hierarchy

Patient

Information about the person

Study

Information about “today’s scan”

Series

A series is a single scan method (e.g., diffusion MRI scan)

Instance

The slice information

## MHA

Associated with ITK

Stores information when processed (e.g., registration or segmentation)
through ITK

Use “pip install medpy” to read MHA files in Python

## JPG / PNG / TIFF

JPEG image files

Most common file format
available, digital photography

Lossy or lossless compression

Up to 10:1 compression

PNG

Raster file format

Supports lossless compression

Transparency

## BRIK

“.brik” file format used primarily with AFNI software