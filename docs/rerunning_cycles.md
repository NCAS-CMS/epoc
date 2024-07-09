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

You will need to re-run all tasks, including post-processing for the cycle you are re-running (*N-1*)
and these need to run before the tasks in cycle *N*. 
Follow the instructions carefully to ensure this is done correctly. 

1. Retrigger the `coupled` task for cycle *N-1*
2. Pause `postproc` for cycle *N*.
3. Reset all post-processing tasks in cycle *N-1* to waiting.
4. If the JDMA task for *N-1* had completed, you will need to delete the data from JDMA. 
   It needs to finish uploading before it can be deleted. 
   If, for example, JDMA is down and the upload hasn't started you can get JASMIN to cancel the request from the queue. 
5. Once the coupled task for *N-1* has successfully completed, trigger coupled for cycle *N*. 
6. You may need to manually re-trigger the post-processing tasks in cycle *N-1*.
   These need to be done in order, check the graph view in cylc if in doubt. 
7. Once all tasks in cycle *N-1* have completed, un-pause `postproc` in cycle *N*. 
