For the original description, see the [original repo][org-repo].

This repo contains the mex files needed for running Ma's code on a Linux system, as well as the changes made to the files that produced those mex files. Tested on Ubuntu 20.04 with g++/gcc 9.4.0, MATLAB R2022a.

Steps performed to accomplish this (generally, these follow the instructions from the "Troubleshooting" section of the README of [this repo][troubleshooting]; below, the *sr-metric* dir in paths can be replaced with *sr-metric-fixed-for-linux*, if you carry out the below instructions using this repo):
1. Run *sr-metric/external/matlabPyrTools/MEX/compilePyrTools.m*.
2. After execution, *compilePyrTools.m* generated new versions of 4 mex files: *upConv.mexa64*, *pointOp.mexa64*, *range2.mexa64*, *histo.mexa64*. Copy these to the parent dir (i.e., to *sr-metric/external/matlabPyrTools/*). These files already exist in that parent dir, so you'll need to overwrite those.
3. Change line 82 in the file *sr-metric/external/randomforest-matlab/RF_Reg_C/src/mex_regressionRF_predict.cpp* to `plhs[0]=mxCreateNumericMatrix(n_size,1,mxDOUBLE_CLASS,mxREAL);`.
4. Change the *sr-metric/external/randomforest-matlab/RF_Reg_C/compile_linux.m* so that it looks like [so][compile-linux]. Note that: 
	  * The `-o` switch won't work, so use `-output` instead.
	  * You must give the correct paths to the C++ files, i.e., precede them with `src/`, provided you will be running *compile_linux.m* from its directory, that is, from *sr-metric/external/randomforest-matlab/RF_Reg_C/*.
	  * You might need to supply the full path to mex for your MATLAB installation if `make mex` won't work.
6. Run *compile_linux.m* from its directory. This will generate the file *mexRF_predict.mexa64*, which is the file required to resolve the `undefined function or variable mexRF_predict` error. 
7. That is it, the *mexRF_predict.mexa64* file should stay in *sr-metric/external/randomforest-matlab/RF_Reg_C* and doesn't need to be copied anywhere. You can now execute the code for Ma's metric, e.g., run *demo.m*.

[org-repo]: https://github.com/chaoma99/sr-metric
[troubleshooting]: https://github.com/roimehrez/PIRM2018
[compile-linux]: https://github.com/Timmate/sr-metric-fixed-for-linux/blob/master/external/randomforest-matlab/RF_Reg_C/compile_linux.m
