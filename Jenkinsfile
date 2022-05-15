
node
{
    def maven= tool name:'maven3.8.5'
    properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '2', daysToKeepStr: '', numToKeepStr: '2')), [$class: 'JobLocalConfiguration', changeReasonComment: ''], pipelineTriggers([pollSCM('* * * * *')])])
    stage('checkOutCode')
    {
        git branch: 'development', credentialsId: 'cca8cd8c-2735-4aee-bf65-8ca7eb587e0f', url: 'https://github.com/mss-ec-apps-DvOps/maven-web-application.git'
    }
    stage('build')
    {
        sh "${maven}/bin/mvn clean package"
    }
    stage('ExecuteSonarQubeReport')
    {
        sh "${maven}/bin/mvn sonar:sonar"
    }
    stage('UploadArtificateIntoNexusRepo')
    {
        sh "${maven}/bin/mvn deploy"
    }
    stage('deployAppIntoTomcat')
    {
        sshagent(['8ab815c0-4a8a-44b7-b415-9dbd852d861e']) {
             sh "scp -o StrictHostKeyChecking=no target/maven-web-application.war ec2-user@65.1.148.203:/opt/apache-tomcat-9.0.62/webapps"
        }
        
    }
}
