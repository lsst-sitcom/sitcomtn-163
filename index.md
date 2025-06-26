# Source Selection for Abell 360 in ComCam Data

```{abstract}
We cover the source selection done for the Abell 360 ComCam cluster study focusing on color and photo-z cuts. We present basic photometric validation and offer an independent validation suite for photo-z work. The various cuts are then used to generate N(z)s and shear profiles. We find that both cuts are quite robust to various choices made and can produce a shear profile for Abell 360.
```


## Introduction 
The Rubin ComCam observing campaign undertaken at the end-of-year 2024 covered seven fields, among which the low-latitude Rubin SV 38 7 field. This field contains the Abell 360 galaxy cluster, an intermediate mass cluster (quote mass + ref) at z=0.22, that we use as a commissioning demonstrator of Rubin capabilities for cluster WL studies. 
In order to best measure the shear profile of the cluster, we would ideally only measure the shapes of galaxies that are lensed by the cluster, i.e. source galaxies that are behind the cluster. This can be done by identifying and removing cluster members via the red sequence, an overdensity on a color-magnitude diagram caused by the cluster members, or by removing objects near and below the redshift of the cluster. This TechNote goes over both methods to define the source selection of galaxies used to measure the shear profile. We will discuss both methods, associated verification, the estimated n(z) from each method, and the shear profile from the various sources.


## Dataset
The Rubin SV 38 7 field has been observed in g (44 visits), r (55 visits), i (57 visits) and z (27 visits) {cite:p}`RTN-011` with no visits in u and y-band. {cite:p}`DMTN-242`
The DP1 catalog includes various flux measurements including cModel and gaap along with HSM shape measurements. 
The HSM resolution factor, defined as $R = 1-\frac{T_\textrm{PSF}}{T_\textrm{I}}$, can be calculated from the catalog.
Photometric redshift estimates are not included in the DP1 catalog but have been created as outlined in {cite:p}`SITCOMTN-154`.
Similarly, details on PSF and photometric calibration are located in {cite:p}`SITCOMTN-161` and __photometric calibration technote?__ . We will focus on the various cuts made to generate the cluster lensing sample for Abell 360. 


We apply basic quality flags to remove objects that have caused a failure in the flux measurements, namely from saturation, cosmic rays, and spurious detections.
The flags and number of objects affected by the flag are listed in Table .
Within 0.5 degrees of the BCG located at (ra, dec) = (37.865017, 6.982205) we have 58676 objects with measurements in griz bands.
The counts per bin is shown in Fig  with griz bands reaching a depth of at 24.9, 24.5, 24.3, and 23.6 mags respectively.
Star-galaxy separation is done using the `refExtendedness` flag which will flag objects if $\frac{\textrm{flux}_\textrm{psf}}{\textrm{flux}_\textrm{cModel}} < .985$ as galaxies giving 52309 galaxies (89%) and 6367 stars (11%).

```{figure} _static/galaxy_dist.png

Distribution of galaxy magnitudes with (approximate) limiting magnitude shown as histogram with griz corresponding to blue, orange, green, and red and the magnitude limit per band shown as solid vertical line in the same color.
```


## Color-cuts

We can select cluster members by identifying the red sequence [citation?] in a color-magnitude diagram. Due to the lack of observations in z we focus on the gri bands and the colors (g-r, r-i, g-i) from this set. To correct for varying aperture sizes, we use gaap fluxes when combining bands for colors and use the cModel flux when calculating magnitudes. To identify the red sequence we first restrict to detections within 0.1 degrees (1.3 Mpc) of the BCG which leaves 2194 galaxies. 
The color-magnitude diagram is shown below


```{figure} _static/CMD_nors.png

Color-magnitude diagrams for galaxies near the BCG using cModel r magnitude on x-axis and gaap calculated colors on the y-axis. From left to right we show the, g-i, r-i, and g-r colors with the red sequence overdensity clearly visible. 
```

