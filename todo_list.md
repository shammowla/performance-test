**TODO:**

docker exec -it jenkins bash

/var/jenkins_home/workspace/FrontendJob
------

graphite_network=$(docker inspect --format='{{.HostConfig.NetworkMode}}' graphite)

run_sitespeed="docker run --name sitespeed --volumes-from=jenkins --network=host --privileged=true --shm-size=1g --rm -v \"/tmp/sitespeed.io/\":/sitespeed.io \sitespeedio/sitespeed.io \
--graphite.host localhost --graphite.port 2003 --graphite.httpPort 8080 --browsertime.browser $browser \--browsertime.iterations $run_number \
$additional_args $URL"

#remove '|| true' when https://github.com/sitespeedio/sitespeed.io/issues/1897 will be resolved

run_sitespeed_with_wpt_script="docker run --name sitespeed --volumes-from=jenkins --network=host --privileged=true --shm-size=1g --rm -v \"/tmp/sitespeed.io/\":/sitespeed.io \sitespeedio/sitespeed.io \
--graphite.host localhost --graphite.port 2003 --graphite.httpPort 8080 --browsertime.browser $browser \--browsertime.iterations $run_number \
$additional_args --webpagetest.file /var/lib/wpt-scripts/$script $URL || true"

if ! $execute_script
then
	eval $run_sitespeed
else
	eval $run_sitespeed_with_wpt_script
fi

#--webpagetest.file $WORKSPACE/script1 
report_dir_name=$(echo $URL | awk -F/ '{print $3}')
sudo rm -rf frontend_report && sudo mkdir "$WORKSPACE/frontend_report"

cd "/var/lib/sitespeed.io/sitespeed-result/$report_dir_name/"
cp -r $(ls -Art | tail -n 1)/* "$WORKSPACE/frontend_report"

------

docker run --name sitespeed --volumes-from=jenkins --network=host --privileged=true --shm-size=1g --rm -v \"/tmp/sitespeed.io/\":/sitespeed.io \sitespeedio/sitespeed.io --graphite.host localhost --graphite.port 2003 --graphite.httpPort 8080 --browsertime.browser chrome --browsertime.iterations 1 https://alloyio.com


HTML files
/control.html
/analyticsTargetAudienceManager/legacy/launch.html
/analyticsTargetAudienceManager/legacy/standalone.html
/analyticsTargetAudienceManager/alloy/launch.html
/analyticsTargetAudienceManager/alloy/standalone.html
/analytics/legacy/launch.html
/analytics/legacy/standalone.html
/analytics/alloy/launch.html
/analytics/alloy/standalone.html
JS files
/js/alloy.min.js
/js/AppMeasurement.js 
/js/at.js
/js/dil.js
/js/visitorapi.min.js
/analyticsTargetAudienceManager/legacy/js/launch/123456/abcdef...
/analyticsTargetAudienceManager/alloy/js/launch/123456/abcdef...
/analytics/legacy/js/launch/123456/abcdef...
/analytics/alloy/js/launch/123456/abcdef...

It would also be awesome if the job that ran the benchmarks could automatically pull down all the latest JS files instead of having to commit them to the repo. If we want to track performance over time (as we publish new releases), I think that’s the only feasible way to do it. What do you think?

Cron Job
(Notify email and slack. Maybe even do something more dramatic like open a war room.)
* Run prod alloy against prod Konductor.
Commit to Alloy
(Don’t notify anything. It’s just noise, since no commit will make it to the next stage without an engineer looking and seeing whether tests are passing.)
* Run commit’s functional tests against commit’s Alloy build + Konductor prod.
* Run commit’s functional tests against commit’s Alloy build + Konductor int.
Commit to Konductor
(Don’t notify anything. It’s just noise, since no commit will make it to the next stage without an engineer looking and seeing whether tests are passing.)
* Run commit’s Konductor code against Alloy’s alloy-latest-release branch.
* Run commit’s Konductor code against Alloy’s master branch.
10:48
I talked to Constantin and it sounds like he already has the Cron Job and Commit to Konductor ones set up.




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
