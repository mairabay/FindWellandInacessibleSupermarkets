#### FindWellandInacessibleSupermarkets ####

This Talend project finds all the supermarkets in the city of Welland, ON that are not accessible by publc transit.

### Methodotology: ###

## Technology ##

The Talend version used was Talend Open Studio for Enterprise Service Bus 6.3.0. The most recent version of Talend is available for download here: https://www.talend.com/download/talend-open-studio?qt-product_tos_download_new=3#qt-product_tos_download_new
 
## Data ##

The following are downloaded dynamically from the Niagara Open Data portal (http://niagaraopendata.ca) via HTTP GET request in the Talend job:
- CSV file with all Transit stops for Welland
- CSV file with all supermarkets in Niagara

This way we are sure we're always working with the most recent version of the dataset.

## Calculation ##

For accessibility the initial idea was to compare addresses or street names, but unfortunately, the Publc Transit Data for Welland does not provide an address for most stops, but rather an intersection. So the solution found was to compare latitude and longitute.

Acessibility of a supermarket was defined as being within a 5-min walk, which is 1/4 mile. The 1/4 mile is a rule used by urbanists. Despite it not being the most accurate one (see http://evstudio.com/the-five-minute-walk-calibrated-to-the-pedestrian/) we decided to use it for smiplicty and development time sake. 

To find out what a 1/4 mile means in latitute and longitude, we used this webste: http://andrew.hedges.name/experiments/haversine/. That gave us 0.005 decimals of longitute and 0.0036 decmals of latitude for the Niagara region.

Finally, to fnd out if a supermarket is within walking distance of a bus stop, we check to see if the lat+long of the supermarket is within the lat+long of each bus stop +-1/4 mile.

## Deployment ##

The Talend job can be compiled into an OSGi Bundle (https://en.wikipedia.org/wiki/OSGi) and deployed in Talend's Runtime container (a web container based on Apache Karaf: https://karaf.apache.org). This will make the job available to other applications as a RESTful web service, returning a JSON file with the resulting inaccessible supermarkets.

The application is currently configured to be available on the follwing URL: "/getWellandInacessibleSupermarkets" . When running locally, the resulting JSON can be seen via browser request to http://localhost:8088/getWellandInacessibleSupermarkets (provided the "RESTService" job is running in Talend ESB or the "RESTService-0.1.jar" is deployed in a local Talend Runtime container).

Talend's Runtime container comes with the ESB installation .zip file (see link in the Technology section above).

To deploy Talend Runtime on a server, follow instructions here: https://help.talend.com/display/TalendESBInstallationGuide60EN/3.8.1+Installing+the+Talend+Runtime+containers.

## Docker - NEW! ##

The Talend job has now been updated to run on Docker!

You can export the Docker-RESTService job or you can just use the Docker-RESTService-0.1.jar included in this repository and deploy it into any Docker container running Talend Runtime 6.3.0 (you can use my container, found here: https://hub.docker.com/r/mairabay/talend-runtime-esb-6.3.0/).

Or if you want, there is also a full Docker container with the Web Service already depolyed in Talend Runtime. That can be found here: https://hub.docker.com/r/mairabay/niagara-open-data-demo/.

## API - NEW! ##

The complete Docker container is also deployed in https://niagaraopendatademo.azurewebsites.net/services/services/getWellandInaccessibleSupermarkets. 
You can use that URL as an API service, to get the resulting JSON from this web service.

## Front-End - NEW! ##

We finally have a front-end for this app!!! You can access it here: https://niagaratransitacessibility.bubbleapps.io/

### Desired future enhancements: ###

- Display all supermarkets in the map at the same time
- Add other location types and cities

### License: ###

See license file for the license.
