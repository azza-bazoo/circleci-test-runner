## An image for CircleCI 2.0 tests, featuring `docker-compose`, `tap-xunit`, and `awscli`

I have my tests set up to start two Docker containers: one for my app, and a second System Under Test container that makes HTTP requests against the app container. The two of these are configured in a `docker-compose.test.yml` file. Then if tests are successful, I push to [Amazon ECR](https://aws.amazon.com/ecr/) and deploy (“update the task definition”) in [Amazon EC2 Container Service](https://aws.amazon.com/ecs/).

This is similar to how [Docker Cloud does automated testing](https://docs.docker.com/docker-cloud/builds/automated-testing/), except that I deploy to ECS instead of Docker Cloud.

Meanwhile, [CircleCI 2.0](https://circleci.com/docs/2.0/) is cool, and runs things with Docker itself, but has a [complex process](https://circleci.com/docs/2.0/building-docker-images/) if you want to build and push Docker images for deployment. Also, it expects [jUnit-style XML output](https://circleci.com/docs/1.0/test-metadata/).

So this Dockerfile installs:

* the main docker CLI client and [docker-compose](https://docs.docker.com/compose/)
* nodejs and the [tap-xunit](https://github.com/aghassemi/tap-xunit) converter
* the [ecs-deploy](https://github.com/silinternational/ecs-deploy) helper script, with its dependencies [awscli](https://aws.amazon.com/cli/) and [jq](https://stedolan.github.io/jq/)
