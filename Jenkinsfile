pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Building docker image') {
            when {
              branch 'master'
            }
            steps {
                echo 'Building docker image'
                script{
                     app = docker.build("ayoubely/train-schedule")
                     app.inside {
                      sh 'echo $(curl localhost:8080)'
                }
               
                }
            
            }
        }
        stage('Push Docker image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com','dockerhub-credentials'){
                        app.push("${env.BUILD_NUMBER}")
                        app.push('latest')
                    }
                }
            }
        }
    }
}
