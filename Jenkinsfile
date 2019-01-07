pipeline{
    environment {
        registry = 'deepj/pseh-weather'
        registryCredential = 'pseh-weather-dj-docker'
        imageName = ''
        dockerImage = ''
    }
    agent any
    stages{
        stage('Build Stage'){
            steps{
                withMaven(maven: 'M3'){
                    sh "mvn install dockerfile:build"
                }
            }
        }

        stage('Docker Push Stage'){
            steps{
                echo "Pushing Image : ${imageName}"
                script {
                    docker.withRegistry( '', registryCredential ) {
                     dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy & Run Stage'){
            steps{
                sshagent(['aws_dev_ssh']) {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@18.222.224.62 docker run -p 8080:8080 -d --name forecast-services ${imageName}"
                }
            }
        }
    }
}