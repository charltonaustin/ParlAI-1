# CI For Open Source Project

## 1 - Create Bitbucket YML for ParlAI

See [bitbucket-pipelines.yml](bitbucket-pipeline.yml)

This doesn't have all the testing and everything, this is only recreating the "Test installation instructions job flow"  
There is a much larger version of this pipeline that would take more than 1-2 hours to create to have full test coverage  
and accurately recreate the multi-job/stage circle-ci version. The docs on bitbucket pipeline do not seem to support  
multi-job/stage workflows, so would require more time to streamline this with multiple bitbucket-pipelines.yml files to support this  
https://support.atlassian.com/bitbucket-cloud/docs/configure-bitbucket-pipelinesyml/  

## 2 - Containerize the app with Docker
Build Docker Container 
```bash
# Rename Docker File
mv Dockerfile.Helm Dockerfile
# Build container
docker build -t parlai:latest .
# Run Container
docker run --rm -it  parlai parlai            
usage: parlai [--help] [--helpall] [--version] COMMAND ...

       _
      /")
     //)
  ==//'=== ParlAI
   /

optional arguments:
  --help, --h                show this help message and exit
  --helpall                  List all commands, including advanced ones.
  --version                  Prints version info and exit.

Commands:

  eval_model (em, eval)      Evaluate a model
  display_data (dd)          Display data from a task
  display_model (dm)         Display model predictions.
  tod_world_script           World for chatting with the TOD conversation structure
  train_model (tm, train)    Train a model
  generate_model_card (gmc)  Evaluate a model
  interactive (i)            Interactive chat with a model on the command line
  safe_interactive           Like interactive, but adds a safety filter
  self_chat                  Generate self-chats of a model
# Run With Options
docker run --rm -it  parlai parlai --version
ParlAI version 1.5.1
```

## Tom's Thoughts

For both major pieces of this assignment I can tell there are more things I should be doing, especially comparing the full circleci build manifest with what I've provided on the bitbucket example, but both lack of time and features and experience with bitbucket lead me to providing a less than ideal version. In practice I would take more time to develop the bitbucket pipeline to support the CI testing that is absolutely critical to evaluating builds. For Docker I would have preferred to have more time to develop a multi-stage Dockerfile, the image when installing all requirements and doing a "source" build is 3.85GB, which is pretty big for a container, everytime we would launch that task be it EKS or Fargate, it would have to wait and download a 4GB object, that's going to increase the network bill by quite a lot potentially. So if I had more time, I would look for more ways to cut the image size by dropping the source files after the build and just using the python package that is produced by `python setup.py develop`.   

Thank you, this was fun. Tom Stewart
