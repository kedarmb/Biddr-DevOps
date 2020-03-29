pipeline {
        
        agent any
        
        parameters {
                string(defaultValue: "devBuild", description: 'Which branch?', name: 'BRANCH_NAME')
        }

        environment {
                CHROME_BIN = '/bin/google-chrome'  
                registry = "thinkperfect/smartbid_frontend"
                registryCredential = 'admindockerRegistry'
                dockerImage = ''
        }
        
        stages {
                
                
                stage('Checkout SCM branch devBuild'){
                        steps{
                               git branch: 'devBuild', credentialsId: '956f3118-748c-4481-a443-9c2c12556c5e', url: 'https://github.com/thinkperfect/SmartBid-Frontend.git'
                        }
                
                }
                
        
              stage('Install node modules'){
                        steps{
                                
                                sh "npm install"
                               
                        }    
                }
        
               stage('Build the code'){
                        steps{
                                sh "npm run-script build"
                        }
                        post {
                                always {  
                                        emailext attachLog: true, attachmentsPattern: 'generatedFile.txt', body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}",
                                        compressLog: true, recipientProviders: [brokenBuildSuspects(), culprits(), developers(), requestor(), brokenTestsSuspects()], subject: "Jenkins Build ${currentBuild.currentResult}: Job ${env.JOB_NAME}",
                                        replyTo: 'devops@thinkperfect.io', to: 'devops@thinkperfect.io, asaha@thinkperfect.io'
                                }            
                        }
                } 
                
                stage('Building an image '){
                      steps {
                          script {
                                dockerImage = docker.build registry + ":$BUILD_NUMBER"
                          }
                  }
              }
                  stage('Push SmartBid-FrontEnd Docker Image to Docker Hub'){
                        steps{
                                withDockerRegistry(credentialsId: 'ebd92aa8-fd40-45c0-b185-c8b5d0c78bd6', url: "") {
                                      sh "docker push thinkperfect/smartbid_frontend:${env.BUILD_ID}"
                                }
                        }
                }
                stage('Remove Unused Docker images'){
                        steps {
                               sh "docker image prune -f" 
                        }
                }
        }
        post {
                always {
                      sh "cd /var/lib/jenkins/workspace"
                      cleanWs()
                }
        }
}


