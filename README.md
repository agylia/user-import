# User Imports

## Overview
Agylia enables you perform automated user imports. You can use this facility to schedule user imports on a regular cycle. This guide explains what you need to do to use the automated user import feature.

## Import File
First you need a file that contains the user information you would like to add to your Agylia endpoint. Ideally this file is in a compatible format with the CSV file used within your Agylia administration portal. 

> TIP: You can download a template CSV file by using the **Import users** option from the **Users** page in your Admin portal; you can test your import file manually by using the same Import user page.

## FTP
After you are happy that you have a compatible file, you need to upload the file to an FTP server.

Agylia provides an FTP server, secured by using TLS/SSL (FTPS), which you can use to upload your file: 

`ftp-<region>.portal-agylia.com`

Region can be either `eu` or `us`, e.g. `ftp-eu.portal-agylia.com`.

> NOTE: Only the EU site is available at this time.

First you should get in contact with us so we can create you an FTP user. The user name for your FTP account will be the first part of you Aglia Admin Portal hostname, e.g. if your hostname is _example.portal-agylia.com_ your user name will be _example_. The password for your FTP account is your API Key. You can find your API Key from your Admin portal (Settings | General | API Key). 

#### Example
The following example shows how to connect to the EU Agylia FTP site, with the user name _example_ and the password _9c52047e63db436bf0b990e6910d71aedf32656ef23_, by using the [lftp](https://en.wikipedia.org/wiki/Lftp) command line tool:

```
$ lftp ftp-eu.portal-agylia.com
lftp ftp-eu.portal-agylia.com:~> login example 9c52047e63db436bf0b990e6910d71aedf32656ef23
lftp example@ftp-eu.portal-agylia.com:~> ls
drwxr-xr-x    2 0        0            4096 Aug 22 16:51 log
drwxr-xr-x    2 1000     1000         4096 Aug 22 15:39 reports
drwxr-xr-x    2 0        0            4096 Aug 23 08:49 tmp
drwxr-xr-x    2 1000     1000         4096 Aug 22 17:07 uploads
lftp example@ftp-eu.portal-agylia.com:/>
```

## Upload
You must upload your user import CSV file into the `/uploads` folder. The file must be called `user-import.csv`. You should overwrite this file each time you want to perform a user upload.

The job that processes the upload runs once a day at midnight UTC by default. However, please let us know if you would like to change this scheduling. The import job only processes uploads that have not processed before, so don't worry about leaving the file on the FTP server.

> TIP: The import job creates a SHA1 hash of the upload file and places the hash in your `/tmp` folder; this is how the import job knows if it has processed that version of the `user-import.csv` file before. Therefore, if you want retry an import then remove all the SHA1 hash files in the folder.

The import job writes to the `/log/user-import.log` as it processes your import file, so you can look at this file to see if everything worked as expected with the upload.

> NOTE: The log file does not contain detailed information about the processing of the user records within the file. This information is sent by email after Agylia has processed all the users.

Finally, the process runs in the context of a nominated administrator account. We recommend creating a global admin user for this specific purpose, e.g. a user called _FTP User_. The email address associated with this user will receive the email notifications from Agylia, including the status report, on completion of the bulk processing. The status report contains a record by record indication of success for each user update. The import job needs to know the identity (but not the password) for your selected admin account. Please let us know which account you’d like to use and we will update the import job for you.

## Summary
In summary, to successfully set-up user imports you need to:

1. Let us know you'd like use the user import feature (so we can provision you an FTP User)
2. Let us know which global administrator account to act as (so you can receive email updates)
3. Copy a `user-import.csv` file to the `/uploads` folder (so we process your users)

If you need any help or have any questions please let us know, we're happy to help.


