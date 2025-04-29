# Starting a new highres-future ensemble member

Instructions for setting up and running a new highres-future or highres-future-noghg workflow under Cylc 8.

These runs continue from the end of the hist-1950 and hist-1950-noghg ensemble members. 

## Setting up the workflow 

1. Take a copy of the reference workflow with `rosie copy`: 
   * highres-future : u-do964
   * highres-future-noghg : u-dp359
  
2. Check the spreadsheet for the nitial conditions to use. The input files can be found on Archer2 under: `/work/n02/n02/annette/EPOC/<id>/`.

3. Set these initial conditions in the workflow: 
   * Atmos restarts: Set `astart` in `app/um/rose-app.conf`
   * NEMO/CICE restarts: Set `CICE_START`, `NEMO_ICEBERGS_START` and `NEMO_START` in `app/nemo_cice/rose-app.conf`.
   * Ozone data: Set `OZONE_PRIMARY_ARCHIVE` in `rose-suite.conf`. This should have the form `/work/n02/n02/annette/EPOC/u-di516/di516a.po'` with `di516` replaced by the appropriate workflow-id. 

4. Edit the following:
   * `ENSEMBLE_NUM` in `rose-suite.conf`
   * `ARCHER2_USERNAME` in `rose-suite.conf`
   * `TRANSFER_DIR` in `flow.cylc` to point to your XFC space.
  
5. Check that reconfiguration is off and that pptransfer, JDMA and monitor are all on. 

## Running the workflow 

## Documenting the workflow 

1. Add the new suite id to the spreadsheet
2. Add a page to github 
