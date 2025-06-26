# Source Selection for Abell 360 in ComCam Data

```{abstract}
We cover the source selection done for the Abell 360 ComCam cluster study focusing on color and photo-z cuts. We present basic photometric validation and offer an independent validation suite for photo-z work. The various cuts are then used to generate N(z)s and shear profiles. We find that both cuts are quite robust to various choices made and can produce a shear profile for Abell 360.
```


## Introduction 
The Rubin ComCam observing campaign undertaken at the end-of-year 2024 covered seven fields, among which the low-latitude Rubin SV 38 7 field. This field contains the Abell 360 galaxy cluster, an intermediate mass cluster (quote mass + ref) at z=0.22, that we use as a commissioning demonstrator of Rubin capabilities for cluster WL studies. 
In order to best measure the shear profile of the cluster, we would ideally only measure the shapes of galaxies that are lensed by the cluster, i.e. source galaxies that are behind the cluster. This can be done by identifying and removing cluster members via the red sequence, an overdensity on a color-magnitude diagram caused by the cluster members, or by removing objects near and below the redshift of the cluster. This TechNote goes over both methods to define the source selection of galaxies used to measure the shear profile. We will discuss both methods, associated verification, the estimated n(z) from each method, and the shear profile from the various sources.


## Dataset
The Rubin SV 38 7 field has been observed in g (44 visits), r (55 visits), i (57 visits) and z (27 visits) (reference for these numbers? Probably in the DP1 paper) with no visits in u and y-band.
The DP1 catalog includes various flux measurements including cModel and gaap along with HSM shape measurements.
The HSM resolution factor, defined as $R = 1-\frac{T_\textrm{PSF}}{T_\textrm{I}}$, can be calculated from the catalog.
Photometric redshift estimates are not included in the DP1 catalog but have been created as outlined in [PZ Technote/paper].
Similarly, details on PSF and photometric calibration are located in [PSF] and [Photometric calibration]. We will focus on the various cuts made to generate the cluster lensing sample for Abell 360. 


We apply basic quality flags to remove objects that have caused a failure in the flux measurements, namely from saturation, cosmic rays, and spurious detections.
The flags and number of objects affected by the flag are listed in Table .
Within 0.5 degrees of the BCG located at (ra, dec) = (37.865017, 6.982205) we have 58676 objects with measurements in griz bands.
The counts per bin is shown in Fig\@ref{galaxydist} with griz bands reaching a depth of at 24.9, 24.5, 24.3, and 23.6 mags respectively.
Star-galaxy separation is done using the `refExtendedness` flag which will flag objects if $\frac{\textrm{flux}_\textrm{psf}}{\textrm{flux}_\textrm{cModel}} < .985$ as galaxies giving 52309 galaxies (89%) and 6367 stars (11%).

```{figure} _static/galaxy_dist.png

Distribution of galaxy magnitudes with (approximate) limiting magnitude shown as histogram with griz corresponding to blue, orange, green, and red and the magnitude limit per band shown as solid vertical line in the same color.
```


## Color-cuts

We can select cluster members by identifying the red sequence [citation?] in a color-magnitude diagram. Due to the lack of observations in z we focus on the gri bands and the colors (g-r, r-i, g-i) from this set. To correct for varying aperture sizes, we use gaap fluxes when combining bands for colors and use the cModel flux when calculating magnitudes. To identify the red sequence we first restrict to detections within 0.1 degrees (1.3 Mpc) of the BCG which leaves 2194 galaxies. 
