**TODO:**
- [ ] investigate server metrics monitoring for windows(now telegraf shows container metrics, not host)
- [ ] move jmeter script invocation from entrypoint script to jenkins job
- [ ] make dashboard default for user
- [ ] create job to do influxdb backups in jenkins
- [ ] modify legend labels for response time over time graphs max_avg -> avg. question- https://community.grafana.com/t/is-it-possible-to-change-legend-using-alias-by-when-series-are-selected-with-variable/4818
- [ ] distributed load testing, jenkins slaves, few backend listeners to one influxdb
- [ ] add volume to store webpagetest reports
- [ ] remove logging on job page plugin 
- [ ] add deploy key to repository
- [ ] setup jenkins job to pull changes from resository and rebuild containers
- [ ] rebuild sitespeed before job execution in jenkins

**Update readme/wiki:**
- [ ] how to use Portainer
- [ ] how to use WebPageTest private instance
- [ ] parameterizing jmeter scenarios with jemter properties
- [ ] sending jmeter test metrics to grafana(backend listener parameters)
- [ ] Load Test Monitoring grafana dashboard structure
- [ ] How to create own charts
- [ ] How to add processes to monitor in telegraf
- [ ] Add steps to change core # and RAM for linux VM on windows host
- [ ] How to remove plugins from Jenkins(remove manually from jenkins and remove from plugins file)
- [ ] How to make annotation on graph
