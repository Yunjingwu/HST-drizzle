# AstroDrizzle



## Load package

```
import drizzlepac
from drizzlepac import tweakreg
from drizzlepac import astrodrizzle
```



## set ref_files

````
F606w:
set jref='/Users/wuyunjing/Desktop/ALMA/new_work/ref_file/F606W_acs/hst_acs_0410.imap'
````

```
F814W:
set jref='/Users/wuyunjing/Desktop/ALMA/new_work/ref_file/F814W_acs/hst_acs_0410.imap'
```

```
F105W:
set iref='/Users/wuyunjing/Desktop/ALMA/new_work/ref_file/F105W_wfc3/hst_wfc3_0423.imap'
```

```
F125W:
set iref='/Users/wuyunjing/Desktop/ALMA/new_work/ref_file/F125W_wfc3/hst_wfc3_0423.imap'
```

```
F160W:
set iref='/Users/wuyunjing/Desktop/ALMA/new_work/ref_file/F160W_wfc3/hst_wfc3_0423.imap'
```

-------



## Update wcs

####Update the WCS keywords using the distortion reference files in the image header, eg. whenever using a new version of DrizzlePac

```
from stwcs import updatewcs
updatewcs.updatewcs('*flt.fits')
```

___



## TweakReg

####TweakReg is a tool to improve the image alignment using matched sources between images.

```
tweakreg.TweakReg('*flt.fits',updatehdr=True,threshold=200,shiftfile=True,outshifts='shift.txt')
```

___



##AstroDrizzle

####AstroDrizzle removes geometric distortion, corrects for sky background variations, flags cosmic-rays, and combines images with optional subsampling.  

```
Parameters description:
1.	build:
			In order to do stacking, This parameter must be setted to True.

2.	driz_cr:
			Detected cosmic-ray
			
3.	driz_cr_corr:
			Create a cosmic-ray cleaned input image

4.	final_scale:
			Select a 'final_scale' value that is ~2x the full width half max to allow for adequate sampling of the PSF
			
5.	final_pixfrac:
			Select a 'final_pixfrac' value that is slightly larger than the 'final_scale' to allow for spillover when drizzling
			
6.	final_rot:
			Remove rotational distortion. 
			Set to 0. 

7.	final_wcs：
			Obtain the WCS solution from a user-designated reference image
			
8.	final_refimage:
			Align
			
9.	driz_sep_bits, final_bits:
			Recommended values: WFC3/IR= '64,512', ACS/WFC & WFC3/UVIS= '32, 64', WFPC2= '8, 1024'. These tell the software which flags to instead treat as good. (ref: QuickStartGuide)
```

____



## Code

### PSF:

​		The scale is chosen to sample the PSF FWHM by about ~2.0 to
2.5 pixels. 

$$PSF = 1.22\frac{\lambda}{d}\times\frac{180}{\pi}\times3600 (arcsec), d=2.4 (m)$$

$$final\_scale=\frac{PSF}{2}$$

| Filter_name | Filter_Cent_wave(nm) | PSF(") | final_scale(") |
| ----------- | -------------------- | ------ | -------------- |
| F606W       | 595.6                | 0.0624 | 0.0312         |
| F814W       | 835.3                | 0.0876 | 0.0438         |
| F105W       | 1050                 | 0.110  | 0.0550         |
| F125W       | 1250                 | 0.131  | 0.0655         |
| F160W       | 1545                 | 0.162  | 0.0810         |



____

### F606W:

#### Dither information:

Ref: https://spacetelescope.github.io/notebooks/notebooks/DrizzlePac/optimize_image_sampling/optimize_image_sampling.html

![avatar](/Users/wuyunjing/Desktop/ALMA/new_work/Dither/F606W_dither.png)

####no_opt:

```
astrodrizzle.AstroDrizzle('*flc.fits', output='f606w_noopt', runfile='astrodrizzle_noopt.log', build=True, clean=True, driz_sep_bits='32, 64', final_bits='32, 64', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```

#### opt:

```
astrodrizzle.AstroDrizzle('*flc.fits', output='f606w_opt', runfile='astrodrizzle_opt.log', build=True, clean=True, driz_sep_bits='32, 64', final_bits='32, 64', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True,  final_scale=0.03, final_pixfrac=0.6, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```

___



### F814W:

#### Dither information:

