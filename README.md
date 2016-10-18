# FindWellandInacessibleSupermarkets #

This Talend project finds all the supermarkets in the city of Welland, ON that are not accessible by publc transit.

## Methodotology: ##

### Technology ###

The Talend version used was Talend Open Studio for Data Integraton 5.6.1. The most recent version of Talend is available for download here: https://www.talend.com/download/talend-open-studio#t4

### Data ###

The following were downloaded from the Niagara Open Data portal (http://niagaraopendata.ca):
- XLS file with all Transt stops for Welland
- CSV file with all supermarkets in Niagara

The XLS file had to be opened and saved in XLSX format because Talend was not detecting the encoding of the XLS.

### Calculation ###

For accessibility the initial idea was to compare addresses or street names, but unfortunately, the Publc Transit Data for Welland does not provide an address for most stops, but rather an intersection. So the solution found was to compare latitude and longitute.

Acessibility of a supermarket was defined as being within a 5-min walk, which is 1/4 mile. The 1/4 mile is a rule used by urbanists. Despite it not being the most accurate one (see http://evstudio.com/the-five-minute-walk-calibrated-to-the-pedestrian/) we decided to use it for smiplicty and development time sake. 

To find out what a 1/4 mile means in latitute and longitude, we used this webste: http://andrew.hedges.name/experiments/haversine/. That gave us 0.005 decimals of longitute and 0.0036 decmals of latitude for the Niagara region.

Finally, to fnd out if a supermarket is within walking distance of a bus stop, we check to see if the lat+long of the supermarket is within the lat+long of each bus stop +-1/4 mile.

## Desired future enhancements: ##

- Create a web interface for runnng the transformation and displaying the results. (the Talend job can be compiled as a WAR and deployed in Tonmact, making it acessble to a front-end as a webs serevce, returning a SOAP envelope, as explaned here: http://www.talendforge.org/wiki/doku.php?id=doc:export_as_webservice)
- Use an HTTP GET component to read the data from the Niagara Open Data portal dinamically, to give real-time results

## License: ##

See license file for the license.
