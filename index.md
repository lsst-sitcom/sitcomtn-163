# Source Selection for Abell 360 in ComCam Data

```{abstract}
We cover the source selection done for the Abell 360 ComCam cluster study focusing on color and photo-z cuts.
Identification of cluster members via the red sequence and photo-z offers a science focused validation of photometry with ComCam.
We are able to cross-match with DESI spectroscopic redshifts to offer an independent validation suite on photo-z estimates.
We make various color and photo-z cuts to generate N(z)s and shear profiles.
We find that both cuts are quite robust to various choices made and can produce a shear profile for Abell 360. Both methods should be applicable to generating a consistent mass estimate for the cluster.
```


## Introduction 
The Rubin ComCam observing campaign undertaken at the end-of-year 2024 covered seven fields, among which the low ecliptic latitude Rubin SV 38 7 field. This field contains the Abell 360 galaxy cluster, an intermediate mass cluster ($M_{\rm 500c} \sim 6 \times 10^{14}\ M_{\odot}$, e.g. {cite:p}``Planck) at z=0.22, that we use as a commissioning demonstrator of Rubin capabilities for cluster WL studies. 
In order to best measure the shear profile of the cluster, we would ideally only measure the shapes of galaxies that are lensed by the cluster, i.e. source galaxies that are behind the cluster. This can be done by identifying and removing cluster members via the red sequence, an overdensity on a color-magnitude diagram caused by the cluster members, or by removing objects near and below the redshift of the cluster. This TechNote goes over both methods to define the source selection of galaxies used to measure the shear profile. We will discuss both methods, associated verification, the estimated n(z) from each method, and the shear profile from the various sources.


## Dataset
The Rubin SV 38 7 field has been observed in *g* (44 visits), *r* (55 visits), *i* (57 visits) and *z* (27 visits) {cite:p}`RTN-011` with no visits in u and y-band. {cite:p}`DMTN-242`
The DP1 catalog includes various flux measurements including cModel and gaap along with HSM shape measurements {cite:p}`HSM1, HSM2`. 
The HSM resolution factor, defined as $R = 1-\frac{T_\textrm{PSF}}{T_\textrm{I}}$, can be calculated from the catalog.
Photometric redshift estimates are not included in the DP1 catalog but have been created as outlined in {cite:p}`SITCOMTN-154`.
Similarly, details on PSF and photometric calibration are located in {cite:p}`SITCOMTN-161` and photometric calibration technote . We will focus on the various cuts made to generate the cluster lensing sample for Abell 360. 

```{note}
Photometric calibration technote is still in draft and has not been made on SITCOM yet
```


We apply basic quality flags to remove objects that have caused a failure in the flux measurements, namely from saturation, cosmic rays, and spurious detections.
Each quality flag is a boolean which is set to True if the algorithm fails.
The flags and number of objects affected by the flag are listed in {numref}`quality_table`.
```{table} Quality flags and number of objects affected by flag.
:widths: auto
:align: center
:name: quality_table

| Flag Name | Number of Objects|
| --------- | ---------------- |
| refExtendedness | 24892 |
| i_iPSF_flag | 62462 |
| i_hsmSHapeRegauss_flag | 91836 |
| g_cModel_flag | 4016 |
| r_cModel_flag | 4228 |
| i_cModel_flag | 4137 |
| z_cModel_flag | 3173 |
| g_gaapFlux_flag | 0 |
| r_gaapFlux_flag | 0 |
| i_gaapFlux_flag | 6 |
| z_gaapFlux_flag | 0 |
```
Within 0.5 degrees of the brightest central galaxy (BCG) located at (RA, DEC) = (37.865017, 6.982205) we have 58676 objects with measurements in *griz* bands.
The counts per bin is shown in {numref}`galaxy-dist`  with *griz* bands reaching a depth of at 24.9, 24.5, 24.3, and 23.6 mags respectively.
Star-galaxy separation is done using the `refExtendedness` flag which will flag objects if $\frac{\textrm{flux}_\textrm{psf}}{\textrm{flux}_\textrm{cModel}} < .985$ as galaxies giving 52309 galaxies (89%) and 6367 stars (11%).

