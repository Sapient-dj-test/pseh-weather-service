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
                    sh "chmod +x ./mvnw"
                    sh "sh mvnw install dockerfile:build"
            }
        }

        stage('Docker Push Stage'){
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                     sh "docker push deepj/pseh-weather:latest"
                    }
                }
            }
        }

                stage('Docker stop Stage'){
                    steps{
                        script {
         script {
          sshPublisher(
           continueOnError: false, failOnError: true,
           publishers: [
            sshPublisherDesc(
             configName: "test-rig3",
             verbose: true,
             transfers: [
              sshTransfer(
               sourceFiles: "",
               removePrefix: "",
               remoteDirectory: "",
               execCommand: "sudo docker stop weather-services;sudo docker rm weather-services"
              )
             ])
           ])
         }                }
                    }
                }


                stage('Docker Run Stage'){
                    steps{
                        script {
         script {
          sshPublisher(
           continueOnError: false, failOnError: true,
           publishers: [
            sshPublisherDesc(
             configName: "test-rig3",
             verbose: true,
             transfers: [
              sshTransfer(
               sourceFiles: "",
               removePrefix: "",
               remoteDirectory: "",
               execCommand: "sudo docker pull deepj/pseh-weather:latest; sudo docker run -p 8080:8080 -d --name weather-services deepj/pseh-weather:latest"
              )
             ])
           ])
         }                }
                    }
                }

    }
}