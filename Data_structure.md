Dataset Description
Files
fs_sentinel_cyprus_2022_buffer5_interp.csv - This file contains a time series of Sentinel data.

meta/cyprus_2022_meta.csv - metadata file for associate fields with the respective crop labels

ID - A unique identifier assigned to each data entry.
FIELD_ID - A unique identifier for each agricultural field.
APPLICANT_ID - A unique identifier for the field's farmer.
CROP_CODE - The code representing the cultivated crop.
CROP_NAME - The name of the cultivated crop.
CROP_FAMILY - The family to which the cultivated crop belongs.
AREA - The area of the field, measured in square meters.
N_PIXELS - The number of Sentinel pixels in the time series provided for each field entry.
DATASET - Indicates whether the data entry belongs to the training or test (cases to predict) subset.
N_IMAGES - The number of street-level images provided for each field entry.
STREET_LEVEL_IMAGES - The names of street-level images associated with each field entry.
meta/cyprus_2022_meta.shp - This file contains geographic information about the fields.

meta/cyrpus_2022_street_level_meta.csv - This file contains metadata for street-level images.

Directory
street_level/ - This directory contains the street-level photos.