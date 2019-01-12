Concourse healthcheck
=====================

Run a test job hourly to test that the Concourse platform and Vault which most jobs depends on are alive.

What?
-----

The job runs a simple shell script that checks if a value returned from Vault is "ok". If it is,
it pushes a value to shortkeep (a HTTP frontend to Redis which can store documents with an expiration time).

What if?
--------

If the vault-test fails, expire the document in Redis (almost, 1s) immediately. Most likely if Vault fails,
the job never gets to the part where it would execute the script and just gives an error getting the parameter
from Vault. In this case the document expires in two hours and checks for its presence will fail.

The document can be watched for with any HTTP monitoring tool through the simple HTTP interface provided
by shortkeep.


References
----------

shortkeep - https://github.com/varesa/shortkeep
