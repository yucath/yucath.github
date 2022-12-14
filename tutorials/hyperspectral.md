### Needed Libraries
> [spectral]( conda install -c conda-forge spectral){:target="_blank"}
> numpy
> matplotlib
> opencv
> pandas
> os


#### Import all the needed libraries
```
from spectral import imshow, view_cube
import spectral.io.envi as envi
import numpy as np
import matplotlib.pyplot as plt
import matplotlib
import cv2
import os
import pandas as pd
```

#### Read the hyperspectral image
The hyperspectral image needs three different images to get a corrected image - *raw data* (raw reflectance values from the camera), *white ref* - which is the reflectance from white calibration surface, and *dark ref* - the refelctance from dark (usually zeros). The white and dark references are used to calibrate and normalise the raw refelctance values using the equation:

```{math}

    Calibrated Image = \frac{Raw reflectance - Dark Reflectance}{White Reflectance - Dark Reflectance}
```

```
dark_ref = envi.open('path_to_dark_ref.hdr','path_to_dark_ref')
white_ref = envi.open('path_to_white_ref.hdr','path_to_white_ref')
raw_ref = envi.open('path_to_raw.hdr','path_to_raw)
```

#### Load the images

````
bands = data_ref.bands.centers
white_data = np.array(white_ref.load())
dark_data = np.array(dark_ref.load())
raw_data = np.array(data_ref.load())
````

#### Normalised (corrected) image



```
corrected_data = np.divide(
            np.subtract(data_data, dark_data),
            np.subtract(white_data, dark_data))

```