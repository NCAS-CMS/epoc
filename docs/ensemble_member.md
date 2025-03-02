# Starting a new ensemble member 

First, check with Jon and Dan for the base run to copy and the initial conditions required. 
You should also confirm the n02 project code for Archer2 and the GWS on Jasmin. 

1. Copy the original job, e.g. u-dm619, by running `rosie copy`. 
2. Update user settings.
   * Set the username, and account group if required.
     In the rose edit GUI this is under "suite conf &rarr; project accounting", or edit `rose-suite.conf`.
   * Set your XFC space. In `flow.cylc` change `TRANSFER_DIR`. 
3. Update the restart files.
   * Set the atmosphere dump.
     Edit `ainitial` in `app/um/rose-app.conf` or "um → namelist → reconfiguration and ancillary control → general technical options".
   * Set the NEMO/CICE restarts.
     These are set in `app/nemo-cice/opt/rose-app-orca1_spunup.conf`.
     (For HH they are set in `rose-app-orca12_spunup.conf`.)
4. Update the ensemble number & metadata.
   * Set `ENSEMBLE_NUM` in `rose-suite.conf` or "suite conf → template variables". 
   * Edit `branch_time_in_parent` in `app/modify_netcdf_metadata/rose-app.conf` or "modify_netcdf_metadata → model metadata → all".
     This should be the number of days since the reference time which is 1950-01-01 (using 360d calendar).
5. Check suite settings:
   * The following code branch should **not** be included in `fcm_make_um/rose-app.conf`: 
     ~~~
     branches/dev/simonwilson/vn11.6_stochastic_header
     ~~~
     This is only required when using start dumps from an earlier UM version.
   * Ensure all tasks are switched on, including the builds, pptransfer, monitor and jdma. See "suite conf → Build and Run". 
     You do not need to select "Use memory file system" or "Clear out".
     For LL "Split Post Processing App by model" should be false; and for HH it should be true. 
   * If you want cylc alerts update `webhook_url` in `bin/notify.py`, otherwise switch these off by commenting out this line in `flow.cylc`:
     ~~~
     #            handlers = notify.py
     ~~~
6. Commit your changes.
7. Document your suite.
   * Add your um job id to the **Suite** column on the [EPOC simulations spreadsheet](https://docs.google.com/spreadsheets/d/11OfKzAq017yA3WrXKD8w5n_yYrWW3_bsiEujvGQDy5k/edit?gid=738210318#gid=738210318). The **Docs** and **Monitor** columns will be autofilled
   * Make a page on the EPOC github by clicking on the autofilled link in the **Docs** column
8. Run the first cycle.
   * You will need to run with [cylc 8](https://cms.ncas.ac.uk/cylc8/). Set `export CYLC_VERSION=8` on puma2.
   * To start the suite, run `cylc vip` from the `roses/<suite-id>` directory.
   * Launch the tui with `cylc tui <suite-id>`.
   * We need to manually hold the suite so it doesn't run too far ahead. Keep an eye on the suite until you see the second cycle appear in the tui, then select the `coupled` task and "hold". A pause symbol of two lines should appear next to this task. 
   * Once the first cycle has completed, get Dan to check the data.
9. Continue the run when ready, by releasing the held task.
