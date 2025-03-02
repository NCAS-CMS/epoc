# Reauthenticating your Globus endpoints 

The Globus certificates only last for 30 days, after which you will need to reauthenticate. 
Both the Archer2 and Jasmin endpoints can be reauthenticated from Archer2 

You can check your linked identities with: 
```
module load globus-cli
globus session show
```
This will list something like this: 

Username                      | ID                                   | Auth Time           
----------------------------- | ------------------------------------ | --------------------
annette@safe.archer2.ac.uk    | XXX | 2025-02-14 09:10 GMT
aosprey@accounts.jasmin.ac.uk | YYY | 2025-02-14 09:11 GMT

To reauthenticate run e.g.: 
```
globus session update annette@safe.archer2.ac.uk
```
Copy and paste the resulting url into your web browser and follow the instructions.

Then repeat for your Jasmin identity, e.g.: 
```
globus session update aosprey@accounts.jasmin.ac.uk 
```

