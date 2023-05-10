pipeline {
    agent any
        stages {
        stage('Initialize'){
            steps{
                echo "Esta es el inicio"
                slackSend  color: "#439FE0", channel: "modulo3actividadgrupal", message: "${STAGE_NAME}"
            }
        }            
            
            
            
        stage('Test2') {
            steps {
                 
           sh 'cat Jenkinsfile'     
            slackSend  color: "#439FE0", channel: "modulo3actividadgrupal", message: "${STAGE_NAME} "
            }
        }              
          
        stage('Sonar') {
            steps {
                 
sh '/var/jenkins_home/sonar-scanner/bin/sonar-scanner -Dsonar.projectBaseDir=. -Dsonar.projectKey=modulo3actividadgrupal -Dsonar.sources=. -Dsonar.host.url=http://192.168.1.89:9001 -Dsonar.login=sqp_49838cf4416d600b4d2d925bca6dfa1edd9771ac -Dsonar.exclusions=sonar.java.binaries/** -Dsonar.java.binaries=**'
                slackSend  color: "#439FE0", channel: "modulo3actividadgrupal", message: "${STAGE_NAME} "
            }
        }           
            
            
        stage('Build') {
            steps {
                sh 'mvn -B package'
                slackSend  color: "#439FE0", channel: "modulo3actividadgrupal", message: "${STAGE_NAME} "
            }
        }
            
        stage('Test') {
            steps {
                 sh "mvn clean verify" 
                slackSend  color: "#439FE0", channel: "modulo3actividadgrupal", message: "${STAGE_NAME}"
            }
        }                    
            
        stage("Publish to Nexus Repository Manager") {
            steps {
                script {
                    pom = readMavenPom file: "pom.xml";
                    filesByGlob = findFiles(glob: "target/*.${pom.packaging}");
                    echo "${filesByGlob[0].name} ${filesByGlob[0].path} ${filesByGlob[0].directory} ${filesByGlob[0].length} ${filesByGlob[0].lastModified}"
                    artifactPath = filesByGlob[0].path;
                    artifactExists = fileExists artifactPath;
                    if(artifactExists) {
                        echo "*** File: ${artifactPath}, group: ${pom.groupId}, packaging: ${pom.packaging}, version ${pom.version}";
                        nexusArtifactUploader(
                            nexusVersion: "nexus3",
                            protocol: "http",
                            nexusUrl: "192.168.1.2:8081",
                            groupId: pom.groupId,
                            version: pom.version,
                            repository: "maven-releases",
                            credentialsId: "nexus",
                            artifacts: [
                                [artifactId: pom.artifactId,
                                        classifier: '',
                                        file: artifactPath,
                                        type: pom.packaging]
                            ]
                        );
                    } else {
                        error "*** File: ${artifactPath}, could not be found";
                    }
                }
                slackSend  color: "#439FE0", channel: "modulo3actividadgrupal", message: "${STAGE_NAME}"
            }
        } 
     }
    
    post {
        always {
        slackSend  channel: "modulo3actividadgrupal", message: "Job: ${env.JOB_NAME} - Result: ${currentBuild.currentResult}"
            }
     }
}
