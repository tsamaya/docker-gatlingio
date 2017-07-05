# docker-gatlingio

[Gatling](https://gatling.io) is Open-Source Load & Performance Testing Tool For Web Applications

This Docker file is based on Vazhenin Denis' [Dockerfile](https://hub.docker.com/r/denvazh/gatling/) adding hints from Grig Gheorghiu's [post](http://agiletesting.blogspot.co.uk/2016/06/running-gatling-load-tests-in-docker.html) and using [openjdk:alpine](https://hub.docker.com/_/openjdk/) image to reduce container size.

## Usage

### Run container

Run gatling Basic and Advanced samples in interactive mode:

  `$ docker run -it --rm tsamaya/docker-gatlingio`

### Make you own container

We will create files and folders as below:
```
.
├── Dockerfile
├── conf
├── results
└── user-files
    ├── data
    └── simulations
        ├── SampleSimulation.scala
```

  * Prepare folder structure

    `$ cd your/work/folder`

    `$ echo "FROM tsamaya/docker-gatlingio:latest" > Dockerfile`

    `$ mkdir -p conf results user-files/simulations user-files/data`

    `$ docker build -t gatling:local .`

  * Create you Simulation file in `user-files/simulations`. Check the [Sample](#Sample simulation) below.

  * Now you can run your simulation in a limited interactive with :

    `$ docker run -it --rm -v ``pwd``/conf:/opt/gatling/conf -v ``pwd``/user-files:/opt/gatling/user-files -v ``pwd``/results:/opt/gatling/results -e JAVA_OPTS=$JAVA_OPTS gatling:local`

  or automatically:

    `$ docker run -it --rm -v ``pwd``/conf:/opt/gatling/conf -v ``pwd``/user-files:/opt/gatling/user-files -v ``pwd``/results:/opt/gatling/results -e JAVA_OPTS=$JAVA_OPTS gatling:local -s SampleSimulation`

  assuming here taht your scala file is `SampleSimulation.scala`

  Note: you can define `$JAVA_OPTS` for example `$ export JAVA_OPTS="-Dusers=3"`

## Samples

### Dockerfile
```Dockerfile
FROM tsamaya/docker-gatlingio:latest
```

### Sample simulation

This file is coming from gatling.io quick start guide

```scala
import io.gatling.core.Predef._
import io.gatling.http.Predef._
import scala.concurrent.duration._

class SampleSimulation extends Simulation {

  val httpConf = http
    .baseURL("http://computer-database.gatling.io")
    .acceptHeader("text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8")
    .doNotTrackHeader("1")
    .acceptLanguageHeader("en-US,en;q=0.5")
    .acceptEncodingHeader("gzip, deflate")
    .userAgentHeader("Mozilla/5.0 (Windows NT 5.1; rv:31.0) Gecko/20100101 Firefox/31.0")

  val scn = scenario("SampleSimulation")
    .exec(http("request_root")
    .get("/"))
    .pause(5)

  val userCount = Integer.getInteger("users", 1)

  setUp(
    scn.inject(atOnceUsers(userCount))
  ).protocols(httpConf)
}
```

## Credit

I want to thank:
- Denis Vazhenin [@denvazh](https://github.com/denvazh)
- Grig Gheorghiu [blogger profile](https://www.blogger.com/profile/17863511617654196370)
for their works that lead to this repo.

## Note

This is my very first Dockerfile publish on [Docker Hub](https://hub.docker.com/r/tsamaya/docker-gatlingio/) built from Github, so I will be grateful for pointing out my mistakes, as well as for advices of any kind. Thanks!

## License

MIT
