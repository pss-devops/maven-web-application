node
{
def path = tool name: "maven3.8.1"
properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')), pipelineTriggers([pollSCM('* * * * *')])])
stage('git')
{
git branch: 'development', credentialsId: '3a00e5fa-6074-44d0-9f7e-f7467def8fe5', url: 'https://github.com/pss-devops/maven-web-application.git'
}

stage('package')
{
sh "${path}/bin/mvn clean package"
}

stage('Report')
{
sh "${path}/bin/mvn sonar:sonar"
}

stage('Store')
{
sh "${path}/bin/mvn deploy"
}

stage('Tomcat')
{
sshagent(['3eccec92-2bab-4680-8686-121a06d96be0']) {
sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.2.128.141:/opt/apache-tomcat-9.0.45/webapps"
}
}

stage('email')
{
emailext body: '''Build over

regards
pavan''', subject: 'Build over', to: 'pavankumardadivu@gmail.com'
}

}
