# Handling model failues 

## Diagnosing errors

Check the job.err file for the cause of the error. 

* Timeouts, node failure, network error (e.g. `Fatal error in PMPI_Init_thread`), memory error (e.g. `Address not mapped to object.`)
  * Just re-trigger the job. 

* `MPI_Abort` means the model failed: 
  * Check model files for the source of error:
  * NEMO errors appear in `work/<cycle>/coupled/ocean.output`
  * UM errors appear in `work/<cycle>/coupled/pe_output/1.<suite-id>.fort6.pe0000`
  * CICE errors appear in the `job.out`
 
Occasionally the HH model will become unstable, which also occurred in the original HighResMIP runs. 
See below for instructions on getting past these instabilities. 
These should be documented in the suite page. 

## NEMO instability

Instabilities in the ocean will appear as: 
~~~
 stpctl: the zonal velocity is larger than 20 m/s
  ====== 
 kt= 87159 max abs(U):  6.0407E+07, i j k:  2914 3389    8

...

  ===>>> : E R R O R
         ===========

 MPPSTOP
 NEMO abort from dia_wri_state
~~~

Re-run the cycle with a reduced timestep in the ocean. 

* In `rose-suite.conf` change the following:
~~~
CLOCK=7,0,0
UM_OPT_KEYS='... orca12_config_3min_1m'
~~~
* Then `rose suite-run –reload` and re-triggger the failed task 
* Pause the coupled task in the next cycle. 

Once the failed task completes successfully, revert back to the original settings: 
* In `rose-suite.conf`: 
~~~
CLOCK=4,0,0
UM_OPT_KEYS='... orca12_config_5min_1m'
~~~
* Then `rose suite-run –reload` again, and re-triggger the paused task 

## UM instability 

Atmosphere instabilities usually appear as: 
~~~
Error message: North/South halos too small for advection
~~~
Perturb the atmos dump:
* Copy the script `/work/n02/n02/annette/scripts/perturb/submit_perturb.slurm`
* Edit the month and suite-id, then submit with `sbatch`.
  
The script leaves a copy of the original and perturbed dumps as `.orig` and `.perturb`
It also sets `PYTHONHASHSEED=0` to create a deterministic perturbation. 

Once the script has completed and generated a new dump, re-trigger the failed task. 

## CICE instability 

CICE instabilities appear as: 
~~~
ABORT: Global i and j: 3347,  3604
ABORT:  Lat, Lon: 83.826494669668421,  73.000001621592844
ABORT: aice: 0.9873568057277784
...
About to call abort ice with: Vertical thermo error
ice: Vertical thermo error
~~~

In this case, reduce the ocean timestep (see above). 