```{figure} _static/galaxy_dist.png
:name: galaxy-dist

Distribution of galaxy magnitudes with (approximate) limiting magnitude shown as histogram with *griz* corresponding to blue, orange, green, and red and the magnitude limit per band shown as solid vertical line in the same color.
```


## Color-cuts

We can select cluster members by identifying the red sequence [citation?] in a color-magnitude diagram.
Due to the lack of observations in *z* we focus on the gri bands and the colors (*g-r*, *r-i*, *g-i*) from this set.
To correct for varying aperture sizes, we use gaap fluxes when combining bands for colors and use the cModel flux when calculating magnitudes.
To identify the red sequence we first restrict to detections within 0.1 degrees (1.3 Mpc) of the BCG which leaves 2194 galaxies. 
The color-magnitude diagram is shown below in {numref}`CMD_nors`


```{figure} _static/CMD_nors.png
:name: CMD_nors

Color-magnitude diagrams for galaxies near the BCG using cModel r magnitude on x-axis and gaap calculated colors on the y-axis. From left to right we show the *g-i*, *r-i*, and *g-r* colors with the red sequence overdensity clearly visible. 
```

The red sequence is identified per color by eye with a limiting magnitude of $r < 24$ also applied and shown in {numref}`CMD_rs` giving 286, 316, 286 objects in the *g-i*, *r-i*, and *g-r* CMDs respectively. Requiring that an object be in all three red sequences restricts us to 216 objects and is shown in {numref}`CMD_all_rs`.

```{figure} _static/CMD_rs.png
:name: CMD_rs

Red sequence members identified by eye and adding a magnitude limit of $r < 24$. From left to right we show the *g-i*, *r-i*, and *g-r* colors.
 
```

```{figure} _static/CMD_all_rs.png
:name: CMD_all_rs

Multi-color red sequence members. From left to right we show the *g-i*, *r-i*, and *g-r* colors. 
```

When applying the color cuts to the entire sample, we remove 873 galaxies and remain with 57803 galaxies. 

## Photo-Z Selection

Photometric redshift (photo-*z*) selection is another option to remove any objects that would not be lensed by the cluster and thus dilute the signal.
The photo-*z*s were estimated using FlexZBoost (FZB) {cite:p}`Flexzboost1, Flexzboost2`, TreePhotoZ (TPZ) {cite:p}`TPZ`, k-Nearest Neighbors (kNN) {cite:p}`RAIL`, and Bayesian Photo-zs (BPZ) {cite:p}`BPZ` as implemented in RAIL {cite:p}`RAIL` using the 4 available bands *griz* on cModel fluxes.
These estimators were chosen as kNN and BPZ will be provided by the Rubin project for future data releases while we expect FZB and TPZ to be readily available from the community.
The estimators were trained on the same 4 bands in the ECDFS field and then applied to the cluster field. Since the cluster redshift is 0.22 {cite:p}`A360z`, we can place a simple cut on the photo-*z* point estimates to remove any galaxies that would be unlensed by the cluster.

We plot the redshift distribution for kNN and BPZ in {numref}`knn_bpz` and for FZB and TPZ in {numref}`fzb_tpz`.

```{note}

Add Eric's $\sigma_z < 0.15$ quality cut
```

```{figure} _static/bpz_knn.png
:name: knn_bpz

Photometric redshift distribution for the galaxy sample (within 0.5 deg, or 6.4 Mpc, of the central BCG) and with i < 24. Blue histogram is using the kNN median estimate, orange histogram for BPZ median, and the black line represents the cluster location. A faint red line is added to display the location of an additional photo-*z* cut used, see text for motivation in choosing these additional cuts.
```

```{figure} _static/fzb_tpz.png
:name: fzb_tpz

Photometric redshift distribution for the galaxy sample (within 0.5 deg, or 6.4 Mpc, of the central BCG) and with i < 24. Purple  histogram is using the FlexZBoost median estimate, blue histogram for TreePZ median, and the black line represents the cluster location. A faint red line is added to display the location of an additional photo-*z* cut used, see text for motivation in choosing these additional cuts.
```

