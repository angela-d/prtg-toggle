# Sample Log Extraction

I'm providing a sample script you can use to test the functionality of PRTG toggle, without having it process a full log and giving you a bunch of useless data in return.  

This script is *not* a pre-requisite to PRTG Toggle and doesn't need to be utilized at all, in order for the toggle to function.

It is provided as an optional starting point for those that want to use PRTG Toggle but don't know where to start.

## What this script does
It extracts data from a log that you find interesting, discards the rest.  You can then use PRTG Toggle to get the interesting data hooked into PRTG's alerts.

## Modify the config lines of [logger](logger)
- REMOTEPATH = The log you want to analyze
- LOGCOPY - A copy of REMOTEPATH will be made to this filename/destination, so we aren't manipulating a live file that's constantly being written to
- PAYMENTSLOG - Extract **POST** and **payment** references, to see if the payment form is under attack
- LASTSENT - Needed to keep track of the last logs that were sent
- MAIL_TO - A copy of the processed log that triggered hits to the payment/POST regex will be sent here
- MAIL_FROM - User authorized to send access from the server you'll run this on

## Dependencies
If you will be sending mail, you'll want a local mailserver (or modify the mailx line to specify different options)
- Linux-based distro (may work on others, but only tested on GNU/Debian Linux)
- mailx (if you don't want email copies, just comment this line from **sendMail**)

## Customizing
The default behavior is to reset the process flow at 7pm (when the logs rotate on this example system).  
- To track behavior other than **POST** or **payments**, modify the regex in the **payments()** function.
- Modify the **admins()** function and specify the IP you don't wish to track.

If you decide to use this script in your production environment, you'll want to modify:
- **utcCleanup()** and adjust the hour your log rotates.  (Or comment out the function contents to completely disable the process flow rotation.)
- Set up a cron, that runs *before* PRTG Toggle:
```bash
38 * * * * /path/to/prtg-toggle/logger
```

## To Use
From a command-line prompt (replace /path/to/ with the path to where you cloned this repo on your system; ie. **/home/username/prtg-toggle**):
```bash
cd /path/to/prtg-toggle && ./logger
```
