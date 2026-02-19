# Recovering from failures in `modify_netcdf_metadata` task

**Important: This script was written to fix the specific diagnostics being written by the EPOC workflows, 
and it is not applicable to other configurations. 
Any changes to the model setup, resolution, diagnostic output etc could require rewriting this code.**

Sometimes the `modify_netcdf_metadata` task will fail with a timeout or out-of-memory error. 
You need to check what state the task left the files in, and possibly take some action. **Do not just re-trigger**. 

## What does it do? 

It fixes the metadata for netCDF files written by XIOS. 
The code is contained in the suite under `app/modify_netcdf_metadata/bin`. 

Loops over files, using filename pattern matching to categorise (`modify_netcdf.py`) 
* Does various in-place metadata updates
* Fixes CICE files (`ice.py`) 
* Fixes ocean files (`ocean.py`) 
* Fixes hybrid heights for atmos files (`heights.py`): 
  - Copies file to a backup (`_being_worked_on`)  
  - Should print if backup copy completed successfully 
  - Adds heights info and orography to orignal  
  - Deletes backup
* Calculates zonal means
  - writes to a new file (`_TESTZ`)
  - then overwrites to original 

## What to do when it fails? 

Try to work out what was happening when the code stopped, by looking at the code and `job.out` file. 
In particular look out for backup files. 
* Is the `_being_worked_on` a full and complete copy of the original?
  - Can check this by doing a diff with the same (unmodified) file from the next cycle e.g:
    `diff <(ncdump -c dw850a_mon_u_197101-197101.nc_being_worked_on) <(ncdump -c ../19720101T0000Z/dw850a_mon_u_197201-197201.nc)`
  - If so roll back to this by moving the original out the way, then renaming the backup as original. 
* Otherwise does the original file look OK? 
  - Then we can roll back to this by moving the backup out the way. 
