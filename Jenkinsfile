node
{
def path = tool name: "maven3.8.1" 
properties([pipelineTriggers([pollSCM('* * * * *')])])

stage('Checkout')
{
git branch: 'development', credentialsId: 'df2b6610-0d71-4f55-9f12-0bf69b887c2b', url: 'https://github.com/pss-devops/maven-web-application.git'
}

stage('Build')
{
sh "${path}/bin/mvn clean package"
}

stage('SonarReport')
{
sh "${path}/bin/mvn sonar:sonar"
}
/*
stage('ArtifactsStore')
{
sh "${path}/bin/mvn deploy"
}
*/
  
stage('UploadToTomcat')
{
sshagent(['3eccec92-2bab-4680-8686-121a06d96be0']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@13.127.103.62:/opt/apache-tomcat-9.0.45/webapps"
}
}

stage('Email')
{
emailext body: '''Build Over

regards
Pavankumar''', subject: 'Build Over', to: 'pavankumardadivu@gmail.com'
}
}
