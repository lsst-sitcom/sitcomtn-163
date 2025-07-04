# Source Selection for Abell 360 in LSSTComCam Data Preview 1

```{abstract}
We cover the source selection done for the Abell 360 LSSTComCam cluster study focusing on color and photo-z cuts.
Identification of cluster members via the red sequence and photo-z offers a science focused validation of photometry with LSSTComCam.
We are able to cross-match with DESI spectroscopic redshifts to offer an independent validation suite on photo-z estimates.
We make various color and photo-z cuts to generate N(z)s and shear profiles.
We find that both cuts are quite robust to various choices made and can produce a shear profile for Abell 360. Both methods should be applicable to generating a consistent mass estimate for the cluster.
```


## Introduction 
The Rubin LSSTComCam {cite:p}`ComCam` observing campaign undertaken at the end-of-year 2024 covered seven fields, among which include the low ecliptic latitude Rubin SV 38 7 field containing the Abell 360 (A360) galaxy cluster.
A360 is an intermediate mass cluster ({math}`M_{500,c} = 4.3\times 10^14 M_\odot`{cite:p}`A360m` at z=0.22 {cite:p}`A360z` that we use as a commissioning demonstrator of Rubin capabilities for cluster weak lensing (WL) studies. 
In order to best measure the shear profile of the cluster, we would ideally only measure the shapes of galaxies that are lensed by the cluster, i.e. source galaxies that are behind the cluster.
This can be done by identifying and removing cluster members via the red sequence, an overdensity on a color-magnitude diagram caused by the cluster members, or by removing objects near and below the redshift of the cluster.
<!--This TechNote goes over both methods to define the source selection of galaxies used to measure the shear profile.-->
We will discuss both methods, associated verification, the estimated n(z) from each method, and the shear profile from the various sources.

This technote is one part of a series studying A360 in order to both stress test the commissioning camera and demonstrate the technical capabilities of the Vera Rubin Observatory.
We study the quality of the PSF modeling and impact it can have on cluster WL in {cite}`SITCOMTN-161`, implementation of cell-based coadds and subsequent use for Metadetect{cite:p}`Sheldon_2023` in {cite}`SITCOMTN-162`, photometric calibration in _insert here_, use of Anacal {cite:p}`Li_2023` to produce a shear profile in {cite:p}`SITCOMTN-164`, and background subtraction in this field and Fornax in _insert here_.
 



## Dataset
The Rubin SV 38 7 field has been observed in *g* (44 visits), *r* (55 visits), *i* (57 visits) and *z* (27 visits) {cite:p}`RTN-011` with no visits in u and y-band.
The Data Preview 1 (DP1){cite:p}`RTN-095good` catalog includes various flux measurements including cModel {cite:p}`SDSS2004` and GaaP {cite:p}`Kuijken_2008` along with HSM shape measurements {cite:p}`HSM1, HSM2`.
The HSM resolution factor, {cite:p}`HSM-y1`, which compares the resolution of a galaxy to the PSF in order to remove both stars and poor quality objects, is defined as $R = 1-\frac{T_\textrm{PSF}}{T_\textrm{I}}$ which can be calculated from the catalog.
Photometric redshift estimates are not included in the DP1 catalog but have been created as outlined in {cite:p}`SITCOMTN-154`.
<!--Similarly, details on PSF and photometric calibration are located in {cite:p}`SITCOMTN-161` and photometric calibration technote . We will focus on the various cuts made to generate the cluster lensing sample for Abell 360. -->


We apply basic quality flags to remove objects that have caused a failure in the flux measurements, namely from saturation, cosmic rays, and spurious detections.
Each quality flag is a boolean which is set to True if the algorithm fails to run on that object for whatever reason.
The flags and number of objects affected by the flag are listed in {numref}`quality_table`.
```{table} Quality flags and number of objects affected by flag.
:widths: auto
:align: center
:name: quality_table

| Flag Name | Number of Objects|
| --------- | ---------------- |
| refExtendedness | 24892 |
| i_iPSF_flag | 62462 |
| i_hsmShapeRegauss_flag | 91836 |
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
Star-galaxy separation is done using the `refExtendedness` flag which will flag objects if $\frac{\textrm{flux}_\textrm{psf}}{\textrm{flux}_\textrm{cModel}} < .985$ as galaxies giving 52309 galaxies (89%) and 6367 stars (11%).
The counts per bin of galaxies only is shown in {numref}`galaxy-dist`  with *griz* bands reaching an estimated depth of 24.9, 24.5, 24.3, and 23.6 mags respectively.
The depths are estimated by eye using the galaxy distribution below.
These limits are not derived from synthetic source injection and should be treated as rough guides to the true limits.


```{figure} _static/galaxy_dist.png
:name: galaxy-dist

Distribution of galaxy magnitudes with (approximate) limiting magnitude shown as histogram with *griz* corresponding to blue, orange, green, and red and the magnitude limit per band shown as solid vertical line in the same color.
```


## Color-cuts

We can select cluster members by identifying the red sequence galaxies in a color-magnitude diagram.
Due to the lack of observations in *z* we focus on the _gri_ bands and the colors (*g-r*, *r-i*, *g-i*) from this set.
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
Photo-*z*s were estimated using FlexZBoost (FZBoost) {cite:p}`Flexzboost1, Flexzboost2`, TreePhotoZ (TPZ) {cite:p}`TPZ`, k-Nearest Neighbors (kNN) {cite:p}`RAIL`, and Bayesian Photo-zs (BPZ) {cite:p}`BPZ` as implemented in RAIL {cite:p}`RAIL` using the 4 available bands *griz* on cModel fluxes.
These estimators were chosen as kNN and BPZ will be provided by the Rubin project for future data releases while we expect FZB and TPZ to be readily available from the community.
The estimators were trained on the same 4 bands in the ECDFS field and then applied to the cluster field.
We apply the same cuts as in {numref}`quality_table` and also require that width of the photo-$z$ estimate, $\sigma_z$, be small ($\sigma_z < 0.25$) for each object.
Along with removing spurious and extremely faint objects, we remove objects that are likely causing large issues for photo-z estimators due to the missing *u* and *y* band.
The redshift distribution, {math}`N(z)`, after applying these quality cuts is shown in {numref}`quality_cuts_nz` along with the location of our subsequent photo-*z* cut to remove cluster members which we place at $z = 0.37 = 0.22 + .15$ to account for wide error bars in photo-*z* estimates.
This cut is placed on the central statistic of the technique which is mean for all techniques except kNN for which we use median due to implementation issues.

```{figure} _static/quality_cuts_nz.png
:name: quality_cuts_nz

Photometric redshift distribution for the galaxy sample (within 0.5 deg, or 6.4 Mpc, of the central BCG and with 20 < _i_ < 24) for 4 photometric redshift estimators. BPZ is shown in blue, TPZ in orange, FZBoost in green, and kNN in red. A faint dashed line is included where the cluster cut will be placed ($z = 0.37$).
```

In this field, we have overlap with the DESI {cite:p}`DESI-dr1` BGS {cite:p}`BGS`, ELG {cite:p}`ELG`, and LRG {cite:p}`LRG` catalogs, shown in {numref}`desi_location`, which allows us to perform simple external validation on the four redshift techniques.

```{figure} _static/desi_location.png
:name: desi_location

Overlap between SV 38-7 field and various DESI subsamples. Note that the BGS sample that covers the low-z regime does not uniformly cover the region and thus we are incomplete in this space. From {cite}`SITCOMTN-154`.
```
To validate the techniques, we compare the photo-*z* for objects with matches to the DESI sample which we show in {numref}`desi_validation`  without placing a cut on $\sigma_z$ and in {numref}`cut_desi_validation` to highlight the improvement in photo-*z* estimate when using this cut. 
Even without placing the cut, most techniques perform quite well up to $z < 0.8$ but then suffer onwards.
It is likely that this is caused by a mismatch between the training sample and validation sample (ELGs) which can cause systematic issues.
Once a cut on $\sigma_z$ is placed, we have much higher quality photo-*z* estimates at the cost of roughly half the remaining sample.


```{figure} _static/scatter_compare_nocut.png
:name: desi_validation 

Photometric redshift estimates versus matched spectroscopic redshifts from DESI for the four methods studied in this technote with minimal quality cuts applied ({numref}`quality_table` only), namely no $\sigma_z$ cut applied on the photometric redshift estimates. In the upper left in blue we display BPZ, upper right in orange TPZ, lower left in green FZBoost, and lower right in red kNN. The issues at high redshift ($z >0.8$) may be caused by mismatch between the training and validation sets. 
```


```{figure} _static/scatter_compare_cut.png
:name: cut_desi_validation 

Photometric redshift estimates versus matched spectroscopic redshifts from DESI for the four methods studied in this technote with quality cuts applied ({numref}`quality_table`) along with requiring  $\sigma_z < 0.25$. In the upper left in blue we display BPZ, upper right in orange TPZ, lower left in green FZBoost, and lower right in red kNN. Note the improvement from {numref}`desi_validation`.
```


For more details on the matching and performance of other photo-z estimators on this field, and DP1 in general, please refer to {cite}`SITCOMTN-154`.

### Red Sequence Redshift

Checking the redshift of the red sequence members acts as a self-consitency check which for photometry and photo-z estimates.
Applying the same red sequence cuts seen in {numref}`CMD_rs` on the DESI matched sample we can directly read off spectroscopic redshifts which we show in {numref}`matched_rs`.
We can further validate the photo-*z* estimators by comparing the DESI N(z) for the red sequence to the photometric N(z) which we display in {numref}`matched_rs_pdf`.


```{figure} _static/matched_redseq_z.png
:name: matched_rs

Histogram of spectroscopic redshifts from DESI matched red sequence galaxies in blue. The cluster location is shown as a faint dashed gray line at $z=0.22$.
```

```{figure} _static/matched_redseq_z-pzpdf.png
:name: matched_rs_pdf

Histogram of spectroscopic redshifts from DESI matched red sequence galaxies in purple along with the N(z) from the photo-*z* estimatators. The cluster location is shown as a faint dashed gray line at $z=0.22$. 
```
 
The peak of the DESI distribution gives us confidence that we are removing cluster members but the tail is slightly concerning. 
The color-cuts could be optimized to reduce the size of the tail, but due to the limited coverage in the area from the DESI low-*z* catalogs and varying selection functions between the two instruments, we only use it as validation.


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
| 20 $\leq$ i_cModel_mag $\leq$ 24 |
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
Similarly, we can apply our photo-$z > 0.37$ cut on BPZ, TPZ, FZBoost, and kNN to produce a source sample that we calculate the shear profile of in {numref}`shear_profile_pz`. 
While there is a larger spread on the value of the $\gamma_+$ between the cuts, the signal is quite similar to the color-cut profile and similarly passes the null test of $\gamma_\times = 0$ aside from the fourth bin which is high in the color-cut profile as well.


```{figure} _static/shear_profile_cc.png
:name: shear_profile_cc

The shear profile with varying color cuts. The tangential shear component is shown with circular points and cross shear component with x points. The error bar represents a 95% confidence interval from bootstrapped samples. “All colors” refers to using all three red sequences and is shown in dark blue, "g-i" uses points found only on the $g-i$ red sequence and is shown in light blue, "r-i" for the $r-i$ redsequence and shown in light red, and "g-r" for the $g-r$ redsequence and shown in dark red. The points are staggered on the x-axis for clarity. 
```

```{figure} _static/shear_profile_pz.png
:name: shear_profile_pz

The shear profile with varying photo-*z* estimators all cut at $z=0.37$. BPZ is shown in blue, TPZ in orange, FZBoost in green, and kNN in red. The cross shear term is shown in the same colors but much fainter and with a "x" at the point instaed of a dot. The data is staggered on the x-axis for clarity. 
```


### N(z)
To estimate the $N(z)$ when using color cuts, we use the ECDFS photo-*z*s and apply the same red sequence and weak lensing cuts.
The ECDFS field was observed in 6 bands which enables higher quality photo-*z* estimates and by using the same instrument, we have a very similar selection function between the two fields.
To offset the difference in depth between the two fields we restrict to $i < 23.5$ and SNR$_i \geq 10$.
The increased brightness limit was chosen to keep the magnitude limit as the dominant cut in the sample (versus the SNR cut) which is not the case at $i = 24$ for the cluster field.
The $N(z)$ with the color cut transferred is presented in {numref}`ecdfs_nz` and the $N(z)$ directly in the field in {numref}`cc_nz`.


```{figure} _static/ECDFS_nz.png
:name: ecdfs_nz

$N(z)$ from 6 band photo-*z* estimates when applying quality cuts, weak lensing cuts, and color cuts all transferred to the ECDFS sample. BPZ is shown in blue, TPZ in orange, FZboost in green, and kNN in red. A dashed red line is added to show the location of the cluster.
```


```{figure} _static/cc_cuts_4bandsnz.png
:name: cc_nz

$N(z)$ from 4 band photo-*z* estimates when applying quality cuts, weak lensing cuts, and color cuts. BPZ is shown in blue, TPZ in orange, FZboost in green, and kNN in red. A dashed red line is added to show the location of the cluster.
```

On the photo-*z* cut, we remove objects that have their central statistic (mean) below $z=0.37$ but the $N(z)$ will still have support in that range from objects near the cut with wide distributions.
The distribution is plotted in {numref}`wl_cuts_nz`.


```{figure} _static/wl_cuts_nz.png
:name: wl_cuts_nz

$N(z)$ from the 4 photo-*z* techniques after applying quality cuts, a $\sigma_z$ cut, weak lensing cuts, and $z \leq 0.37$ cut. BPZ is shown in blue, TPZ in orange, FZBoost in green, and kNN in red. A dashed red line is added to show the location of the cluster and a dashed gray line is added to show the location of the photo-*z* cut.
```


All 3 $N(z)$ distributions peak at $z\approx 0.6$ and approach zero by $z\approx 1.3$.
Both of the 4-band distributions,  {numref}`cc_nz` and {numref}`wl_cuts_nz`, show a peak for kNN near 0.4 which is concerning. 
The photo-z $N(z)$ is very good at removing low redshift objects as seen by the flat distribution between the red and gray lines in {numref}`cc_nz`. 


## Conclusion

In this technote we have outlined the source selection for studying A360 in order to enable cluster weak lensing and verify the science pipelines.
We find that the high quality photometry makes the red sequence easily identifiable and the source selection foairly consistent between the multiple colors.
When using photometric redshifts, we are able to recreate a similar signal and validate against the DESI spectroscopic redshifts which acts as a completely external test suite for photometric redshifts which were not trained on DESI spec-*z*s.
In total, the two methods give confidence that the combination of object detection and deblending, galaxy photometry, and photometric redshift estimates from the Vera Rubin observatory will enable high quality cluster science in the years to come.

## References 

```{bibliography}
```
