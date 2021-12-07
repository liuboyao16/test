# ASSA

Atom Surface Scattering Analysis

## Analyse spin echo data in the energy domain

### Generate the spectrum

All the measurements HeSE takes now is stored in a .mat file, so It will be most convenient to use MATLAB to analyse them. First of all, we give an example file (examples/dy018667.mat). In order to analyse it in the energy domain, or to extract the phonon spectrum, one can do the following:

In the ASSA folder, use the following 2 cammands:
```matlab
postprocess_dyfiles.postprocess_dyfile('initStr','examples/dy0','NumVec',[18713],'reProcessing',1,'fixFalsePositive',1);
res = fit_dyFiles.prepare_measured_set_for_fitting('initStr','examples/dy0','NumVec',[18713]);
```
To look at the phonon spectrum, we can do

```matlab
plot(res(1).Energ_meV,res(1).SKw)
axis([-5 15 0 1e9]); xlabel('\DeltaE/meV'); ylabel('Intensity/arbitrary units');
```

The spectrum should be generated as shown below.

<img src="https://github.com/liuboyao16/test/blob/main/examples/dy018713.jpg" width="600">

#### An explanation of the commands

* `postprocess_dyfiles.postprocess_dyfiles` is a function used to process the files obtained. The original .mat file has only one structure called `meas`, which contains all the raw data. This process creates an additional structure called `processed_meas`.
  * In the example, the file's name is `'examples/dy018713'`, which are split into `'examples/dy0'` and `[18713]`. `'examples/dy0'` is the `'initStr'` variable, and `[18713]` is the `'NumVec'` variable. You can put a vector containing multiple numbers, to process multiple files in a certain folder.
  * If `'reProcessing'` is set to `1`, the script will postprocess the files again even if it has been processed. If `'reProcessing'` is `0`, the function will not do anything to it.
  * Sometimes the system can have a glitch and the signal will have an outlier. In that case we will set `'fixFalsePositive'` to `0` in order to fix that. In the current example it is not needed, so `'fixFalsePositive'` is `1`.
* `fit_dyFiles.prepare_measured_set_for_fitting` further process the data in `processed_meas`, and saves the data into a structrue called `res`. The phonon spectrum as a function of energy transfer is stored in `res(1).Energ_meV` and `res(1).SKw`.

### Additional features of `postprocess_dyfiles.postprocess_dyfiles`

The raw data taken in a typical spin echo measurement can be extracted and plotted using
```matlab
figure; hold on
load('examples/dy018713.mat')
plot(meas.ibase,meas.mean.Preal);
plot(meas.ibase,meas.mean.Pimag);
xlabel('base current/A'); ylabel('Spin polarisation'); legend('real','imaginary');
```
The result should look like:

<img src="https://github.com/liuboyao16/test/blob/main/examples/dy018713_pol.jpg" width="600">

This file actually shows the simplest case. There is no outlier in the data. If you look at the base current you will find that it is equally spaced. However, that may not always be the case. There can be various features in the data that need dealing with, which are listed below.

### Outliers in the data.
An example of such a case can be found in examples/dy018713.mat. Use the following commands again.

```matlab
figure; hold on
load('examples/dy018863.mat')
plot(meas.ibase,meas.mean.Preal);
plot(meas.ibase,meas.mean.Pimag);
xlabel('base current/A'); ylabel('Spin polarisation'); legend('real','imaginary');
```

we can find that the signal does have an outlier at around 0.83 A.

<img src="https://github.com/liuboyao16/test/blob/main/examples/dy018863_pol.jpg" width="600">

The function `postprocess_dyfiles.postprocess_dyfiles` automatically fix that problems. There are 8 loops taken in total, by comparing In order to fix that, we can change `'fixFalsePositive'` from `1` to `0`, so the postprocessing will become

```matlab
postprocess_dyfiles.postprocess_dyfile('initStr','examples/dy0','NumVec',[18725],'reProcessing',1,'fixFalsePositive',0);
```

