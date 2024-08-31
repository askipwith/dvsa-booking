# dvsa-booking
Change your driving test to a new location within a couple of search dates

# Setup
1. Update script configuration settings
2. Enable audio in chrome site preferences for the DVSA site so that sound can autoplay
3. Login to DVSA site with your usual
4. Enable script in tampermonkey

# Script configuration settings
 - LICENSE_NUMBER = "[add your number]" - candidate's driving license number
 -  TEST_REFERENCE_NUMBER = "[add your number]" - candidate's driving test referene number
 -  TEST_LOCATION = "BR1" - the location you want to book - NB use a postcode to retrieve multiple centres
 -  FIRST_TEST_DATE = new Date("2024-09-07") - the earliest date for a test. Format YYYY-MM-DD
 -  LAST_TEST_DATE = new Date("2024-1209-17") - the latest date for a test. Format YYYY-MM-DD
 -  MIN_CENTRES = 16 - minimum number of centres to search
 -  AVG_PAGE_INT - average wait time (ms) between page requests to reduce risk of WAF block
 -  AVG_SEARCH_INT - average wait time (ms) between test centre searches to reduce risk of WAF block

Be sure to add your license number, test reference and modify the search dates. Use a postcode in the loction as this script is designed to search through multiple centres. Decrease the wait times to increase search speed and frequency. However you may upset their firewall which will often issue a CAPCHA or block access entirely for a period.

# Usage
You need to have a test booked and paid for, hence a test booking reference. This script wil attempt to find a better location and / or date. It won't automatically make the booking, rather leave you on the closest matching test site page with an audible 'attention' alert. You then go ahead and make the booking manually.

# Search loop
Nagivate to the DVSA 'change practical driving test' page and click the green button. The bot will then:
1. Fill in your details and log in
2. Search the TEST location
3. Keep loading centres until it has at least MIN_CENTRES
4. Search each centre for avaiable dates between FIRST_TEST_DATE and LAST_TEST_DATE
5. Load the test centre booking page for the closest centre with a matching date...
6. ...or wait for AVG_SEARCH_INT and try again in a loop
It will also try and deal with a range of DVSA issue pages (service unavailable, max search limit, 500 error, time out, log out etc.). When running, look in the console log to see what is going on in more detail.

# WAF CAPTCHA or Block
The bot will make an alert sound and wait for manual input

# General
I've never coded in JS... so please feel free to improve I'd be delighted. I used Microsoft's copilot which is amazing. But the script will look horrible to someone that knows what they are doing I'm sure.