The red sequence is identified per color by eye and shown in Fig _ giving 286, 316, 286 objects in the g-i, r-i, and g-r CMDs respectively. Requiring that an object be in all three red sequences restricts us to 216 objects and is shown in Fig _.

```{figure} _static/CMD_rs.png

Red sequence members identified by eye and adding a magnitude limit of $r < 24$.
```

```{figure} _static/CMD_all_rs.png

Multi-color red sequence members.
```

When applying the color cuts to the entire sample, we remove 873 galaxies and remain with 57803 galaxies. 

## Photo-Z Selection

Photometric redshift (photo-z) selection is another option to remove any objects that would not be lensed by the cluster and thus dilute the signal.
The photo-zs were estimated using FlexZBoost {cite:p}`Flexzboost1, Flexzboost2` and TreePhotoZ {cite:p}`TPZ` as implemented in RAIL {cite:p}`RAIL` using the 4 available bands griz on cModel fluxes.
This estimator was trained on the same bands in the ECDFS field when crossed with a variety of spectroscopic, grism, and high quality photometric redshifts totaling 22,720 redshifts.
Additional details on photo-z estimates and validation can be found in the {cite:p}`SITCOMTN-154`.
The cluster redshift is 0.22 {cite:p}`A360z` and we place a simple cut on point estimates as photo-z > 0.3, 0.4, or 0.5.

We plot the redshift distribution for the two methods in Fig _ .
In this field, we have overlap with the DESI {cite:p}`DESI-dr1` BGS {cite:p}`BGS`, ELG {cite:p}`ELG`, and LRG {cite:p}`LRG` catalogs  which allows us some external validationwhich we use to assess the two redshift techniques shown in Fig _.

```{figure} _static/fzb_tpz.png

Photometric redshift distribution for the galaxy sample (within 0.5 deg, or 6.4 Mpc, of the central BCG) and with i < 24. Blue histogram is using the FlexZBoost median estimate, orange histogram for TreePZ median, and the black line represents the cluster location. 
```

```{figure} _static/desi_location.png

Overlap between SV 38-7 field and various DESI subsamples. Note that the BGS sample that covers the low-z regime does not uniformly cover the region and thus we are incomplete in this space.
```

```{figure} _static/desi_validation.png

Validation for FlexZBoost(FlexZBoost) and TPZ (TreePhotoZ) with spectroscopic redshift on x-axis and estimated photometric redshift on y-axis. 
```


## Comparison

We compare the two methods using n(z) and the shear profile. We use ECDFS photo-zs to estimate a n(z) distribution from color cuts by applying the same cuts to both profiles. This lets us use higher quality photo-z estimates in a 6 band field while maintaining a very similar selection function. ECDFS is observed to greater depth but applying a magnitude cut of $i < 24$ should mimic the SV 38-7 limit. To create an n(z) estimate we simply stack the the point photo-z estimates


```{figure} _static/shear_profile_cc.png

The shear profile with varying color cuts. The tangential shear component is shown with circular points and cross shear component with x points. The error bar represents a 95% confidence interval from bootstrapped samples. “All colors” refers to using all three red sequences and is shown in dark blue, "g-i" uses points found only on the $g-i$ red sequence and is shown in light blue, "r-i" for the $r-i$ redsequence and shown in light red, and "g-r" for the $g-r$ redsequence and shown in dark red. The points are staggered on the x-axis for clarity. 
```

```{figure} _static/shear_profile_pz.png

The shear profile with varying photo-z cuts. Same formatting as above figure. The cut with photo-z > 0.22 is shown in dark blue, photo-z > 0.3 is shown in light blue, photo-z > 0.4 in light red, and photo-z > 0.5 in dark red. 
```

```{figure} _static/fzb_nz.png

The estimated n(z) using the FZB with the various photo-z cuts imposed as dashed lines. 
```

```{figure} _static/ECDFS_nz.png

The estimated n(z) using ECDFS FZB estimates and applying the "all colors" color cut.
```


## References 

```{bibliography}
```
