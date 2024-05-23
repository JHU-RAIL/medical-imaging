# Code Snippets


## Matplotlib Image Overlay

This is from [this StackOverflow post](https://stackoverflow.com/questions/31877353/overlay-an-image-segmentation-with-numpy-and-matplotlib)

```python 
  import numpy as np
  mask = np.zeros((10,10))
  mask[3:-3, 3:-3] = 1 # white square in black background
  im = mask + np.random.randn(10,10) * 0.01 # random image
  masked = np.ma.masked_where(mask == 0, mask)
  
  import matplotlib.pyplot as plt
  plt.figure()
  plt.subplot(1,2,1)
  plt.imshow(im, 'gray', interpolation='none')
  plt.subplot(1,2,2)
  plt.imshow(im, 'gray', interpolation='none')
  plt.imshow(masked, 'jet', interpolation='none', alpha=0.7)
  plt.show()
  ```

### 3D Interpolation

```python
  from pathlib import Path
  import numpy as np
  import nibabel as nib
  import matplotlib.pyplot as plt
  import scipy.ndimage as nd
  
  def interp3(x, y, z, v, xi, yi, zi, order, **kwargs):
      """Sample a 3D array "v" with pixel corner locations at "x","y","z" at the
      points in "xi", "yi", "zi" using linear interpolation. Additional kwargs
      are passed on to ``scipy.ndimage.map_coordinates``."""
      def index_coords(corner_locs, interp_locs):
          index = np.arange(len(corner_locs)) 
          if np.all(np.diff(corner_locs) < 0):
              corner_locs, index = corner_locs[::-1], index[::-1]
          return np.interp(interp_locs, corner_locs, index)
   
      orig_shape = np.asarray(xi).shape
      xi, yi, zi = np.atleast_1d(xi, yi, zi)
      for arr in [xi, yi, zi]:
          arr.shape = -1
   
      output = np.empty(xi.shape, dtype=float)
      coords = [index_coords(*item) for item in zip([x, y, z], [xi, yi, zi])]
   
      nd.map_coordinates(v, coords, order=order, output=output, **kwargs)
   
      return output.reshape(orig_shape)
  
  def resize(data, shape, order=1):
      x = np.linspace(0, data.shape[0], data.shape[0])
      y = np.linspace(0, data.shape[1], data.shape[1])
      z = np.linspace(0, data.shape[2], data.shape[2])
      
      xi, yi, zi = np.mgrid[0:data.shape[0]:(shape[0])*1j, 
                            0:data.shape[1]:(shape[1])*1j,
                            0:data.shape[2]:(shape[2])*1j]
                             
      return interp3(x, y, z, data, xi, yi, zi, order)
  

brain = resize(data, [112, 112, 32], 1)

```

## Alignment of NIFTI Files

```python
import nibabel as nib
import nilearn.image

n_t2 = nib.load(filename_t2)

n_t1 = nib.load(filename_t1)

n_t1_resampled = nilearn.image.resample_img(n_t1,
	target_affine=n_t2.affine,
    target_shape=n_t2.shape)
```

## TorchIO Subjects

```python
def reader(filename):
    n = nib.load(filename)
    return n.get_fdata(), np.eye(4)#n.affine

def mask_reader(filename_mask):
    n = nib.load(filename_mask)
    data = (n.get_fdata() >= 0.3).astype(np.uint8)
    return data, np.eye(4)#n.affine

#
#  Load the TorchIO Subjects data
#
subjects = {
    'train': [],
    'validation': [],
}

for tt in ['train', 'validation']:
    for fn in filenames[tt]:
        mask_filename = str(fn).replace(f't2.', f't2_mask.')

        *subject = tio.Subject(
            t1=tio.ScalarImage(fn, reader=reader),
            label=tio.LabelMap(mask_filename, reader=mask_reader)
        )

        subjects[tt].append(subject)
```

#### TorchIO Transforms

```python
signal = tio.Compose([ 
    tio.RescaleIntensity(percentiles=(0.1, 99.9), out_min_max=(0, 1)),
    tio.Clamp(out_min = 0, out_max = 1),
])

spatial = tio.OneOf({
        tio.RandomAffine(degrees=(-3,3),translation=(-0.1,0.1)): 1.0,
    },
    p=0.75,
)

resample = tio.Compose([ 
    #tio.Resample((1, 1, 3)), 
    tio.Lambda(lambda x: resize(x, (192, 192, 64)), types_to_apply=[tio.INTENSITY, tio.LABEL]),
    tio.CropOrPad((192, 192, 64)) ,
#    tio.Lambda(lambda x: torch.cat((x, x, x),axis=0), types_to_apply=[tio.INTENSITY]),
])

transform = {
    'train': tio.Compose([spatial, resample, signal]),
    'validation': tio.Compose([resample, signal]),
}
```