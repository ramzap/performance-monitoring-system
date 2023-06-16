# performance-monitoring-system
**Prerequisite:**
- JDK installed preferably 17 version at least.
- Maven installed.
- Intelij installed.
- Github account.

**Setting Up infrastructure:**
1. Clone demo Gatling tests repo link to repo: https://github.com/james-willett/gatling-fundamentals-java-api.
2. Install Docker: https://www.docker.com/.
3. Install docker compose: https://docs.docker.com/compose/
4. Install Jenkins, now there is two ways how can you have it. Locally setup on your machine, installation guide: https://www.jenkins.io/download/ (you need to download war file).
Open the folder in which you have downloaded file, right click select to open terminal and run command "java -jar jenkins.war". Jenkins now should be working on http://localhost:8080 .
Other way is to setup jenkins on docker: https://octopus.com/blog/jenkins-docker-install-guide
5. Clone my repo performance-monitoring-system: https://github.com/ramzap/performance-monitoring-system
6. Open the folder that you have cloned for performance monitoring system, mouse right click and select to open terminal in this folder location.
7. Type in "docker-compose up -d" and hit enter. It should pull everything, by everything I mean that docker compose will create one service with Telegraf, Grafana, Influxdb in it and Graphite plugin enabled.
8. To verify that everything is working type in " docker ps" you should see three docker containers runing" (influxdb, telegraf, grafana).
9. Try to open grafana in your browser http://localhost:3000. It should be working now.
10. Now to have fully working solution you need to config gatling config file for enabling graphite results. Open intelij open cloned project "gatling-fundamentals-java-api".
11. Navigate src>resources>gatling.conf file uncomment these lines 106 (writers line and added to it graphite), 117-125(graphite configuration, change in it host to "localhost"). Save your changes.
12. Run gatling engine and select one of available simulations.
13. Check that in infludb you have database gatlingdb, and it is not empty. You can do this by opening folder in which you have cloned my repo, open new terminal in this folder location.
 Type in "docker ps", find influxdb container id and write down it somwhere for you. Run now this command "docker exec-it yourInfluxdbContainerId influx" (example: docker exec-it b9166d4c5bce influx).
 Hit enter. Now you have connected to database. Type in "SHOW databases". You should be able to see database gatlingdb, type in terminal "Use gatlingdb"  and after that then "SHOW entries",
 if you get something that is all you need to be able to get data to Grafana.
 14. Now to be able to configure dashboards in Grafana you need to create grafana datasource connection. Go to grafana  Data sources select InfluxDb type. 
 In field URL type in http://influxdb:8086, in Database field type in gatlingdb. In field User type in admin. Press Save&test button. It should bee green indicating that everything works as expected.
Create one more connection for influxdb telegraf information. Fill in the same information as for gatling connection just different Database name, name shoudl be influx. Press button Save&test.
15. Try to configure new dashboard in grafana. You can follow this article to this: https://www.blazemeter.com/blog/gatling-grafana-influxdb
16. Fork my repo performance-monitoring-system to your account.
17. One more thing is good to do to create new job in Jenkins that will be runing gatling tests that you have locally on your machine. https://www.educative.io/blog/performance-testing-tutorial-gatling-jenkins
Press button New Item, Select free project, add some kind of description that would be meaningfull for you. Source Code Management Select git.
And enter all needed information about repository that you have created in your account by forking my repo. 
In build steps select add Invoke top-level Maven targets, Goals should be gatling:test -D"gatling.simulationClass"="videogamedb.simulation.VideoGameDbSimulations".
Or any other simulation from your repo that you want to run. In Post build Actions. Select "Track Gatling load simulation". Save it and try to run.
