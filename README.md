# ASSA

Atom Surface Scattering Analysis

## Analyse spin echo data in the energy domain

All the measurements HeSE takes now is stored in a .mat file, so It will be most convenient to use MATLAB to analyse them. First of all, we give an example file (examples/dy018667.mat). In order to analyse it in the energy domain, or to extract the phonon spectrum, one can do the following:

In the ASSA folder, use the following 3 cammands:

```matlab
postprocess_dyfiles.postprocess_dyfile('initStr','example/dy0','NumVec',[18713],'fixFalsePositive',1);
res = fit_dyFiles.prepare_measured_set_for_fitting('initStr','example/dy0','NumVec',[18713]);
```
Relevant information, such as the phonon spectrum, temperature, momentum transfer, etc., will be stored in the `res` structure. In this case, to look at the phonon spectrum, we can do

```matlab
plot(res(1).Energ_meV,res(1).SKw)
axis([-5 15 0 1e9]); xlabel('\DeltaE/meV'); ylabel('Intensity/arbitrary units');
```

The spectrum should be generated as shown below.

![some test figures](https://github.com/liuboyao16/test/blob/main/examples/dy018713.jpg)

### An explanation of the commands

* `postprocess_dyfiles.postprocess_dyfiles` is a function used to process the files obtained. The original .mat file has only one structure called `meas`, which contains all the raw data. This process creates an additional structure called `processed_meas`.
  * In the example, the file's name is `'example/dy018713'`, which are split into `'example/dy0'` and `[18713]`. `'example/dy0'` is the `'initStr'` variable, and `[18713]` is the `'NumVec'` variable. You can put a 
  * Sometimes the system can have a glitch and the signal will have an outlier. In that case we will set `'fixFalsePositive'` to `0` in order to fix that. In the current example it is not needed, so `'fixFalsePositive'` is `1`.
