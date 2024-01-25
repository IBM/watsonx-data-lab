# On-Time Performance Dataset

The Airline On-Time performance database contains information on flights within the US from 1987 through 2020. This is a very large dataset, so only the records from January 2013 have been included inside this image.

The following link provides more information on the dataset and the columns that are found in the records. Note that in the version of the data used in this system does not contain the diversion records 1 through 5. These fields are blank in the data sample used. Note that the initial diversion airport does exist in the record.

<a href="https://dax-cdn.cdn.appdomain.cloud/dax-airline/1.0.1/data-preview/index.html" target="_blank">Airline Report On-Time Performance Dataset</a>

## Disclaimer

Except as expressly set forth in this agreement, the data (including enhanced data) is provided on an "as is" basis, without warranties or conditions of any kind, either express or implied including, without limitation, any warranties or conditions of title, non-infringement, merchantability or fitness for a particular purpose.

Neither you nor any data providers shall have any liability for any direct, indirect, incidental, special, exemplary, or consequential damages (including without limitation lost profits), however caused and on any theory of liability, whether in contract, strict liability, or tort (including negligence or otherwise) arising in any way out of the use or distribution of the data or the exercise of any rights granted hereunder, even if advised of the possibility of such damages.

## Tables

#### AIRCRAFT

|Column|Type
|------|------|
|TAIL_NUMBER| VARCHAR| 
|MANUFACTURER| VARCHAR| 
|MODEL| VARCHAR

#### AIRLINE_ID

|Column|Type
|------|------|
|Code| INT| 
|Description| VARCHAR|

#### AIRPORT_ID

|Column|Type
|------|------|
|Code| INT| 
|Description| VARCHAR|

#### CANCELLATION
|Column|Type
|------|------|
|Code| INT| 
|Description| VARCHAR|

#### ONTIME
|Column|Type
|------|------|
|Year| INT| 
|Quarter| INT| 
|Month| INT| 
|DayofMonth| INT| 
|DayOfWeek| INT| 
|FlightDate| VARCHAR| 
|Reporting_Airline| VARCHAR| 
|DOT_ID_Reporting_Airline| INT| 
|IATA_CODE_Reporting_Airline| VARCHAR| 
|Tail_Number| VARCHAR| 
|Flight_Number_Reporting_Airline| INT| 
|OriginAirportID| INT| 
|OriginAirportSeqID| INT| 
|OriginCityMarketID| INT| 
|Origin| VARCHAR| 
|OriginCityName| VARCHAR| 
|OriginState| VARCHAR| 
|OriginStateFips| VARCHAR| 
|OriginStateName| VARCHAR| 
|OriginWac| INT| 
|DestAirportID| INT| 
|DestAirportSeqID| INT| 
|DestCityMarketID| INT| 
|Dest| VARCHAR| 
|DestCityName| VARCHAR| 
|DestState| VARCHAR| 
|DestStateFips| VARCHAR| 
|DestStateName| VARCHAR| 
|DestWac| INT| 
|CRSDepTime| INT| 
|DepTime| INT| 
|DepDelay| INT| 
|DepDelayMinutes| INT| 
|DepDel15| INT| 
|DepartureDelayGroups| INT| 
|DepTimeBlk| VARCHAR| 
|TaxiOut| INT| 
|WheelsOff| INT| 
|WheelsOn| INT| 
|TaxiIn| INT| 
|CRSArrTime| INT| 
|ArrTime| INT| 
|ArrDelay| INT| 
|ArrDelayMinutes| INT| 
|ArrDel15| INT| 
|ArrivalDelayGroups| INT| 
|ArrTimeBlk| VARCHAR| 
|Cancelled| INT| 
|CancellationCode| INT| 
|Diverted| INT| 
|CRSElapsedTime| INT| 
|ActualElapsedTime| INT| 
|AirTime| smallINT| 
|Flights| INT| 
|Distance| INT| 
|DistanceGroup| INT| 
|CarrierDelay| INT| 
|WeatherDelay| INT| 
|NASDelay| INT| 
|SecurityDelay| INT| 
|LateAircraftDelay| INT| 
|FirstDepTime| INT| 
|TotalAddGTime| INT| 
|LongestAddGTime| INT| 
|DivAirportLandings| INT| 
|DivReachedDest| INT| 
|DivActualElapsedTime| INT| 
|DivArrDelay| INT| 
|DivDistance| INT| 
|DivAirport| VARCHAR|