# Bryong your own OpenQAOA container to Amazon Braket Hybrid Jobs

If you are wondering what is an Amazon Braket Hybrid Jobs, check out Braket's very answer

> With Hybrid Jobs, you can run algorithms that use both classical and quantum compute resources. These hybrid quantum-classical algorithms can help you optimize the performance of the quantum devices currently available. These algorithms are designed to maximize the benefits and reduce the drawbacks of both types of computational systems and are generally used to find the ground state or global minimum of a particular system.

QAOA is a great example of such hybrid quantum-classical algorithms, and we here provide a simple procedure to prepare your own OpenQAOA container.

## Steps to create your own OpenQAOA container

First of all, these instructions are based on Amazon Braket's documentation. Therefore, a safe place to check if you are an experienced Docker and AWS practitioner is to head to the [Bring your own container](https://docs.aws.amazon.com/braket/latest/developerguide/braket-jobs-byoc.html) page!

The following instructions will allow you to create your OpenQAOA container. Note that they will work out of the box only for Linux and macOs.
 
 1. Make sure all the dependencies are installed and credentials are properly set:
	 - Clone openqaoa from github `git clone https://github.com/entropicalabs/openqaoa.git`
	 - Install docker on your system
	 - Install the [AWS command line interface](https://aws.amazon.com/cli/)
	 - Create an Amazon Elastic Container Registry (Amazon ECR) repository (see step 3 [here](https://docs.aws.amazon.com/braket/latest/developerguide/braket-jobs-byoc.html))
 2. Authenticate with AWS: to build the repository we first authenticate with AWS. We need to authenticate twice because we first need to download the Braket docker image which will be used as a base for building the OpenQAOA image, and a second time to upload the OpenQAOA image to your own ECR.
	 - Authenticate through the AWS command line interface: 
		 - `docker login -u AWS -p $(aws ecr get-login-password --region ${REGION}) 292282985366.dkr.ecr.${REGION}.amazonaws.com`, this command is needed to download the base-
		 - `docker login -u AWS -p $(aws ecr get-login-password --region ${REGION}) ${ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com`
		 - Note that you will need to re-do this procedure for any `${REGION}` of interest, and you will also need to insert your own `$ACCOUNT_ID`
3. Build the image, and push it to the repository: 
	- Go to the OpenQAOA root folder and type `docker build -t ${REPOSITORY_NAME}`, where you can choose the tag you wish
	- Then create an image tag (alias) with the full repository path for the ECR`docker tag ${REPOSITORY_NAME}:latest ${ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/${REPOSITORY_NAME}:latest`
	- And finally push the image to: `${ACCOUNT_ID}.dkr.ecr.${REGION}.amazonaws.com/${REPOSITORY_NAME}:latest]` where all the names have been changed accordingly
