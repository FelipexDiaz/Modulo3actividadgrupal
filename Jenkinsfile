pipeline {
    agent any
        stages {
        stage('Initialize'){
            steps{
                echo "Esta es el inicio"
                slackSend  color: "#439FE0", channel: "fundamentos-de-devops", message: "${STAGE_NAME}  :unicorn_face:", username: "felipe diaz"
            }
        }
            
            
        stage('Test2') {
            steps {
                 
           sh 'cat Jenkinsfile'     
            slackSend  color: "#439FE0", channel: "fundamentos-de-devops", message: "${STAGE_NAME}  :unicorn_face:"
            }
        }              
          
        stage('Sonar') {
            steps {
                 
sh '/var/jenkins_home/sonar/bin/sonar-scanner -Dsonar.projectBaseDir=. -Dsonar.projectKey=pipe -Dsonar.sources=. -Dsonar.host.url=http://192.168.1.2:9000 -Dsonar.login=squ_748e8a3c4efd1b032364b067b52a2d33b5755ff9 -Dsonar.exclusions=sonar.java.binaries/** -Dsonar.java.binaries=**'
                slackSend  color: "#439FE0", channel: "fundamentos-de-devops", message: "${STAGE_NAME}  :unicorn_face:"
            }
        }           
            
            
        stage('Build') {
            steps {
                sh 'mvn -B package'
                slackSend  color: "#439FE0", channel: "fundamentos-de-devops", message: "${STAGE_NAME}  :unicorn_face:"
            }
        }
            
        stage('Test') {
            steps {
                 sh "mvn clean verify" 
                slackSend  color: "#439FE0", channel: "fundamentos-de-devops", message: "${STAGE_NAME}  :unicorn_face:"
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
                slackSend  channel: "fundamentos-de-devops", message: "${STAGE_NAME}"
            }
        } 
     }
    
    post {
        always {
        slackSend  channel: "fundamentos-de-devops", message: "Job: ${env.JOB_NAME} - Result: ${currentBuild.currentResult}"
            }
     }
}
