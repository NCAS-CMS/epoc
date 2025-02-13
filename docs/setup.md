# Setup required to run an EPOC ensemble member

In order to run an EPOC workflow, you must have the following accounts:
* [Archer2 and Puma2](https://cms.ncas.ac.uk/archer2/unified-model/#getting-started)
* [MOSRS](https://code.metoffice.gov.uk/)
* [Jasmin](https://jasmin.ac.uk/users/access/)

You will then need to complete the following setup steps:  
* Configure your [Archer2 and puma2 environments to run the UM](https://ncas-cms.github.io/um-training/getting-setup-selfstudy.html)
* Setup [globus](https://cms.ncas.ac.uk/unified-model/pptransfer-globus/) to transfer data from Archer2 to Jasmin
* Setup [Jasmin transfer cache](https://ncas-cms.github.io/canari/xfc) for staging of data on Jasmin. 
* Setup [JDMA](jdma) for migration of data to tape. 
* Setup epoc GWS access on Jasmin:
  * Request epoc GWS access via the [Jasmin accounts portal](https://accounts.jasmin.ac.uk/).
  * Then contact Jon to be made a GWS deputy
  * Then contact the Jasmin helpdesk for tape access. 
