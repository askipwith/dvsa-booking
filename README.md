# DVSA booking script
Change your UK practical driving test to a new location within a specified dates and times (with preferences)

# Usage
You need to have a test booked and paid for, your driving license number and your test booking reference. This script will attempt to find a better location and / or date. It won't automatically make the booking, rather leave you on the test centre's booking page with the best slot selected and an audible 'attention' alert. You then go ahead and change the booking manually. The script can be scheduled to run at a given time e.g. at 0600 when new bookings tend to get released each day.

# Setup
1. Book and pay for any available driving test
2. Update default script configuration settings
3. Enable audio in chrome site preferences for the DVSA site so that sound can autoplay
4. Enable script in tampermonkey
5. Load https://www.gov.uk/change-driving-test?dvsaConfigReset. NB use the dvsaConfigReset parameter to reset stored and settings and login

# Default script configuration settings

Candidate parameters:
 - dvsaConfigLN = "[add your number]" - License Number, driving license number for the candidate
 - dvsaCongigTRN = "[add your number]" - Test Reference Number, test reference for the candidate

Delay parameters:
 - dvsaConfigLT = "2024-09-01T05:59:00" - Launch Time, format yyyy-mm-ddThh:mm:ss OR blank for immediate
 - dvsaConfigAPI = 1000 * 10 - Average Page Interval, average time (ms) between page requests to reduce risk of WAF block
 - dvsaConfigASI = 1000 * 60 * 10 - Average Search Interval, average time (ms) between test centre searches to reduce risk of WAF block

Search parameters - test centre:
 - dvsaConfigSL = "BR1" - Search Location - Search Location, NB use a postcode to retrieve multiple centres
 - dvsaConfigMC = 8 - Minimum Centres, minimum number of centres to search

Search parameters - dates and times:
 - dvsaConfigFTD = "2025-01-01" - First Test Date, the earliest date for a test, format yyyy-mm-dd
 - dvsaConfigLTD = "2025-03-30" - Last Test Date, the latest date for a test, format yyyy-mm-dd
 - dvsaConfigITD = "2025-01-23" - Ideal Test Date, the ideal date for a test, format yyyy-mm-dd
 - dvsaConfigFTT = "10:30" - First Test Time, the earliest time for a test, format hh:mm 24hr
 - dvsaConfigLTT = "16:30" - Last Test Time, the latest time for a test, format hh:mm 24hr
 - dvsaConfigITT = "12:00" - Ideal Test Time, the ideal time for a test, format hh:mm 24hr

Alert parameters:
 - dvsaConfigWBA = true - WAF Block Alert, issue audible alert on a WAF block if true
 - dvsaConfigADA = true - Available Date Alert, issue audible alert when available date found if true

Be sure to add your license number, test reference and modify the search dates. Use a postcode in the loction as this script is designed to search through multiple centres. Decrease the wait times to increase search speed and frequency. However you may upset their firewall which will often issue a CAPCHA or block access entirely for a period.

# Search loop
Nagivate to the DVSA 'change practical driving test' page and click the green button. The bot will then:
1. Fill in your details and log in
2. Search for centres closest to the dvsaConfigSL
3. Keep loading centres until it has at least dvsaConfigMC
4. Search each centre for available slots that match dvsaConfigFTD, dvsaConfigLTD, dvsaConfigFTT, dvsaConfigITT
5. Load the test centre booking page for the closest centre with a matching slot
6. Select the best slot using dvsaConfigITD, dvsaConfigITT and make an alert sound...
7. ...or wait for dvsaConfigASI and try again in a loop
   
It will also try and deal with a range of DVSA issue pages (service unavailable, max search limit, 500 error, time out, log out etc.). When running, look in the console log to see what is going on in more detail.

# Updating configuration settings
As soon as the script is run, the default settings are copied into storage which will persist, even when default settings are changed in the script. To re-load (updated) default settings add 'dvsaConfigReset' to any of the matching URLs. To change any of the settings via the URL, add the parameter and new value to the URL. Any settings updated in this way will persist. Example: to reset stored values to the script's latest default settings and then disable audible alerts for a WAF block load: https://www.gov.uk/change-driving-test?dvsaConfigReset&dvsaConfigWBA=false

# Timed launch - method 1
Set the 'dvsaConfigLT' config parameter to a future date and then load: https://www.gov.uk/change-driving-test - the script will wait. However I have had issues with the WAF blocker using this method.

# Timed launch - method 2
Create an application to run a shell script to launch chrome with the required URL. Then use a method to run that script at a given time. I use a mac: application is created in automator, and I use the calendar to launch the application, using a custom alert to open the application file. This seems to work better. Test your own computer settings to ensure that (a) it does not sleep and (b) there are no 'screen lock', 'focus' or 'do not disturb' settings that will prevent the run.

# Notes on the WAF blocker
It's pretty harsh and unpredictable. Keep your dvsaConfigAPI, dvsaConfigASI, dvsaConfigMC settings to something reasonable. Running incognito may help, but not when using a timed launch as the WAF seems to insist on a CAPTURE immediately.

# WAF CAPTCHA or Block / Available Date alerts
If enabled via configuration settings, the script will make an alert sound and wait for manual input. Make sure your sound is enabled.

# General
I've never coded in JS... so please feel free to improve I'd be delighted. I used Microsoft's copilot which is amazing. But the script will look horrible to someone that knows what they are doing I'm sure.
