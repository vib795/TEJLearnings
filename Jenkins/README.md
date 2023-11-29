# Jenkins Core Learning Guide

## 1. Introduction to Jenkins

Jenkins is an open-source automation server that facilitates continuous integration and continuous delivery (CI/CD). It is widely used for building, testing, and deploying software.

### Example: Install Jenkins

```bash
# Install Jenkins on Ubuntu
sudo apt-get update
sudo apt-get install jenkins
```

## 2. Installation and Setup

Install Jenkins and configure basic settings.

### Example: Run Jenkins using Docker

```bash
docker run -p 8080:8080 -p 50000:50000 jenkins/jenkins
```

## 3. Jobs and Builds

Create and configure Jenkins jobs for building applications.

### Example: Create a Freestyle Project

A Freestyle Project is one of the simplest project types in Jenkins. It allows you to configure build steps and post-build actions in a graphical user interface.

1. **Create a New Freestyle Project:**
   - In the Jenkins dashboard, click on "New Item."
   - Enter a name for your project and select "Freestyle project."
   - Click "OK" to create the project.

2. **Configure Build Steps:**
   - Scroll down to the "Build" section and click on "Add build step."
   - Choose the appropriate build step based on your project requirements (e.g., execute shell commands, Windows batch command, etc.).

3. **Configure Post-Build Actions:**
   - Scroll down to the "Post-build Actions" section and click on "Add post-build action."
   - Configure actions such as archiving artifacts, triggering other projects, or sending email notifications.

4. **Save and Run the Project:**
   - Click "Save" to save your project configuration.
   - Click "Build Now" to manually trigger a build and observe the build console output.

This example demonstrates the basic setup of a Freestyle Project in Jenkins. Freestyle Projects are suitable for simple build and deployment scenarios, but for more complex workflows, consider exploring Jenkins Pipeline as Code.

## 4. Version Control Integration

Integrate Jenkins with version control systems, such as Git.

### Example: Configure Jenkins to Pull from Git

Jenkins can be configured to automatically pull source code from a Git repository. This integration ensures that your build jobs use the latest code from the repository.

1. **Install Git Plugin:**
   - In the Jenkins dashboard, navigate to "Manage Jenkins" > "Manage Plugins."
   - Install the "Git Plugin" from the available plugins.

2. **Configure Source Code Management in Jenkins Job:**
   - In your Jenkins job configuration, under the "Source Code Management" section, select "Git."
   - Enter the repository URL and configure credentials if the repository requires authentication.

3. **Specify Branches to Build:**
   - Specify the branches you want Jenkins to build. You can build all branches or specify branches using wildcards.

4. **Additional Git Options:**
   - Configure additional options such as submodules, clean workspace before each build, and advanced Git behaviors.

5. **Save and Run the Project:**
   - Click "Save" to save your project configuration.
   - Trigger a build to see Jenkins automatically pull the latest code from the Git repository.

This example demonstrates the basic configuration for integrating Jenkins with a Git repository. Adjust the settings based on your Git repository structure and version control needs.

## 5. Plugins

Explore and use Jenkins plugins to extend functionality.

### Example: Install and Configure Amazon EC2 Plugin

Jenkins plugins enhance its functionality and allow integration with various services. The Amazon EC2 Plugin is commonly used to dynamically provision build agents on Amazon EC2 instances.

1. **Install Amazon EC2 Plugin:**
   - In the Jenkins dashboard, navigate to "Manage Jenkins" > "Manage Plugins."
   - Install the "Amazon EC2 Plugin" from the available plugins.

2. **Configure EC2 Cloud in Jenkins:**
   - In Jenkins, go to "Manage Jenkins" > "Configure System."
   - Scroll down to the "Cloud" section and add a new cloud of type "Amazon EC2."

3. **Configure EC2 Instance Templates:**
   - Add one or more EC2 instance templates, specifying the AMI, instance type, and other configurations.

4. **Job Configuration to Use EC2 Agents:**
   - In your Jenkins job configuration, under the "Restrict where this project can be run" section, choose the label associated with your EC2 instance template.

5. **Save and Run the Project:**
   - Click "Save" to save your project configuration.
   - Trigger a build, and Jenkins will dynamically provision an EC2 instance to run the build job.

This example demonstrates installing and configuring the Amazon EC2 Plugin to enable Jenkins to dynamically scale build agents on Amazon EC2 instances. Explore other plugins based on your integration needs.


