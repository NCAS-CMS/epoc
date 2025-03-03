# Reauthenticating your Globus endpoints 

The Globus certificates only last for 30 days, after which you will need to reauthenticate. 

## Renewing via the web app 

If access has expired you can renew via the [web app](https://app.globus.org/collections). 
In the collection box search for and select  "ARCHER2 file systems" it will then automatically tell you you need to reauthorize and provide a link to do so.  Repeat for "JASMIN Default Collection".

## Renewing via Archer2 

Both the Archer2 and Jasmin endpoints can also be reauthenticated from Archer2. This can be done before access expires. 

You can check your linked identities with: 
```
module load globus-cli
globus session show
```
This will list something like this: 
```
Username                      | ID                                   | Auth Time           
----------------------------- | ------------------------------------ | --------------------
annette@safe.archer2.ac.uk    | XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX | 2025-02-14 09:10 GMT
aosprey@accounts.jasmin.ac.uk | YYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYYY | 2025-02-14 09:11 GMT
```

To reauthenticate run e.g.: 
```
globus session update annette@safe.archer2.ac.uk
```
Copy and paste the resulting url into your web browser and follow the instructions.

Then repeat for your Jasmin identity, e.g.: 
```
globus session update aosprey@accounts.jasmin.ac.uk 
```

