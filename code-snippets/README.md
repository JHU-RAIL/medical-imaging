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

  