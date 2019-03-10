---
layout: default
title: IJP Color Calibrator
nav_order: 2
parent: IJP-Color
permalink: /docs/ijp-color/color-calibrator
---

__IJP Color Calibrator__ plugin color calibrates images using a color chart. Supported charts:
* GretagMacbeth ColorChecker
* [X-Rite ColorChecker Passport](https://xritephoto.com/colorchecker-passport-photo)
* [Image Science Associates](https://www.imagescienceassociates.com/) ColorGauge

Supports 8, 16, 32 bit per channel color images, including raw.

![Image Calibrator]({{site.url}}/assets/images/ijp-color/Color_Calibrator_0.6_01.png)

## Features
* Interactive placement of color chart (select 4 corners using polygon selection)
* Custom chip margin
* Render colors of the reference chart and display reference values
* Assume that input image is in linear XYZ (e.g raw image) or non-linear (sRGB) color space
* 6 mapping options (linear, linear cross-band, quadratic, quadratic cross-band, cubic, cubic-cross band)
* Automatic computation of the best reference and mapping method ("Suggest Options")
* Displays detailed information about the correction, including errors for each chip, and correction fit scatter plots ("Show extra info")
* Computed calibration can be applied to other images, assuming that images were acquired under the same lighting and cameras settings
