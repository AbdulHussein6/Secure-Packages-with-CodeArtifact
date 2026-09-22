<img src="https://cdn.prod.website-files.com/677c400686e724409a5a7409/6790ad949cf622dc8dcd9fe4_nextwork-logo-leather.svg" alt="NextWork" width="300" />

# Secure Packages with CodeArtifact

**Project Link:** [View Project](https://nextwork.ai/projects/8c60d99f-e8ff-519e-a0d6-e8dfe8fa1b67)

**Author:** Abdul Hussein  
**Email:** abdulhussein@hotmail.se

---

![Image](https://nextwork.ai/authentic_blue_lucky_ferret/uploads/8c60d99f-e8ff-519e-a0d6-e8dfe8fa1b67_1d79e699)

## Introducing Today's Project!

In this project, I demonstrated how to set up AWS CodeArtifact to manage and secure Java package dependencies for an application. I've done this project to learn how CodeArtifact integrates with build tools like Maven to control, audit, and secure the third-party packages my applications rely on.

### Key tools and concepts

Services I used were AWS CodeArtifact and AWS IAM. Key concepts I learnt include how CodeArtifact acts as a proxy to Maven Central, caching dependencies for faster and more reliable builds, and how IAM policies (with STS tokens) securely authenticate Maven's access to the repository via settings.xml — removing the need to manage credentials manually.

### Project reflection

This project took me approximately 1 hours. The most challenging part was configuring the settings.xml file correctly so Maven could authenticate with CodeArtifact using the STS token.

This project is part three of a series of DevOps projects where I'm building a CI/CD pipeline! I'll be working on the next project soon — continuing to expand the pipeline with the next stage of the DevOps Challenge.

## CodeArtifact Repository

CodeArtifact is a secure, central place to store all your software packages. When you're building an application, you typically use dozens of external packages or libraries - things other developers have created that you don't want to build from scratch.

A CodeArtifact domain is like a folder that holds multiple repositories belonging to the same project or organization.

My domain is nextwork.

Upstream repositories are like backup libraries that your primary repository can access when it doesn't have what you need. If you didn't set up CodeArtifact or have an upstream repository, your build would fail because a package is missing. 

My repository's upstream repository is Maven Central.

![Image](https://nextwork.ai/authentic_blue_lucky_ferret/uploads/8c60d99f-e8ff-519e-a0d6-e8dfe8fa1b67_n4o5p6q7)

## CodeArtifact Security

### Issue

To access CodeArtifact, we need an authorization token because it uses short-lived, temporary credentials instead of permanent ones for better security. I ran into an error when retrieving a token because my EC2 instance didn't have an IAM role with the correct permissions attached yet, so it couldn't authenticate the request.

### Resolution

To resolve the error with my security token, I created a new IAM role with the CodeArtifact access policy attached, then attached that role to my EC2 instance. After re-running the export token command, it executed successfully and returned an authorization token without any errors. This resolved the error because the EC2 instance now had the proper IAM permissions to authenticate itself and request a temporary token from CodeArtifact — instead of having no credentials at all, which was the root cause of the original "Unable to locate credentials" error.

When you attach an IAM role to an EC2 instance, AWS automatically provides and rotates temporary security credentials for that instance. This means that applications running on the instance, like our Maven build, can automatically use these temporary credentials to make AWS API calls without you having to handle credential management.

## The JSON policy attached to my role

The JSON policy I set up grants AWS CodeArtifact permissions: authorization token retrieval, repository endpoint access, and package read access, plus a scoped STS bearer token limited to codeartifact.amazonaws.com.

![Image](https://nextwork.ai/authentic_blue_lucky_ferret/uploads/8c60d99f-e8ff-519e-a0d6-e8dfe8fa1b67_23rp7q8r9)

## Maven and CodeArtifact

### To test the connection between Maven and CodeArtifact, I compiled my web app using settings.xml

The settings.xml file configures Maven to check CodeArtifact for dependencies and authenticate automatically, enabling seamless, one-time-setup access to the repository.

Compiling is like translating your project's code into a language that computers can understand and run. When you compile your project, you're making sure everything is correctly set up and ready to turn into a working app.

![Image](https://nextwork.ai/authentic_blue_lucky_ferret/uploads/8c60d99f-e8ff-519e-a0d6-e8dfe8fa1b67_c17eace8)

## Verify Connection

I checked my nextwork-devops-cicd repository in CodeArtifact and saw a list of Maven packages — the dependencies from my pom.xml, which CodeArtifact fetched from Maven Central and cached during the compile.

![Image](https://nextwork.ai/authentic_blue_lucky_ferret/uploads/8c60d99f-e8ff-519e-a0d6-e8dfe8fa1b67_1d79e699)
---

*Built with [NextWork](https://nextwork.ai) - [View this project](https://nextwork.ai/projects/8c60d99f-e8ff-519e-a0d6-e8dfe8fa1b67)*
