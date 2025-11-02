# DWORSHAK BASIN DASHBOARD
Alert system for on-call engineer and resource for basin conditions and forecasting.



* Build a web-first Python app (runs locally or in the cloud) with:

* FastAPI (data API + alert engine + background jobs)

* A lightweight UI (either Streamlit for fastest MVP or a small React/Vue front-end) that works on desktop and phone

* Scheduler (APScheduler) to poll your source links (USGS/NOAA/RFC/SNOTEL/etc.)

* Rules engine for alerts (thresholds, rate-of-change, ensemble exceedance)

* Notifications via SMS/Push (Twilio SMS, Pushover/Telegram, or web push)

* SQLite (MVP) → Postgres (later) to store pulls and alert state

* Packaging: run in Docker or as a local app; mobile use via PWA (add to home screen) so you get a phone-friendly “app” without separate native builds

*** This gives you one place on your computer (or a VPS) to see all basin conditions at once and get phone alerts.
.
.
.
.
.

**Fire Data:** https://firms.modaps.eosdis.nasa.gov/map/#d:24hrs;@0.0,0.0,3.0z
**SNOTEL Data:** https://nwcc-apps.sc.egov.usda.gov/imap/#version=170&elements=&networks=!&states=!&counties=!&hucs=&minElevation=&maxElevation=&elementSelectType=any&activeOnly=true&activeForecastPointsOnly=true&hucLabels=false&hucIdLabels=false&hucParameterLabels=true&stationLabels=&overlays=&hucOverlays=&basinOpacity=75&basinNoDataOpacity=25&basemapOpacity=100&maskOpacity=0&mode=data&openSections=dataElement,parameter,date,basin,options,elements,location,networks&controlsOpen=true&popup=&popupMulti=&popupBasin=&base=esriNgwm&displayType=station&basinType=6&dataElement=WTEQ&depth=-8&parameter=PCTMED&frequency=DAILY&duration=I&customDuration=&dayPart=E&monthPart=E&forecastPubDay=1&forecastExceedance=50&useMixedPast=true&seqColor=1&divColor=7&scaleType=D&scaleMin=&scaleMax=&referencePeriodType=POR&referenceBegin=1991&referenceEnd=2020&minimumYears=20&hucAssociations=true&relativeDate=-1&lat=46.312&lon=-116.315&zoom=8.0
**Weather Data 1:** https://www.wunderground.com/forecast/us/id/lewiston
**Weather Data 2:** https://www.nwrfc.noaa.gov/rfc/
**Weather Data 3:** https://www.nwrfc.noaa.gov/weather/10_day.cgi
DWR Forebay Elevation: https://www.nwd-wc.usace.army.mil/dd/common/dataquery/www/
DWR Qout (1 hour average): https://www.nwd-wc.usace.army.mil/dd/common/dataquery/www/