```{note}

Update the photo-z cuts to only have $z_{cl} = 0.22 + 0.15$
```
In this field, we have overlap with the DESI {cite:p}`DESI-dr1` BGS {cite:p}`BGS`, ELG {cite:p}`ELG`, and LRG {cite:p}`LRG` catalogs, shown in {numref}`desi_location`, which allows us to perform simple external validation on the four redshift techniques.
To validate the techniques, we compare the photo-z for objects with matches to the DESI sample which we show in {numref}`desi_validation_knnbpz` for kNN and BPZ and in {numref}`desi_validation_fzbtpz` for FZB and TPZ.
Both techniques suffer at the low and high $z$ range which can be attributed to the missing *u* and *y* bands but are overall acceptable.
For objects with spec-$z > 0.8$, it is possible that there is a mismatch between the training sample, observed sample, and validation sample which could cause systematic issues for the predictors.


```{figure} _static/desi_location.png
:name: desi_location

Overlap between SV 38-7 field and various DESI subsamples. Note that the BGS sample that covers the low-z regime does not uniformly cover the region and thus we are incomplete in this space. From {cite}`SITCOMTN-154`.
```

```{figure} _static/desi_validation_fzbtpz.png
:name: desi_validation_fzbtpz

Validation for FZB and TPZ with spectroscopic redshift on x-axis and estimated photometric redshift on y-axis. 
```

```{figure} _static/desi_validation_knnbpz.png
:name: desi_validation_knnbpz

Validation for kNN and BPZ with spectroscopic redshift on x-axis and estimated photometric redshift on y-axis. 
```

For kNN and BPZ, we make cuts at photo-$z = 0.22$ and $0.45$.
The first cut is directly at the cluster redshift and would remove most cluster members if we had perfect photo-*z*s and the second to remove the spike in the distribution from {numref}`knn_bpz`.
For FZB and TPZ, we make cuts at photo-$z = 0.22$ and $0.6$. The second cut from TPZ overestimating redshift of objects up until $z\approx 0.6$ as shown in {numref}`desi_validation_fzbtpz`.

For more details on the matching and performance of other photo-z estimators on this field, and DP1 in general, please refer to {cite}`SITCOMTN-154`.


```{note}

Add sub-section on red sequence + DESI plot. Text about validation with photometry and photo-z
```

## Shear profiles and N(z)
With a variety of source selection methods available, we can compare the $N(z)$ and final HSM shear profile from each source to see if the subsamples are relatively stable.
Following {cite}`HSC_wlen`, we also apply a set of weak lensing cuts to both samples to ensure that the sample has good quality shape measurements.
The cuts and values used are listed below in {numref}`weaklen_table`
```{table} Weak Lensing Cuts
:widths: auto
:align: center
:name: weaklen_table
| Column |
| ------ |
| refExtendedness > 0.5 | 
| i_cModel_mag $\leq$ 24 |
| $\frac{\textrm{i_cModelFlux}}{\textrm{i_cModelFluxErr}} \geq 10$ |
| i_hsmShapeRegauss_e1$^2$ + i_hsmShapeRegauss_e2$^2 \leq 4$ |
| $1-\frac{T_\textrm{PSF}}{T_\textrm{I}} \geq 0.3$ |
| i_blendedness $\leq 0.42$ |
```


### Shear Profile
To calculate the shear profile for each subsample when applying color cuts or photo-z cuts, we first convert from $e_1$ and $e_2$ to $\gamma_1$ and $\gamma_2$ by applying HSC Y1 shear calibration {cite:p}`HSCCalib1, HSCCalib2`.
<!--For other shear calibration techniques please see [Anacal technote] for results when applying Anacal, and {cite}`SITCOMTN-162` for Metadetect.-->
Once calibrated, we can convert the shears to tangential and cross copmonents using the cluster BCG, located at (RA, DEC) = (37.865017, 6.982205), using 
```{math}
\gamma_+ &= -\gamma_1 \cos{2\phi} - \gamma_2\sin{2\phi} \\
\gamma_\times &= \gamma_1 \sin{2\phi} - \gamma_2\cos{2\phi}
```

