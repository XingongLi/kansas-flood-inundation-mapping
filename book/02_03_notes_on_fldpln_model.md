# Some Notes on Using the FLDPLN Model

* When running xp_create_segment_library.m or model.py to create raw segment-based library for the Wildcat Creek, the sizes of the folder "rawlib" and "rawlib_py" varied between 36,209,164 and 36,209,168 bytes at different runs while the sizes of the other two folders ("segs" and "seg_py" and "seglib" and "seglib_py") are identical. This might be caused by the random row orders in some matrices which eventually leads to slightly different file sizes in compressed .mat files.
* For parallelization parameters of the FLDPLN model, for very large library, it's recommended to set the 'mtype' (model type) as 'HD' (hard-disk) as the RAM options (either 'ram0' or 'ram') may cause memory overflow. Otherwise, setting 'mtype' to 'ram' and 'worker_type' to 'Threads' is both more memory efficient and faster. 
* How to use MATLAB parallelization parameters to create segment-based library and the interactions between "mtype" (hd, ram0 and ram) and "para.type" (none, parfor, parfeval) parameters

## Parallelization in the FLDPLN Model

Parallelized segment library generation using "parfeval" can be done using either thread- or processe-based environment in MATLAB where thread-based environment is more memory efficient. However, when using the fldpln_py Python package, the following error occurred when using thread-based workers to create library.
```
Using Threads-based parfeval with 6 workers
Starting parallel pool (parpool) using the 'Threads' profile ...
Error using parpool (line 108)
Thread-based pools are not supported in this environment.

Error in rp_create_segment_library_v8 (line 187)

Error occurred during program execution\n:An error occurred when evaluating the result from a function. Details: 
  File C:\Program Files\MATLAB\R2024a\mcr\toolbox\parallel\cluster\cluster\parpool.m, line 108, in parpool

  File C:\Users\lixi\AppData\Local\Temp\lixi\mcrCache24.1\fldpln5\fldpln_py\rp_create_segment_library_v8.m, line 187, in rp_create_segment_library_v8
Thread-based pools are not supported in this environment.
```
**It seems that MATLAB Runtime doesn't support thread-based parallelization.** As such, the FLDPLN model's Python package, i.e., fldpln_py package, can only uses **process-based parallelization environment**.

When running xp_create_segment_library.m on my new laptop, the following error was encountered:
```
Using Processes-based parfeval with 6 workers
Starting parallel pool (parpool) using the 'Processes' profile ...
Error using parpool (line 133)
Too many workers requested. The cluster "Processes" has the NumWorkers property set to a maximum of 2 workers but 6 workers were requested. Either request a number of workers less than NumWorkers, or increase the value of the NumWorkers property for the cluster (up to a maximum of 512 for the Local cluster).

Error in rp_create_segment_library_v8 (line 187)
        p = parpool(para.worker_type, numworkers);

Error in xp_create_segment_library (line 63)
rp_create_segment_library_v8(bildir, segdir, filmskfile, segshpfile, fldmn, fldmx, dh, mxht, libdir, mtype, para);
```
It turned out that, while feature('numcores') returns the number of cores as 12, both parpool('Processes', 12) and parpool('Threads', 12) function calls complained there is only 2 workers/cores. See the MATLAB code snippet below:
```MATLAB
>> feature('numcores')
MATLAB detected: 12 physical cores.
MATLAB detected: 14 logical cores.
MATLAB was assigned: 14 logical cores by the OS.
MATLAB is using: 12 logical cores.
MATLAB is not using all logical cores because hyper-threading is enabled.

ans =

    12

>> p = parpool('Processes', feature('numcores'))
Starting parallel pool (parpool) using the 'Processes' profile ...
Error using parpool (line 133)
Too many workers requested. The cluster "Processes" has the NumWorkers property set to a maximum of 2
workers but 12 workers were requested. Either request a number of workers less than NumWorkers, or
increase the value of the NumWorkers property for the cluster (up to a maximum of 512 for the Local
cluster).

>> p = parpool('Threads', feature('numcores'))
Error using parpool (line 108)
A minimum pool size of 12 was requested. The maximum thread-based pool size is currently 2.
 
```
This seems related with [hyper-threading setting](https://www.mathworks.com/matlabcentral/answers/463068-number-of-cores-by-default) on my laptop. When use maxNumCompThreads('automatic'), it returns just 2!
```MATLAB
>> maxNumCompThreads('automatic')

ans =

     2
```
So we used maxNumCompThreads('automatic') instead of feature('numcores') to get the actual number of cores? Our additional questions are:
* How many actual/physical cores does my laptop have?
* Will disabling hyper-threading would resolve this issue?

Additional resources on parallelization in MATLAB:
* [Choose Between Thread-Based and Process-Based Environments](https://www.mathworks.com/help/parallel-computing/choose-between-thread-based-and-process-based-environments.html)
* [Run Code on Parallel Pools](https://www.mathworks.com/help/parallel-computing/run-code-on-parallel-pools.html)
* [Choose Between spmd, parfor, and parfeval](https://www.mathworks.com/help/parallel-computing/choose-spmd-parfor-parfeval.html)


