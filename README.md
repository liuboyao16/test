# test

Scripts to analyse the data in the energy domain

All the measurements HeSE takes now is stored in a .mat file. First of all, we give an example file (examples/dy018667.mat). In order to analyse it in the energy domain, or to extract the phonon spectrum, one can do the following:

use the cammand

postprocess_dyfiles.postprocess_dyfile('NumVec',[18713],'initStr', 'example/dy0','fixFalsePositive',1,'reProcessing',1);

res = fit_dyFiles.prepare_measured_set_for_fitting('NumVec',[18713],'initStr', 'example/dy0');

res = fit_dyFiles.fit_dyFiles_freq_domain(res,@Vibrations_GRu.get_vibrations,3,1, [],[0 10 0 0.01],[3 7]);
  
![some test figures](https://github.com/liuboyao16/test/blob/main/examples/dy018713.jpg)
