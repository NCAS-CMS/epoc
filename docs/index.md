# EPOC simulations

Documentation for the EPOC runs performed on ARCHER2 by NCAS. 

## Documentation related to running an EPOC workflow 

* [Setup required](setup)
* [Reauthenticating your Globus endpoints](reauthenticating_globus)
* [Starting a new hist-1950 or 4xCO2 ensemble member](ensemble_member)
* [Starting a new highres-future ensemble member](highres_future_member)
* [Handling model failures](model_failures)
* [Handling `modify_netcdf_metadata` failures](modify_netcdf_failures)

## Production workflows

### LL runs

| Experiment | Ens 1 |  Ens 2 | Ens 3 |  Ens 4 | Ens 5 |
|:--- | --- | --- | --- | --- | --- |
| control-1950 | [u-cz422](suites/cz422) | 
| hist-1950 | [u-di500](suites/di500) | [u-dm619](suites/dm619) | [u-dn862](suites/dn862) | [u-do025](suites/do025) | [u-dn910](suites/dn910) |
| highres-future | [u-do964](suites/do964) | [u-dp562](suites/dp562) | [u-dp581](suites/dp581) | [u-dp762](suites/dp762) | [u-dp856](suites/dp856) | 
| hist-1950-noghg | [u-di516](suites/di516) | [u-dn997](suites/dn997) | [u-dp388](suites/dp388) | [u-do356](suites/do356) | [u-do530](suites/do530) |
| highres-future-noghg | [u-dp359](suites/dp359) | [u-dp563](suites/dp563) | [u-dp933](suites/dp933) | [u-dp798](suites/dp798) | [u-dp940](suites/dp940) | 
| abrupt 4xCO2 | [u-dg773](suites/dg773) | [u-dh773](suites/dh773) | [u-di233](suites/di233) | [u-dj758](suites/dj758) | [u-dk751](suites/dk751) |

### HH runs

| Experiment | Ens 1 | Ens 2 | Ens 3 | Ens 4 |
|:--- | --- | --- | --- | --- |
| control-1950 | [u-cx749](suites/cx749) | 
| hist-1950 | [u-di997](suites/di997) | [u-du798](suites/du798) |
| highres-future | [u-dp361](suites/dp361) | 
| hist-1950-noghg | [u-dj424](suites/dj424) |
| highres-future-noghg | [u-dp497](suites/dp497) | 
| abrupt 4xCO2 | [u-dg290](suites/dg290) | [u-di531](suites/di531) + [u-dr720](suites/dr720)| [u-dk074](suites/dk074) | [u-dr689](suites/dr689) |
