# Chicago Taxi Data

Taxi trips are reported to the City of Chicago in its role as a regulatory agency. To protect privacy but allow for aggregate analyses, the Taxi ID is consistent for any given taxi medallion number but does not show the number.

The data set used in this system contains records from January 1st, 2013 and does not include the census tract value nor the Taxi ID.

<a href="https://data.cityofchicago.org/Transportation/Taxi-Trips/wrvz-psew" target="taxis">Taxi Trips</a>

## Disclaimer

This site provides applications using data that has been modified for use from its original source, www.cityofchicago.org, the official website of the City of Chicago.  The City of Chicago makes no claims as to the content, accuracy, timeliness, or completeness of the data provided at this site.  The data provided at this site is subject to change at any time.  It is understood that the data provided at this site is being used at one’s own risk.

## Tables

#### TAXIRIDES

|Column|Type
|------|------|
|TRIP_ID| int|
|COMPANY| varchar|
|DROPOFF_LATITUDE| double|
|DROPOFF_LONGITUDE| double|
|EXTRAS| double|
|FARE|   double|
|PAYMENT_TYPE| varchar|
|PICKUP_LATITUDE| double|
|PICKUP_LONGITUDE| double|
|TIPS|  double|
|TOLLS| double|
|TRIP_END_TIMESTAMP| timestamp|
|TRIP_MILES| double|
|TRIP_SECONDS| int|
|TRIP_START_TIMESTAMP| timestamp|
|TRIP_TOTAL| double