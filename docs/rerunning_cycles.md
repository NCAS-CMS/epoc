# Re-running from an earlier cycle

To re-run the previous cycle: see https://ncas-cms.github.io/epoc/model_failures#re-running-the-previous-cycle

To start from an earlier point is more complicated, 
and involves manually moving files and editing the control nanmelists. 
You may need to retrieve start dumps from tape. Only the January files are archived. 

In this example, assume you want to start from cycle *M*. 

### UM files 

Make sure the atmos restart for cycle *M* is in place in `share/data/History_data`. 

You also need to make sure the `<suite-id>.xhist` file is pointing to the correct dump. 

If it exists, you can copy this from the cycle before the one you wish to re-run, so *M-1*. 
~~~
cd work/<cycle>/coupled/history_archive 
cp temp_hist.0001 ~/cylc-run/<suite>/share/data/History_Data/<suite>.xhist
~~~

If the dirctory for cycle *M-1* no longer exists, then just edit the existing `xhist` file. 
You will also need to set the variable `H_STEPIM`. 
If you have the log file for when the cycle was originally run, 
find the timestep for the start of the cycle, e.g. 

~~~
Atm_Step: Timestep  1866240   Model time:   1986-01-01 00:00:00
~~~

You should also check that the ozone files are in place. 

### NEMO files 

Ensure the NEMO restart(s) are in `share/data/History_data/NEMOhist`. 
NEMO will chose the latest file, so make sure any later files are moved aside first. 

You also need to have the NEMO control namelist from the previous work directory. 
If `work/<cycle>/coupled/` does not exist, then create it. 
Copy in the NEMO namelist `namelist_cfg` from a cycle with a 5 min timestep. 
Edit the variables `nn_it000` and `nn_itend`. 
Again these can be found in the log file for the original job, near the start. 
~~~
[INFO] Sourcing namelist file from the work directory of the previous cycle
[SUBPROCESS OUTPUT] nn_it000=3746881,
[SUBPROCESS OUTPUT] nn_itend=3755520,
~~~

### CICE files 

Go to `share/data/History_data/CICEhist`, and edit `ice.restart_file` to point to the appropriate file 

### Triggering tasks 

At Cylc 8 the best way to rollback to an earlier cycle is to start a new flow from the point you wish to start from. This will then run all tasks from this point onwards, e.g.:  
```
cylc trigger --flow=new u-dy398 21030101T0000Z/coupled
```
You then need to stop the old flow. To do this you need to know the flow numbers which you can see from the scheduler log or GUI. 
```
cylc stop --flow=1 u-dy398
```
You will need to tigger each task in the new cycle manually or set pre-requisites. 
