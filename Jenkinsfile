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
                    sh ".\mvnw install dockerfile:build"
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


    }
}