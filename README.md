# test-deploy
This project tests deployment of documentation built with sphinx inside a docker container to an AWS s3 bucket using Travis-CI. 

The `docs` directory contains sample doc files for testing.
The `scripts` directory contains:
 
- [docker-docs.sh](/scripts/docker-docs.sh) -- runs a docker container with the `f5devcentral/containthedocs:latest` image and mounts the working directory to the container. 
- [test-docs.sh](/scripts/test-docs.sh) -- runs `sphinx-build`, `make linkcheck`, and `write-good` to test the documentation.
- [deploy-docs.sh](/scripts/deploy-docs.sh) -- runs a docker container with the `f5devcentral/containthedocs:latest` image; mounts the working directory to the container and imports the secure AWS login environment variables from the Travis CI build environment.

The deploy-docs.sh script replaces travis-ci's built-in s3 deploy functionality. 
This allows us to use the deploy scripts built into the f5devcentral/containthedocs container image.
 
Travis CI uses the [aws-sdk gem](https://github.com/aws/aws-sdk-ruby) to deploy objects to the s3 bucket using API calls. 

The `containthedocs` deployment script uses the `aws s3 sync` CLI command to sync the contents of the `docs/_build` directory (generated by the sphinx build) with the specified path in the destination bucket.  
 