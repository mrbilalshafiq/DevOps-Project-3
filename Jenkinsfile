pipeline {
    agent any 
    stages{
        stage('Testing'){
                steps{
                    sh 'chmod +x jenkins/testing.sh'
                    sh './jenkins/testing.sh'
                }
            }
        stage("Build Images"){
            steps{
                sh 'chmod +x jenkins/build.sh'
                sh './jenkins/build.sh'
            }
        }
         stage('Push'){
                steps{
                    script{
                            docker.withRegistry('', 'docker-hub-credentials'){
                                sh '''
                                cd app
                                docker-compose push
                                docker system prune -af
                                '''
                            }
                        }
                    }
                }
        stage("Deployment"){
            steps{
                sh 'aws eks --region eu-west-2 update-kubeconfig --name my-cluster'
                sh 'chmod +x jenkins/deployment.sh'
                sh './jenkins/deployment.sh'
            }
        }
        
    }
    
}