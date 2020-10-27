frontend load test with sitespeed.io + webpagetest
backend load testing with Apache Jmeter 


Grafana: data visualization & monitoring
Influxdb: time series DB platform for metrics & events(Time Series Data)
Telegraf: server agent for collecting & reporting metrics
Sitespeed.io: set of tools for frontend load testing
Graphite: time series DB platform for metrics
Jenkins: continuous integration server for tests execution
Portainer: service for managing docker environment
Webpagetest: private instance of webpagetest server for frontend tests execution
Apache Jmeter: tool for backend load testing


docker-compose -f docker-compose.yml pull
docker-compose -f docker-compose.yml build
docker-compose -f docker-compose.yml down

docker-compose -f docker-compose.yml up -d


Services endpoints
jenkins localhost:8181
grafana localhost:8857
portainer localhost:9000
webpagetest server localhost:80
influxdb localhost:8653


jenkins consists of 2 jobs:

BackendJob: run Jmeter scenarios
FrontendJob: run tests with sitespeed.io and webpagetest private instance



Test runs comparison
Tests comparison is done using scripted js dashboard. It could be accessed at http://127.0.0.1:8857/dashboard/script/compare_tests.js



FrontendJob
To run frontend test: Open FrontendJob -> Build with Parameters -> Set build parameters -> Build 

This job will start sitespeed.io docker container and run test with parameters using WebPageTest private instance

Frontend test deliverables:

sitespeed.io HTML report 
webpagetest HTML report 