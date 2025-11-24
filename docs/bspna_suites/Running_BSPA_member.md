# Starting a new BSPNA ensemble member 

First, check with Jon and Dan for the base run to copy and the initial conditions required. 
You should also confirm the n02 project code for Archer2 and the GWS on Jasmin. 

1. Copy the original job, u-ds212, by running `rosie copy`. 
2. Update user settings.
   * Set the username, and account group if required.
     In the rose edit GUI this is under "suite conf &rarr; project accounting", or edit `rose-suite.conf`.
   * Set your XFC space. In `flow.cylc` change `TRANSFER_DIR`. 
3. Update the restart files.
   * Set the atmosphere dump.
     Edit `ainitial` in `app/um/rose-app.conf` or "um → namelist → reconfiguration and ancillary control → general technical options".
   * Set the NEMO/CICE restarts.
     These are set in `app/nemo-cice/opt/rose-app-orca12_spunup.conf`.
4. Update the ensemble number & metadata.
   * Set `ENSEMBLE_NUM` in `rose-suite.conf` or "suite conf → template variables". 
   * Edit `branch_time_in_parent` in `app/modify_netcdf_metadata/rose-app.conf` or "modify_netcdf_metadata → model metadata → all".
     This should be the number of days since the reference time which is 1950-01-01 (using 360d calendar).
5. Check suite settings:
   * Ensure all tasks are switched on, including the builds, pptransfer, monitor and jdma. See "suite conf → Build and Run". 
     You do not need to select "Use memory file system" or "Clear out".
   * If you want cylc alerts store your webhook_url in the file `~/.notify/webhook`  - this will be picked up by `bin/notify.py`, otherwise switch these off by commenting out this line in `flow.cylc`:
     ~~~
     #            handlers = notify.py
     ~~~
6. Commit your changes.
7. Document your suite.
   * Add your um job id to the **Suite** column on the [BSPNA simulations spreadsheet](https://docs.google.com/spreadsheets/d/1UNTNaphWJ1l3E8UryxRGrnxWaLYiY9NPnS1chptYPTA/edit?gid=738210318#gid=738210318). The **Docs** and **Monitor** columns will be autofilled
   * Make a page on the BSPNA github by clicking on the autofilled link in the **Docs** column
8. Run the first cycle.
   * You will need to run with [cylc 8](https://cms.ncas.ac.uk/cylc8/). Set `export CYLC_VERSION=8` on puma2.
   * To start the suite, run `cylc vip` from the `roses/<suite-id>` directory.
   * Launch the tui with `cylc tui <suite-id>`.
   * Once the first cycle has completed, get Dan to check the data.
   * Once these checks are complete, release the suite with `cylc release <suite-id> --all`
9. Continue the run when ready, by releasing the held task.
