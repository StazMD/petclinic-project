pipeline {
    agent any
    tools{
        jdk 'Java 17'
        maven 'Maven 3.8.1'
    }

    stages{
        stage("Compile"){
            steps{
                sh "mvn clean compile"
            }
        }

         stage("Test Cases"){
            steps{
                sh "mvn test"
            }
        }

         stage("Build"){
            steps{
                sh "mvn clean install"
            }
        }
        stage("Build docker image"){
            steps{
                sh "docker build -t stanislavscurtu/petclinic:$env.BUILD_NUMBER ."
            }
        }
        stage("Publish docker image"){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: "docker-credentials", usernameVariable: "DOCKER_REPOSITORY_USER", passwordVariable: "DOCKER_REPOSITORY_PASSWORD")]){
                        sh "docker login -u $DOCKER_REPOSITORY_USER -p $DOCKER_REPOSITORY_PASSWORD"
                        sh "docker push stanislavscurtu/petclinic:$env.BUILD_NUMBER"
                    }
                }
            }
        }
    }
}
