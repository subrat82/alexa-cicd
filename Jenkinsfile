pipeline {
  agent any
  stages {
    stage('Initialize') {
      steps {
        echo 'Starting the pipeline'
        sh 'mvn clean'
      }
    }
    stage('Build') {
      steps {
        sh 'mvn -Dmaven.test.failure.ignore=true install'
      }
    }
    stage('Test') {
      steps {
        sh 'mvn -Dtest=AlexaControllerTest test'
      }
    }
    stage('Deploy') {
      steps {
        archiveArtifacts 'target/*.war'
        sh 'aws --version'
        sh 'aws s3 ls'
        //elasticbeanstalk-us-west-2-jenkins
        //sh '''aws --debug s3 cp /var/lib/jenkins/workspace/alexa-cicd_master/target/alexa-cicd-0.0.1-SNAPSHOT.war s3://elasticbeanstalk-us-east-1-593614531934/2018362ew4-alexa-cicd-0.0.1-SNAPSHOT.war'''
        //sh 'aws --debug elasticbeanstalk create-application-version --application-name alexacicd --version-label "alexacicd-jenkins$BUILD_DISPLAY_NAME" --description "Created by $BUILD_TAG"  --source-bundle=S3Bucket=elasticbeanstalk-us-east-1-593614531934,S3Key=2018362ew4-alexa-cicd-0.0.1-SNAPSHOT.war'
        //sh 'aws elasticbeanstalk update-environment --environment-name=Alexacicd-env --version-label "alexacicd-jenkins$BUILD_DISPLAY_NAME"'
        
        sh '''aws --debug s3 cp /var/lib/jenkins/workspace/alexa-cicd_master/target/alexa-cicd-0.0.1-SNAPSHOT.war s3://elasticbeanstalk-us-west-2-jenkins/alexa-cicd-0.0.1-SNAPSHOT.war '''
        sh 'aws --debug elasticbeanstalk create-application-version --application-name alexacicd --version-label "alexacicd-jenkins$BUILD_DISPLAY_NAME" --description "Created by $BUILD_TAG"  --source-bundle S3Bucket="elasticbeanstalk-us-west-2-jenkins",S3Key="alexa-cicd-0.0.1-SNAPSHOT.war" '
        //sh 'aws elasticbeanstalk create-environment --environment-name=Alexacicd-env --version-label "alexacicd-jenkins$BUILD_DISPLAY_NAME"'
        //sh 'aws elasticbeanstalk create-environment --application-name alexacicd --environment-name Alexacicd-env --version-label "alexacicd-jenkins$BUILD_DISPLAY_NAME" --solution-stack-name "64bit Amazon Linux 2018.03 v2.9.20 running Python 3.6" --option-settings.member.1.Namespace="aws:autoscaling:launchconfiguration" --option-settings.member.1.OptionName="IamInstanceProfile" --option-settings.member.1.Value="aws-elasticbeanstalk-ec2-role" '
        sh 'ls'
        sh 'pwd'
        sh 'aws elasticbeanstalk update-environment --application-name alexacicd --environment-name Alexacicd-env --version-label "alexacicd-jenkins$BUILD_DISPLAY_NAME" --solution-stack-name "64bit Amazon Linux 2018.03 v2.16.6 running GlassFish 5.0 Java 8 (Preconfigured - Docker)" --option-settings=file://options.json '
      }
    }
  }
}
