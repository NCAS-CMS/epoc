# Starting a new highres-future ensemble member

Instructions for setting up and running a new highres-future or highres-future-noghg workflow under Cylc 8.

These runs continue from the end of the hist-1950 and hist-1950-noghg ensemble members. 

## Setting up the workflow 

1. Take a copy of the reference workflow with `rosie copy`: 
   * highres-future : u-do964
   * highres-future-noghg : u-dp359
  
2. Check the spreadsheet for the initial conditions to use. The input files can be found on Archer2 under: `/work/n02/n02/annette/EPOC/<id>/`.

3. Set these initial conditions in the workflow: 
   * Atmos restarts: Set `astart` in `app/um/rose-app.conf`
   * NEMO/CICE restarts: Set `CICE_START`, `NEMO_ICEBERGS_START` and `NEMO_START` in `app/nemo_cice/rose-app.conf`.
   * Ozone data: Set `OZONE_PRIMARY_ARCHIVE` in `rose-suite.conf`. This should have the form `/work/n02/n02/annette/EPOC/u-di516/di516a.po'` with `di516` replaced by the appropriate workflow-id. 

4. Edit the following:
   * `ENSEMBLE_NUM` in `rose-suite.conf`
   * `ARCHER2_USERNAME` in `rose-suite.conf`
   * `TRANSFER_DIR` in `flow.cylc` to point to your XFC space.
  
5. Check that reconfiguration is off and that pptransfer, JDMA and monitor are all on.

6. Ensemble number 5 includes extra diagnostics. Copy the following files from the baseline workflow:
   * `app/xml/file/iodef_orca12_1m.xml`
   * `app/xml/rose-app.conf`

## Running the workflow 

1. Run the workflow with `cylc vip`. 

2. Once the first cycle has finished, contact Dan to check the output looks OK.

3. Check the correct restart files have been included. Look at `~/cylc-run/<id>/runN/log/job/20150101T0000Z/coupled/NN/job.out` and check these variables: 
   * `CICE_START`
   * `NEMO_START`
   * `NEMO_ICEBERGS_START`
   * `astart`

4. Check the workflow has picked up the ozone archive. Look in `~/cylc-run/<id>/runN/log/job/20150101T0000Z/retrieve_ozone/NN/job.out` and you should see the final line: 
```
[INFO] Proceeding to redistribution with 24 months of data
```

5. Once you are happy that everything is working correctly, release the run with the command:
```
cylc release <id> --all
``` 

## Documenting the workflow 

1. Commit changes to the workflow.
   
2. Add your workflow id to the [EPOC simulations spreadsheet](https://docs.google.com/spreadsheets/d/11OfKzAq017yA3WrXKD8w5n_yYrWW3_bsiEujvGQDy5k/edit?usp=sharing)
 
3. Add a page on the EPOC github site. 
