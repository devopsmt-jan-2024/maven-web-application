pipeline{

agent any

tools{
maven 'maven 3.9.6' 
}

stages{
 stage('CHeckOutCode'){
 steps{
     sendSlackNotifications('STARTED')
 git branch: 'development', credentialsId: 'fb960c3a-0adb-4293-acac-0decc723fe54', url: 'https://github.com/devopsmt-jan-2024/maven-web-application.git'
}
}

stage('Build'){
steps{
sh "mvn clean package"
}
}
/*
stage('ExecuteSonarQubeReport'){
steps{
sh "mvn clean deploy"
}
}

stage('UploadArtifactIntoNexus'){
steps{
sh "mvn clean deploy"
}
}

stage('DeployAppIntoTomcat'){
steps{
sshagent(['97a3423f-de7f-4e70-bf4f-69744ba33287']) {
    sh "scp  -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@172.31.12.58:/opt/apache-tomcat-9.0.86/webapps"
}
}
}
*/
}//stages closing

post {
 success {
  sendSlackNotifications(currentBuild.result)
  }
  failure {
   sendSlackNotifications(currentBuild.result)
   }
}

}//pipeline closing

def sendSlackNotifications(String buildStatus = 'STARTED') {
  // build status of null means successful
  buildStatus =  buildStatus ?: 'SUCCESS'

  // Default values
  def colorName = 'RED'
  def colorCode = '#FF0000'
  def subject = "${buildStatus}: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'"
  def summary = "${subject} (${env.BUILD_URL})"

  // Override default values based on build status
  if (buildStatus == 'STARTED') {
    color = 'YELLOW'
    colorCode = '#FFFF00'
  } else if (buildStatus == 'SUCCESS') {
    color = 'GREEN'
    colorCode = '#00FF00'
  } else {
    color = 'RED'
    colorCode = '#FF0000'
  }
  
  // Send notifications
  slackSend (color: colorCode, message: summary, channel: "citibank-project")
}
