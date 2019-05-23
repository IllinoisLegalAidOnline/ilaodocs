===================================
Downloading Data from Acquia Lift
===================================

Note:  this requires that the computer have the AWS command line tools installed.

Step 1:  Request the exports be generated
===========================================

* Log into `Acquia Lift <https://us-east-1.lift.acquia.com/#personsearch:accountId=ILAO;`>_ 
* Go to Configure and click on Import/Export
* Click Export Data
* Set the "from" date to 31 days from the current date (the date of the last export)
* Check data exports for person, touch, event, and rankings
* Include the email address and tracking ID
* Press Ok

It may take a while to generate the exports.

Step 2:  Verify that the exports were created
==============================================

While logged into Acquia Lift and on the Import/Export page:

* Press search
* Verify that the list of exported files appear with a status of Completed
 

Step 3: Get the list of exports
==================================

Each CSV file generated is actually broken into multiple files due to their size.  To get the full list of files in our outbox:

* Open up a command line interface
* Enter aws s3 ls s3://lift.prod.api-file.us-east-1/ILAO/OUTBOX/
* This will display the list of files

Step 4: Download the individual files and move to shared drive
===============================================================
Download each file individually, from the command line.  The command to download is:

.. code-block:: php

    aws s3 cp s3://lift.prod.api-file.us-east-1/ILAO/OUTBOX/[filename] ./

This will copy the file to the current directory. Once each complete CSV is downloaded (usually 4 files), they can be moved to the shared drive:

* The folder they go in is Program/Technology Development/Acquia Lift Data
* Create a new folder for the current date formatted as yyyy-mm-dd
* Create a folder for each type of data (rankings, event, person, touch)
* Copy and paste the files for each into the correct folder
* Delete the originals from  your local downloads




