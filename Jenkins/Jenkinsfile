def registry = 'https://hub.docker.com/repositories/vickykumawat'
def imageName = 'vickykumawat/ttrend'
def version   = '2.1.5'
pipeline {
    agent {
        node {
            label 'maven'
        }
    }
environment {
    PATH = "/opt/apache-maven-3.9.10/bin:$PATH"
}
    stages {
        stage("Code-build"){
            steps {
                 echo "----------- build started ----------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                 echo "----------- build complted ----------"
            }
        }
        stage("test"){
            steps{
                echo "----------- unit test started ----------"
                sh 'mvn surefire-report:report'
                 echo "----------- unit test Complted ----------"
            }
        }  

        stage("Docker Image Creation"){
            steps{
                echo "This is the building the code"
                sh "docker build -t ${imageName}:${version} ."
                echo "Build is Successfully"
            }
        }
        stage("Push to DockerHub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    echo "Logging in to Docker Hub..."
                    sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                    
                    echo "Pushing image to Docker Hub..."
                    sh "docker push ${imageName}:${version}"
                
                }
            }
        }
      stage(" Deploy ") {
       steps {
         script {
           sh './deploy.sh'
         }
       }
     }
       
}
}
