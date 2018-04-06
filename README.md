# PFE-Cynapsys
## Step 1

### Website
In the [website directory](website/) there are four files:

1. **[index.html](website/index.html):** This is the index file that our S3 bucket will be displaying.
2. **[app.js](website/app.js):** The heart of our website. We will be making some changes to this in a bit, but for now we can leave it alone.
3. **[squirrel.png](website/squirrel.png):** Our friend! SAM the squirrel.
4. **[website.yaml](website/website.yaml):** The CloudFormation template used to create the Amazon S3 bucket for the website.

To create the website stack.

[<img src="https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png">](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=myteststack&templateURL=https://awscomputeblogimages.s3-us-west-2.amazonaws.com/CSIM-website.yaml)

Once the stack is complete, we will need to keep track of the S3 bucket name and the URL for the website. 

Now we have all the seperate parts of our website, but lets get SAM up and running:

```bash
sh upload_website.sh <s3-bucket-name>
```

And visit the url you saved before. You should see something like this:

![SAM Screenshot](/img/sam-screenshot.png)

Ta-da, a working website! SAM the squirrel may be all alone right now, but we'll fix that in a bit.


## Step 2
### API
The Serverless API we are building! The [api directory](api/) contains five files. 

1. **[beta.json](api/beta.json):** The CloudFormation staging file. This will be used by CloudFormation to pass parameters to our CloudFormation template.
2. **[buildspec.yml](api/buildspec.yml):** This is used by CodeBuild in the build step of our pipeline. We will get to that later.
3. **[index.js](api/index.js):** The Lambda function code!
4. **[package.json](api/package.json):** The package.json that defines what packages we need for our Lambda function.
5. **[saml.yaml](api/saml.yaml):** SAML my YAML! This is the SAM template file that will be used to create our API gateway resource and Lambda function, hook them up together

Create a new github repo from the [api directory](api/) and place these files in there. This repo will be used for your automated CI/CD pipeline we build below.

## Step 3

### Pipeline
The pipeline is a full CI/CD serverless pipeline for building and deploying your api. In this example we are using CloudFormation to create the pipeline, all resources, and any permissions needed.

The following resources are created:

- An S3 bucket to store deployment artifacts.
- An AWS CodeBuild stage to build any changes checked into the repo.
- The AWS CodePipeline that will watch for changes on your repo, and push these changes through to build and deployment steps.
- All IAM roles and policies required.

The CloudFormation templates being used to create these resources can be found in [pipeline directory](pipeline/).

To create the pipeline stack, click the launch stack button below.

[<img src="https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png">](https://console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/new?stackName=myteststack&templateURL=https://awscomputeblogimages.s3-us-west-2.amazonaws.com/CSIM-main.yaml)
