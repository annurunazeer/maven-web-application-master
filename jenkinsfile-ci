node{
    echo "GitHub Branch Name: ${env.BRANCH_NAME}"
    echo "Jenkins Build Number: ${env.BUILD_NUMBER}"
    echo "Jenkins Node Name: ${env.NODE_NAME}"

    echo "Jenkins Home: ${env.JENKINS_HOME}"
    echo "Jenkins URL: ${env.JENKINS_URL}"
    echo "Job Name: ${env.JOB_NAME}"
    properties([
    buildDiscarder(logRotator(numToKeepStr: '3')),
    pipelineTriggers([
        pollSCM('H/2 * * * *')
    ])
])

    def maven = tool name: "Maven", type: 'maven'

    stage('Checkout') {
        git branch: 'main',
            credentialsId: '5eeb9d60-91af-41f4-a9fd-d81c5a636453',
            url: 'https://github.com/annurunazeer/maven-web-application-master.git'
    } 

    stage('Build') {
        bat "${maven}\\bin\\mvn clean package"
    }

    stage('Execute SonarQube Report') {
            bat "${maven}\\bin\\mvn sonar:sonar"
    }

    stage('Upload Artifact into Nexus') {
        bat "${maven}\\bin\\mvn deploy"
    }
}
