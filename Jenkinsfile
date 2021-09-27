pipeline {
    
    environment {
        registry = "joshuasr/cs-204-jenkins-calculator"
        registryCredential = 'dockerhub'
        dockerImage=''
    }
    
    agent any
    
    tools {
     maven 'apache maven 3.6.3'
     jdk 'JDK 8'
    }

    stages {
        stage('Build') {
            steps {
                sh 'echo Build'
            }
        }
        
        stage('Test') {
            steps {
                sh 'echo Test'
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'echo Deploy'
            }
        }
        
        stage ('Package') {
            steps {
                sh 'mvn package'
                archiveArtifacts artifacts: 'src/**/*.java'
                archiveArtifacts artifacts: 'target/*.jar'
            }
        }

        stage ('Building image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        
        stage ('Deploy Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        
        stage ('Remove unused docker image') {
            steps {
                sh "docker rmi $registry:$BUILD_NUMBER"
            }
        }

    }
    post {
        failure{
              mail to: 'joshuasrichardson1@gmail.com',
          subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
          body: "Something is wrong with ${env.BUILD_URL}"
        }
    }

}
