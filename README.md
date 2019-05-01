# api.weather.gov
### Community discussion for the US National Weather Service API
Welcome! This repository is here to serve as a meeting place for developers in the general public to interface with each other and the NWS API development team. This will allow us to gather better feedback on user needs and respond more quickly to concerns.

#### Note: This forum is **not** for outages or operational issues!
Any outages or other technical operational issues with the live service should be reported to the NWS Telecommunications Operations Center by emailing TOC.NWSTG@noaa.gov or calling (301) 713-0902.

### What's the API?
api.weather.gov (which we'll simply call "API" or "the API" from now on) represents the public face of the next generation of data services from the [National Weather Service](https://weather.gov). It offers public access to a wide range of essential weather data, in a way that modern web developers expect: a REST-style, JSON-based web service.

The API is also being built to provide a powerful and modern platform for data dissemination for both internal and external customers. We're doing this to help eliminate some internal redundancies, giving you and our own forecasters and developers a one-stop shop for vital data.

The API also provides the data backend that will power the next release of our popular Forecast web site (version 3), which will go live at forecast.weather.gov later this year.

### I'm new to this. How do I use the API?
Our current documentation is located at https://www.weather.gov/documentation/services-web-api

We also provide a service definition in [OpenAPI v3.0](https://swagger.io/specification/) (previously known as Swagger) format. You can access this file at https://api.weather.gov/openapi.json (in JSON format) or https://api.weather.gov/openapi.yaml (in YAML format).

Much of the data returned from the API is in [GeoJSON](http://geojson.org/) (RFC 7946) format. Certain endpoints also offer useful alternative formats, such as [OASIS Common Alerting Protocol (CAP) XML](http://docs.oasis-open.org/emergency/cap/v1.2/CAP-v1.2-os.html) for alerts data.

<a name="how-to-get-forecast"></a>
### How do I get a forecast for a location from the API?
You'll need to know the latitude and longitude of the location in decimal degrees. (If you want to get really geospatially technical, your location should be a WGS 84 or EPSG 4326 coordinate.)
* For our example here, we'll use the Washington Monument in Washington, D.C. It's located at 38.8894 latitude, -77.0352 longitude.
* Please note that, for efficiency purposes, the API doesn't support more than four decimal places of precision in coordinates. If you send a more precise coordinate, you'll receive an error giving you the closest proper coordinate. Four decimal places is about 30 feet (10 meters) over most of the United States, so that should still be close enough!

Once you know the latitude and longitude, it's an easy three-step process from there. You can follow along in your browser with the links below:

1. Retrieve the metadata for your location from `https://api.weather.gov/points/{lat},{lon}`.
   * For our example the URL will be https://api.weather.gov/points/38.8894,-77.0352
2. You'll get back a JSON document. Inside the document, find the `properties` object, and inside that, find the `forecast` property. You'll find another URL there.
   * For our example this gives us https://api.weather.gov/gridpoints/LWX/96,70/forecast
   * You can also get the hour-by-hour forecast from the `forecastHourly` property. For our example it's https://api.weather.gov/gridpoints/LWX/96,70/forecast/hourly
3. Retrieve that URL. You'll get a JSON document containing the forecast for that location. There you go!

We're still working on documentation for the JSON that API returns, but we think it's pretty easy to understand if you just look at it. If you need more reference, the forecast JSON very closely aligns with the information you'd see on a web page on forecast.weather.gov. If you still have questions, [see below](#user-content-questions-comments) for how to ask.

### I'm getting an Access Denied (403) error from the API.
Make sure your program is including a `User-Agent` header in your request. We recommend setting the value to something that identifies your application and includes a contact email. This helps us contact you if we notice unusual behavior, such as your program consuming a high amount of resources.

In the future we will replace the User-Agent requirement with a more typical API key system.

If you're including the User-Agent header and are still having problems, please [let us know](#user-content-questions-comments).

### I already use some of the NWS web services on forecast.weather.gov. How can I switch my application to use the API?
Depending on what service you're using, the data may already be available via the API.

#### I use the Digital Weather Markup Language service on forecast.weather.gov.
You can follow [the directions from earlier](#user-content-how-to-get-forecast) on how to get a forecast. On the third step, you can request the DWML instead of the JSON forecast by adding an `Accept` header with the value `application/vnd.noaa.dwml+xml` to your request. (You'll need to consult the documentation for your program's HTTP library on how to add a request header.)

We would encourage you to change your application to use the new JSON forecast format, as this will be our main focus for support going forward.

#### I use the JSON service on forecast.weather.gov.
The API won't support the existing JSON format, but take a look at the new JSON format [as described earlier](#user-content-how-to-get-forecast). We think you'll agree that it's much simpler to use in your application.

#### I'm not able to switch to using the API just yet.
You can continue to use the existing legacy services by changing the URL your application uses from `forecast.weather.gov` to `f1.weather.gov`. This server will continue to provide the legacy services after we launch Forecast v3.

Please note that f1.weather.gov is a temporary compatibility service that will be shut down at a future date. You will still need to move to API eventually, but this will allow you some additional time to transition.

#### I'm using a service that isn't described here.
Please [give us some feedback](#user-content-questions-comments) on what you're using, so we can evaluate it for future inclusion into the API, or point you to the best place to get the information.

<a name="questions-comments"></a>
### I have some questions or comments.
If you'd like to ask a question or share some comments, you can do so by simply [opening a new issue on this repository](https://github.com/weather-gov/api/issues/new).
