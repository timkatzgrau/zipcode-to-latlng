
# Zip Code to Latitude and Longitude API

Google Sheets API has unlimited readable requests and the US zip code data set is pretty small.
So let's avoid paying ridiculous amounts of money for a geocoding API.


#  Instructions
#### Duplicate this Google Sheet
https://docs.google.com/spreadsheets/d/1XesNaSc2nUkpFSP1fsRbM761y6-IYyzsZB6Ct4nDQSo/edit?usp=sharing

#### Enable the Google Sheets API in your cloud console
https://console.cloud.google.com/apis/api/sheets.googleapis.com/

You might need to add a billing account.  Don't worry though, we get unlimited requests so you won't be charged.


#### Add A Service account
1.  Create a service account here: https://console.cloud.google.com/apis/credentials
2.  Add the email of the service account to your Google sheet as an editor
3.  Make your Google sheet viewable by anyone with the link

#### Required Stuff

SHEET ID
You can get this from the url when the Google sheet is opened
https://docs.google.com/spreadsheets/d/XXXXXXXXXXXXXXXXXXXXXXXXXXX-XXXXXXXXXXXXXXXX/edit#gid=0

API KEY
You can get this here: https://console.cloud.google.com/apis/credentials

## Usage/Examples
The beauty of this is that the zipcodes are mapped to the corresponding row in the spreadsheet. 

Zip code 12345 will read the values in row 12345 in the spreadsheet.  This saves a ton of time because we don't need to search anything when the results get returned.

We just need to make sure that the leading zeros are stripped for some zipcodes because spreadsheet rows dont use leading zeros.

```javascript
    const SHEET_ID = "XXXXXXXXXXXXXXXXXXXXXXXXXXX-XXXXXXXXXXXXXXXX";
    const API_KEY = "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX";

    var zip = "10001";
    isValidZip = /(^\d{5}$)|(^\d{5}-\d{4}$)/.test(zip);

    if(isValidZip){
        var requestOptions = {
            method: 'GET',
            redirect: 'follow'
        };
        
        while(zip[0] == "0"){
            zip = zip.substring(1);
        }        

        fetch("https://content-sheets.googleapis.com/v4/spreadsheets/" + SHEET_ID + "/values/Zip%20Codes!A" + zip + "%3AB" + zip + "?key=" + API_KEY, requestOptions)
        .then(response => response.text())
        .then(result => console.log("Lat: " + JSON.parse(result).values[0][0] + ", Lng: " + JSON.parse(result).values[0][1]))
        .catch(error => console.log("Request failed"));
    } else {
        console.log("Invalid Zip");
    }
```

