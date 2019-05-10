[![PyPI version](https://badge.fury.io/py/matrixprofile-ts.svg)](https://badge.fury.io/py/matrixprofile-ts)
[![Build Status](https://travis-ci.org/target/matrixprofile-ts.svg)](https://travis-ci.org/target/matrixprofile-ts)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
# matrixprofile-ts

matrixprofile-ts is a Python 2 and 3 library for evaluating time series data using the Matrix Profile algorithms developed by the [Keogh](https://www.cs.ucr.edu/~eamonn/MatrixProfile.html) and [Mueen](https://www.cs.unm.edu/~mueen/) research groups at UC-Riverside and the University of New Mexico. Current implementations include MASS, STMP, STAMP, STAMPI, STOMP and SCRIMP++.

Read the Target blog post [here](https://tech.target.com/2018/12/11/matrix-profile.html).

Further academic description can be found [here](https://www.cs.ucr.edu/~eamonn/MatrixProfile.html).

## Contents
- [Installation](#installation)
- [Quick start](#quick-start)
- [Detailed Example](#detailed-example)
- [Algorithm Comparison](#algorithm-comparison)
- [Matrix Profile in Other Languages](#matrix-profile-in-other-languages)
- [Contact](#contact)
- [Citations](#citations)

## Installation

Major releases of matrixprofile-ts are available on the Python Package Index:

`pip install matrixprofile-ts`

Details about each release can be found [here](https://github.com/target/matrixprofile-ts/blob/master/docs/Releases.md).

## Quick start

```
>>> from matrixprofile import *
>>> import numpy as np
>>> a = np.array([0.0,1.0,1.0,0.0,0.0,1.0,1.0,0.0,0.0,1.0,1.0,0.0])
>>> matrixProfile.stomp(a,4)
(array([0., 0., 0., 0., 0., 0., 0., 0., 0.]), array([4., 5., 6., 7., 0., 1., 2., 3., 0.]))
```
Note that SCRIMP++ is highly recommended for calculating the Matrix Profile due to its speed and anytime ability.

## Examples

Jupyter notebooks containing various examples of how to use matrixprofile-ts can be found under `docs/examples`.

As a basic introduction, we can take a synthetic signal and use STOMP to calculate the corresponding Matrix Profile (this is the same synthetic signal as in the [Golang Matrix Profile library](https://github.com/aouyang1/go-matrixprofile)). Code for this example can be found [here](https://github.com/target/matrixprofile-ts/blob/master/docs/examples/Matrix_Profile_Tutorial.ipynb)

![datamp](https://github.com/target/matrixprofile-ts/blob/master/datamp.png)


There are several items of note:

- The Matrix Profile value jumps at each phase change. High Matrix Profile values are associated with "discords": time series behavior that hasn't been observed before.

- Repeated patterns in the data (or "motifs") lead to low Matrix Profile values.


We can introduce an anomaly to the end of the time series and use STAMPI to detect it

![datampanom](https://github.com/target/matrixprofile-ts/blob/master/datampanom.png)

The Matrix Profile has spiked in value, highlighting the (potential) presence of a new behavior. Note that Matrix Profile anomaly detection capabilities will depend on the nature of the data, as well as the selected subquery length parameter. Like all good algorithms, it's important to try out different parameter values.

## Algorithm Comparison

This section shows the matrix profile algorithms and the time it takes to compute them. It also discusses use cases on when to use one versus another. The timing comparison is based on the synthetic sample data set to show run time speed.

All time comparisons were ran on an 8 core 4 ghz processor with 16 GB of memory.

| Algorithm | Time to Complete                                                       | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|-----------|------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| STAMP     | 391 ms ± 4.36 ms per loop (mean ± std. dev. of 7 runs, 1 loop each)    | STAMP is an anytime algorithm that lets you sample the data set to get an approximate solution. Our implementation provides you with the option to specify the sampling size in percent format.                                                                                                                                                                                                                                                                                                                                              |
| STOMP     | 172 ms ± 1.74 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)  | STOMP computes an exact solution in a very efficient manner. When you have a historic time series that you would like to examine, STOMP is typically the quickest at giving an exact solution.                                                                                                                                                                                                                                                                                                                                               |
| SCRIMP++  | 83.5 ms ± 1.64 ms per loop (mean ± std. dev. of 7 runs, 10 loops each) | SCRIMP++ merges the concepts of STAMP and SCRIMP++ together to provide an anytime algorithm that enables "interactive analysis speed". Essentially, it provides an exact or approximate solution in a very timely manner. Our implementation allows you to specify the max number of seconds you are willing to wait for a solution to obtain an approximate solution. If you are wanting the exact solution, it is able to provide that as well. The original authors of this algorithm suggest that SCRIMP++ can be used in all use cases. |

## Matrix Profile in Other Languages
- R: https://github.com/franzbischoff/tsmp
- Golang: https://github.com/aouyang1/go-matrixprofile

## Contact
- Andrew Van Benschoten (avbs89@gmail.com)
- Tyler Marrs (tylerwmarrs@gmail.com)

## Citations
1. Chin-Chia Michael Yeh, Yan Zhu, Liudmila Ulanova, Nurjahan Begum, Yifei Ding, Hoang Anh Dau, Diego Furtado Silva, Abdullah Mueen, Eamonn Keogh (2016). Matrix Profile I: All Pairs Similarity Joins for Time Series: A Unifying View that Includes Motifs, Discords and Shapelets. IEEE ICDM 2016

2. Matrix Profile II: Exploiting a Novel Algorithm and GPUs to break the one Hundred Million Barrier for Time Series Motifs and Joins.  Yan Zhu, Zachary Zimmerman, Nader Shakibay Senobari, Chin-Chia Michael Yeh, Gareth Funning, Abdullah Mueen, Philip Berisk and Eamonn Keogh (2016). EEE ICDM 2016

3. Matrix Profile V: A Generic Technique to Incorporate Domain Knowledge into Motif Discovery. Hoang Anh Dau and Eamonn Keogh. KDD'17, Halifax, Canada.

4. Matrix Proﬁle XI: SCRIMP++: Time Series Motif Discovery at Interactive Speed. Yan Zhu, Chin-Chia Michael Yeh, Zachary Zimmerman, Kaveh Kamgar and Eamonn Keogh, ICDM 2018.