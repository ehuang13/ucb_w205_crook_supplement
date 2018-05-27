# Under Construction

# UCB MIDS W205 Summer 2018 - Kevin Crook's agenda for Synchronous Session #4

## Start updated docker images in your droplet (not in docker container)

We are going to start running clusters of docker containers in your droplet. Some of the images we will be running are very large and require several minutes to bring down.  To help save class time, please update these images prior to class start.  Also, we many find issues with the images and need to push new images with fixes.  So, updating images before class and before you do any meaningful work on assignments is always a good idea.

Run these command in your droplet (but **NOT** in a docker container):

docker pull midsw205/base
docker pull confluentinc/cp-zookeeper:latest
docker pull confluentinc/cp-kafka:latest

## Update the course-content repo in your docker container in your droplet

See instructions in previous synchronous sessions.

# Review the Query Project

We will spend a few minutes reviewing the Query Project now that everyone has been actively working on it and is more familiar with the dataset.

## Breakout - group discussions about the Query Project

We will go into breakout so everyone can discuss the Query Project. The following is a list of suggested topics to discuss, but this is open ended, you group can discuss anything they feel will help with the project.  Time permitting, we will repeat this in following weeks. 

* Discuss the 3 tables in the dataset, the data in each, and what type of queries we could find out from each table
* What is a trip?
* What is a commuter trip?
* For each of the following pricing options, how would you design a query to detect the customer base?
  * a flat price for a single one-way trip
  * a day pass that allows unlimited 30-minute rides for 24 hours
  * an annual membership
* What kinds of analytical questions could we ask to help us analyze the customer base, if they are worth targeting, and how to target them?
* Does the fixed station model with docks help us with analytics?  Does it hurt us with the customer base?

# Revisit docker concepts in single container mode

We will revisit docker concepts in a single container mode.  This will prepare us to understand docker in a cluster mode.

Run our regular docker container that we have been using to clone and update our course-content repo and been using to work on our assignments.  
* -it
** i and t are separate 

```
docker run -it --rm -v ~/w205:/w205 midsw205/base bash
```

## What containers are running right now?

- New terminal window

- `docker ps`

## What containers exist?

- `docker ps -a`

## Container name

- `fervent_austin` is my running `midsw205/base:latest` container

## What images do I have?

- Images vs. containers

- `docker images`

## Image name 

- Need both repository & tag

- e.g., `midsw205/base:latest`




## Clean up containers

`docker rm -f <name-of-container>`

::: notes
:::

#
## Idiomatic docker

- start a `midsw205/base:latest` container
- run `pwd`
- exit container

-vs-

```
docker run -it --rm -v ~/w205:/w205 midsw205/base pwd
```
 



::: notes
I hope some of this simplifies when we start using the containers to _just_ run a command... i.e.,
`docker run [<opts>] <image> [<command>]`
... e.g., 
ME: check this query for backticks from bq cli sql
`docker run -it --rm midsw205/base bq query --use_legacy_sql=false 'select count(*) from mytable'`
in one go (edited)

then they're only "in" one place
:::

#
## Docker compose

- What is docker compose?

## Update your course content repo in w205

```
cd ~/w205/course-content
git pull --all
```

## Docker compose .yml file

- `cd w205`
- `mkdir kafka`
- save `docker-compose.yml` from recently pulled `~/w205/course-content` to
  recently created `~/w205/kafka` directory


::: notes

Save the following snippet as `~/w205/kafka/docker-compose.yml` on your host
filesystem

    ---
    version: '2'
    services:
      zookeeper:
        image: confluentinc/cp-zookeeper:latest
        network_mode: host
        environment:
          ZOOKEEPER_CLIENT_PORT: 32181
          ZOOKEEPER_TICK_TIME: 2000
        extra_hosts:
          - "moby:127.0.0.1"

      kafka:
        image: confluentinc/cp-kafka:latest
        network_mode: host
        depends_on:
          - zookeeper
        environment:
          KAFKA_BROKER_ID: 1
          KAFKA_ZOOKEEPER_CONNECT: localhost:32181
          KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:29092
          KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR: 1
        extra_hosts:
          - "moby:127.0.0.1"

:::




## Docker compose spin things up

- `cd ~/w205/kafka`
- `docker-compose up -d`
- `docker-compose ps`

::: notes
- This is the start of spinning up things that will lead to projects 2&3
- Have them go through on command line, talk about what is happening.
:::

## Clean up

`docker-compose down`

- Can check with:
- `docker-compose ps`


#
## Summary
- git branching
- where are we with Docker?
- Idiomatic Docker
- docker-compose

##

![](images/pipeline-overall.svg)

::: notes
docker-compose is for this
:::


#

## 


::: notes
md works here
:::

# 

## Extras




#

<img class="logo" src="images/berkeley-school-of-information-logo.png"/>

