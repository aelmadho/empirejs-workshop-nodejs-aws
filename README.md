# Empire Conf - Deploying Node.js on AWS

As a developer you are probably already familiar with how to build and run an application on your local machine:

![small localhost](1%20-%20Development%20Environment/images/localhost.png)

But the next step is packaging your application up and running it on a server, or even a whole fleet of servers, and managing this can be challenging:

![large deployment](1%20-%20Development%20Environment/images/deployment.png)

This workshop will help you go from localhost to deployed on AWS three times, using three different services:

- [AWS Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/)
- [AWS Lambda](https://aws.amazon.com/lambda/) + [Serverless](https://serverless.com/)
- [EC2 Container Service](https://aws.amazon.com/ecs/)

For this workshop the sample code is written in Node.js but the same Amazon services can be applied to other runtime languages. The sample app is a simple REST API for an Adventure Time fan website. The API provides endpoints for consuming structured data about Adventure Time characters and locations.

You can view an example of the raw data [here](2%20-%20Elastic%20Beanstalk/code/db.json)

The external HTTP interface of the API has a basic spec:

- `GET /api/` - A simple welcome message
- `GET /api/characters` - A list of all characters
- `GET /api/characters/:id` - Fetch a specific character by ID
- `GET /api/locations` - A list of all locations
- `GET /api/locations/:id` - Fetch a specific location by ID
- `GET /api/characters/by-location/:locationId` - Fetch all characters at a specific location
- `GET /api/characters/by-gender/:gender` - Fetch all characters of specified gender
- `GET /api/characters/by-species/:species` - Fetch all characters of specified species
- `GET /api/characters/by-occupation/:occupation` - Fetch all characters that have specified occupation

&nbsp;

## Instructions

1. [Create a development machine](1%20-%20Development%20Environment/) to use for the rest of workshop
2. [Deploy API on Elastic Beanstalk](2%20-%20Elastic%20Beanstalk/)
3. [Deploy API on AWS Lambda](3%20-%20Serverless%20Lambda/)
4. [Deploy API on EC2 Container Service](4%20-%20EC2%20Container%20Service/)

&nbsp;

## Architecture Diagrams

### [Elastic Beanstalk](2%20-%20Elastic%20Beanstalk/)

A monolithic application behind a load balancer. All API route handling is done within the server process. A cluster of Node.js processes sharing a single port is used in order to fully utilize all cores of a multicore EC2 Instance:

![elastic beanstalk](1%20-%20Development%20Environment/images/elastic-beanstalk-architecture.png)

### [EC2 Container Service](4%20-%20EC2%20Container%20Service/)

Microservices run in docker containers behind an application load balancer. The API is split into a `characters` and `locations` service, and the application load balancer routes the appropriate API traffic to the approriate docker containers. Each docker container has a single process. Container ports are linked to randomly assigned external ports on the host instance:

![ec2 container service](1%20-%20Development%20Environment/images/ecs-architecture.png)

### [Lambda](3%20-%20Serverless%20Lambda/)

Nanoservices. Each API endpoint is served by its own AWS Lambda function. An API gateway sits in front of all the Lambdas and routes requests to each function:

![lambda](1%20-%20Development%20Environment/images/lambda-architecture.png)