## 6. Pipeline as Code (Jenkinsfile)

Learn how to define and use Jenkins pipelines as code.

### Example: Basic Jenkinsfile

[Jenkins Pipeline](https://www.jenkins.io/doc/book/pipeline/) allows you to define your build pipeline as code, which can be version-controlled along with your application source code.

1. **Create a Jenkinsfile:**
   - In your project repository, create a file named `Jenkinsfile`.
   - Define the stages and steps of your build pipeline using Groovy syntax.

   ```groovy
   pipeline {
       agent any

       stages {
           stage('Build') {
               steps {
                   echo 'Building...'
               }
           }
           stage('Test') {
               steps {
                   echo 'Testing...'
               }
           }
           stage('Deploy') {
               steps {
                   echo 'Deploying...'
               }
           }
       }
   }
2. **Configure Jenkins Job to Use Jenkinsfile:**
    - In your Jenkins job configuration, under "Pipeline," select "Pipeline script from SCM" as the Definition.
    - Specify the repository URL and Jenkinsfile path.

3. **Run the Pipeline:**
    - Save your job configuration and trigger a build.
    - Jenkins will read the Jenkinsfile from the repository and execute the defined pipeline.

This example demonstrates a basic Jenkinsfile with three stages: Build, Test, and Deploy. Jenkinsfiles enable version-controlled, reproducible, and shareable pipeline definitions.

## 7. Security and Authentication

Implement security measures and user authentication in Jenkins.

### Example: Configure Jenkins Security

[Jenkins Security Documentation](https://www.jenkins.io/doc/book/managing/security/)

Security in Jenkins is crucial to control access and protect sensitive information.

1. **Configure Global Security:**
   - In the Jenkins dashboard, navigate to "Manage Jenkins" > "Configure Global Security."
   - Check the box for "Enable Security" to activate security settings.

2. **Manage Users and Passwords:**
   - Create user accounts for individuals who need access.
   - Configure passwords or integrate with external authentication systems.

3. **Access Control:**
   - Specify who can do what by configuring matrix-based security or using role-based access control (RBAC).

4. **Configure Project-based Matrix Authorization Strategy:**
   - For fine-grained access control, configure matrix-based security within individual project settings.

5. **Secure Jenkins Agents:**
   - If using Jenkins agents, ensure they are configured securely. Limit access to the Jenkins controller and agents.

6. **Audit Logging:**
   - Enable and configure audit logging to track changes and monitor security-related events.

7. **Save and Apply Changes:**
   - Save your security configuration changes and apply them.

Security in Jenkins is an ongoing process. Regularly review and update your security configurations based on organizational requirements and changes.

This example provides a basic overview of configuring security in Jenkins. Refer to the official documentation for more in-depth security practices.

## 8. Monitoring and Reporting

Set up monitoring for Jenkins builds and generate reports.

### Example: Integrate with Prometheus and Grafana

[Jenkins Prometheus Plugin](https://plugins.jenkins.io/prometheus/)

Monitoring Jenkins builds and generating reports provides insights into the health and performance of your CI/CD pipeline.

1. **Install Prometheus Plugin:**
   - In the Jenkins dashboard, navigate to "Manage Jenkins" > "Manage Plugins."
   - Install the "Prometheus Metrics Plugin" from the available plugins.

2. **Configure Prometheus Metrics:**
   - In Jenkins, go to "Manage Jenkins" > "Configure System."
   - Scroll down to the "Metrics" section and configure Prometheus metrics settings.

3. **Set Up Grafana Dashboard:**
   - Install Grafana and set up a dashboard to visualize Jenkins metrics.
   - Import a Jenkins dashboard template from Grafana's dashboard repository.

4. **Run Jenkins Builds:**
   - Trigger Jenkins builds and monitor the metrics in Grafana.

5. **Additional Monitoring Tools:**
   - Explore additional monitoring tools and plugins, such as Jenkins Health Advisor and Logstash, to enhance visibility.

6. **Customize Reports:**
   - Customize reports based on your organization's key performance indicators (KPIs) and metrics.

7. **Save and Analyze Data:**
   - Save the configuration and analyze monitoring data to identify trends and potential issues.

Monitoring and reporting help optimize Jenkins performance and ensure the reliability of your CI/CD pipeline.

This example provides a basic setup for integrating Jenkins with Prometheus and Grafana. Adjust configurations based on your monitoring and reporting requirements.