Once calibrated and converted, we can extract a shear profile centered on the BCG and vary the color cuts applied which we present in {numref}`shear_profile_cc`.
Notably, the profile is quite insensitive to the specific color cut chosen with the tangential components located at similar values and the cross component mostly consistent with 0. 
Similarly, we can apply varied photo-*z* cuts which we show in {numref}`shear_profile_fzb` for FZB and TPZ, and in {numref}`shear_profile_knn` for kNN and BPZ.
While there is a larger spread on the value of the $\gamma_+$ between the cuts, the signal is quite similar to the color-cut profile and similarly passes the null test of $\gamma_\times = 0$ aside from the innermost bin which only has $\sim 30$ galaxies.


```{figure} _static/shear_profile_cc.png
:name: shear_profile_cc

The shear profile with varying color cuts. The tangential shear component is shown with circular points and cross shear component with x points. The error bar represents a 95% confidence interval from bootstrapped samples. “All colors” refers to using all three red sequences and is shown in dark blue, "g-i" uses points found only on the $g-i$ red sequence and is shown in light blue, "r-i" for the $r-i$ redsequence and shown in light red, and "g-r" for the $g-r$ redsequence and shown in dark red. The points are staggered on the x-axis for clarity. 
```

```{figure} _static/shear_profile_pz_fzbtpz.png
:name: shear_profile_fzb

The shear profile with varying FZB photo-z cuts.  Same formatting as above figure. The cut with photo-*z* > 0.22 is shown in dark blue, photo-*z* > 0.4 is shown in light blue, photo-*z* > 0.6 in light red, and photo-z > 0.5 in dark red. 
```

```{figure} _static/shear_profile_pz_knnbpz.png
:name: shear_profile_knn

The shear profile with varying FZB photo-z cuts.  Same formatting as above figure. The cut with photo-*z* > 0.22 is shown in dark blue, photo-*z* > 0.4 is shown in light blue, photo-*z* > 0.6 in light red, and photo-z > 0.5 in dark red. 
```

### N(z)
To estimate the $N(z)$ when using color cuts, we use the ECDFS photo-*z*s and apply the same red sequence and weak lensing cuts.
The ECDFS field was observed in 6 bands which enables higher quality photo-*z* estimates and by using the same instrument, we have a very similar selection function between the two fields.
The weak lensing cut requiring $i < 24$ helps mitigate any difference in depth between the two samples. 
The $N(z)$ from color cuts is presented in {numref}`nz_knnbpz` for kNN and BPZ and in {numref}`nz_fzbtpz` for FZB and TPZ.

```{note}

Need to add $i_{SNR} \geq 10$ cut as well and update these N(z) to actually use the stacked PDFs
```

<!--```{figure} _static/fzb_nz.png
:name: fzb_nz

The estimated n(z) using the FZB with the various photo-z cuts imposed as dashed lines. 
```-->

```{figure} _static/ECDFS_nz_knnbpz.png
:name: nz_knnbpz

The estimated n(z) using ECDFS kNN (top panel) and BPZ (bottom panel) photo-$z$ estimates and applying the "all colors" red sequence cut. In both panels, the lighter color is used to display the distribution before applying the color cut, and darker after. Note that we have applied several additional weak lensing cuts as apposed to the simple photometry quality flags in {numref}`knn_bpz`.
```

```{figure} _static/ECDFS_nz_fzbtpz.png
:name: nz_fzbtpz

The estimated n(z) using ECDFS FZB (top panel) and TPZ (bottom panel) photo-$z$ estimates and applying the "all colors" color cut. In both panels, the lighter color is used to display the distribution before applying the color cut, and darker after. Note that we have applied several additional weak lensing cuts as apposed to the simple photometry quality flags in {numref}`fzb_tpz`.
```



```{note}

Add shear ratio plots for pz cuts using 2 bins if any of them are good.
```




## References 

```{bibliography}
```
