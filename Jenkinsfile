pipeline {
agent any

stages {
stage('Clean') {
steps {
sh 'mvn clean'
script {
if (currentBuild.result == 'SUCCESS') {
env.BUILD_STAGE1_STATUS = 'SUCCESS'
} else {
env.BUILD_STAGE1_STATUS = 'FAILURE'
}
}
}
}

stage('Install') {
steps {
sh 'mvn install'
script {
if (currentBuild.result == 'SUCCESS') {
env.BUILD_STAGE2_STATUS = 'SUCCESS'
} else {
env.BUILD_STAGE2_STATUS = 'FAILURE'
}
}
}
}
}

post {
always {
script {
def summary = "Pipeline summary:\n"
summary += "Build stage: ${env.BUILD_STAGE1_STATUS}\n"
summary += "Test stage: ${env.BUILD_STAGE2_STATUS}\n"
slackSend channel: '#fundamentos-de-devops', message: summary, tokenCredentialId: 'SecretSlack'
}
}
}
}