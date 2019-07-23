node
{
  echo "GitHub BranhName ${env. BRANCH_NAME}"
  echo "Jenkins Job Number ${env.BUILD_NUMBER}"
  echo "Jenkins Node Name ${env.NODE_NAME}"
  
  echo "Jenkins Home ${env.JENKINS_HOME}"
  echo "Jenkins URL ${env.JENKINS_URL}"
  echo "JOB Name ${env.JOB_NAME}"
  
  def mvnHome = tool name: 'maven3.6.1', type: 'maven'
  /* To keep max no of builds */
  properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '3'))])

    stage ('checkout code'){
    git branch: 'development', credentialsId: 'c2cf3300-f752-4e07-8cf5-77275a41a2a7', url: 'https://github.com/Giri512/maven-web-application.git'
    }
    
    stage ('Build'){
      sh "${mvnHome}/bin/mvn clean package"
    }
    
   stage ('Sonarqube Report'){
      sh "${mvnHome}/bin/mvn sonar:sonar"
    }
    stage ('Uploadartifacts to Nexus Server'){
      sh "${mvnHome}/bin/mvn deploy"
    } 
     stage ('Deploy App to Tomcat Server'){
      sshagent(['tomcatsrv']) {
          /* Need to mention Tomcat IP */
    sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@35.154.34.199:/opt/apache-tomcat-9.0.20/webapps/maven-web-application.war"
}
    } 
   /*stage ('Email Notification'){
      mail bcc: '', body: '''Build Completed Successfully.

Best Regards,
Giri.''', cc: '', from: '', replyTo: '', subject: 'Build Completed', to: 'ngkumar.kali@gmail.com'
    } */
}
