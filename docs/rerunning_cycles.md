# Re-running the previous cycle

Sometimes it is necessary to go back and re-run the previous cycle to overcome an instability. 
Some manual steps are required to ensure that the model picks up from the correct point. 

Lets call the current failed cycle *N*, and the previous cycle that we wish to re-run *N-1*. 

## Setup model files 

### UM files 

In `share/data/History_data`, the `<suite-id>.xhist` file needs to point to the correct dump. 
You need to copy this from the cycle before the one you wish to re-run *N-2*. 
~~~
cd work/<cycle>/coupled/history_archive 
cp temp_hist.0001 ~/cylc-run/<suite>/share/data/History_Data/<suite>.xhist
~~~
Check that the `xhist` file now points to the dump for the cycle you wish to start from. 

### NEMO files 

Go to `share/data/History_data/NEMOhist`. 
Move the latest dump files out the way, e.g. if the timestamp of the latest dump was `20351101`: 
~~~
mkdir SAVE
mv *20351101* SAVE
~~~
This should leave the cycle you wish to start from as the latest NEMO dumps. 

### CICE files 

Go to `share/data/History_data/CICEhist`, and edit `ice.restart_file` to point to the appropriate file 

## Cylc 

You need to re-run the coupled task in cycle *N-1* and then cycle *N* to check you have got past the crash point. 
All the post-processing tasks for cycle *N-1* then need to be re-run in order to fully process the new data. 
It is best to pause the suite until this has completed. 

Follow the instructions carefully to ensure this is done correctly. 
If in doubt ask Annette for guidance. 

1. Retrigger the `coupled` task for cycle *N-1*
2. Reset all post-processing tasks in cycle *N-1* to waiting (so it's easier to see when tasks have been re-run).
3. Now pause the suite. 
4. If the `jdma` task for *N-1* had completed, you will need to delete the data from JDMA. 
   It needs to finish uploading before it can be deleted. 
   If, for example, JDMA is down and the upload hasn't started you can get JASMIN to cancel the request from the queue. 
5. Once `coupled` *N-1* has finished, manually trigger `coupled` for cycle *N*.
6. Once this has completed, trigger all the post-processing tasks for cycle *N-1* in order: 
   * `postproc`: This may fail with an error like `ValueError: Incorrect size for fixed length header; given 0 words but should be 256.`. Have a look at the `a.p4*` files in `History_Data`. Is there a zero-length file from the month before (so cycle *N-2*)? If so delete this file and re-try. Do not delete any other files.  
   * `compress_netcdf`: This may not show up in the graph. To insert it run: `cylc insert SUITE-ID compress_netcdf.CYCLE-POINT`
   * `modify_netcdf_metadata` 
   * `pptransfer` 
   * `jdma`: Check the old batch has been deleted first. If there is a delay here, you can proceed, but set jdma to failed first so we keep the task active and it's clear it needs to be re-run. 
8. Once all tasks in cycle *N-1* have completed, release the suite to continue the run. 
