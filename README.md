# ASSA

Atom Surface Scattering Analysis

## Scripts to analyse the data in the energy domain

All the measurements HeSE takes now is stored in a .mat file, so It will be most convenient to use MATLAB to analyse them. First of all, we give an example file (examples/dy018667.mat). In order to analyse it in the energy domain, or to extract the phonon spectrum, one can do the following:

In the ASSA folder, use the following 3 cammands:

```matlab
postprocess_dyfiles.postprocess_dyfile('initStr','example/dy0','NumVec',[18713],'fixFalsePositive',1);
res = fit_dyFiles.prepare_measured_set_for_fitting('initStr','example/dy0','NumVec',[18713]);
res = fit_dyFiles.fit_dyFiles_freq_domain(res,@Vibrations_GRu.get_vibrations,3,1, [],[0 10 0 0.01],[3 7]);
```
The phonon spectrum should be generated as shown in the figure below.

![some test figures](https://github.com/liuboyao16/test/blob/main/examples/dy018713.jpg)

### An explanation of the commands