![avatar](/Users/wuyunjing/Desktop/ALMA/new_work/Dither/F814W_dither.png)

#### no_opt:

```
astrodrizzle.AstroDrizzle('*flc.fits', output='f814w_noopt', runfile='astrodrizzle_noopt.log', build=True, clean=True, driz_sep_bits='32, 64', final_bits='32, 64', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```

#### opt:

```
astrodrizzle.AstroDrizzle('*flc.fits', output='f814w_opt', runfile='astrodrizzle_opt.log', build=True, clean=True, driz_sep_bits='32, 64', final_bits='32, 64', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True, final_scale=0.04, final_pixfrac=0.6, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```

------------------



### F105W:

#### Dither information:

![avatar](../ALMA/new_work/Dither/F105W_dither.png)

#### no_opt:

```
astrodrizzle.AstroDrizzle('*flt.fits', output='f105w_noopt', runfile='astrodrizzle_noopt.log', build=True, clean=True, driz_sep_bits='64, 512', final_bits='64, 512', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```

#### opt:

```
astrodrizzle.AstroDrizzle('*flt.fits', output='f105w_opt', runfile='astrodrizzle_opt.log', build=True, clean=True, driz_sep_bits='64, 512', final_bits='64, 512', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True, final_scale=0.055, final_pixfrac=0.6, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```

-------



### F125W:

#### Dither information:

![avatar](../ALMA/new_work/Dither/F125W_dither.png)

#### no_opt:

```
astrodrizzle.AstroDrizzle('*flt.fits', output='f125w_noopt', runfile='astrodrizzle_noopt.log', build=True, clean=True, driz_sep_bits='64, 512', final_bits='64, 512', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```

#### opt:

```
astrodrizzle.AstroDrizzle('*flt.fits', output='f125w_opt', runfile='astrodrizzle_opt.log', build=True, clean=True, driz_sep_bits='64, 512', final_bits='64, 512', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True, final_scale=0.07, final_pixfrac=0.6, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```

------



### F160W:

#### Dither information:

![avatar](../ALMA/new_work/Dither/F160W_dither.png)

#### no_opt:

```
astrodrizzle.AstroDrizzle('*flt.fits', output='f160w_noopt', runfile='astrodrizzle_noopt.log', build=True, clean=True, driz_sep_bits='64, 512', final_bits='64, 512', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```

#### opt:

```
astrodrizzle.AstroDrizzle('*flt.fits', output='f160w_opt', runfile='astrodrizzle_opt.log', build=True, clean=True, driz_sep_bits='64, 512', final_bits='64, 512', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True, final_scale=0.08, final_pixfrac=0.6, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```

------



## Same final_scale parameter

### F606W

```
astrodrizzle.AstroDrizzle('*flc.fits', output='f606w_opt_0.03', runfile='astrodrizzle_opt.log', build=True, clean=True, driz_sep_bits='32, 64', final_bits='32, 64', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True,  final_scale=0.03, final_pixfrac=0.6, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```



### F814W

```
astrodrizzle.AstroDrizzle('*flc.fits', output='f814w_opt_0.03', runfile='astrodrizzle_opt.log', build=True, clean=True, driz_sep_bits='32, 64', final_bits='32, 64', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True, final_scale=0.03, final_pixfrac=0.6, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```



### F105W

```
astrodrizzle.AstroDrizzle('*flt.fits', output='f105w_opt_0.03', runfile='astrodrizzle_opt.log', build=True, clean=True, driz_sep_bits='64, 512', final_bits='64, 512', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True, final_scale=0.03, final_pixfrac=0.6, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```



### F125W

```
astrodrizzle.AstroDrizzle('*flt.fits', output='f125w_opt_0.03', runfile='astrodrizzle_opt.log', build=True, clean=True, driz_sep_bits='64, 512', final_bits='64, 512', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True, final_scale=0.03, final_pixfrac=0.6, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```



### F160W

```
astrodrizzle.AstroDrizzle('*flt.fits', output='f160w_opt_0.03', runfile='astrodrizzle_opt.log', build=True, clean=True, driz_sep_bits='64, 512', final_bits='64, 512', final_wcs=True, final_rot=0., driz_cr=True, driz_cr_corr=True, final_scale=0.03, final_pixfrac=0.6, final_refimage='/Users/wuyunjing/Desktop/ALMA/new_work/test/idjb11020_drz.fits')
```



