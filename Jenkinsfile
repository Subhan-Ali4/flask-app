pipeline {

    agent any

    stages {

        stage("code") {

            steps {

                git url: 'https://github.com/Subhan-Ali4/flask-app.git', branch: 'main'

            }

        }

        stage("build") {

            steps {

                sh 'docker build -t flask-app .'

            }

        }

        stage("test") {

            steps {

                echo "code test done"

            }

        }
        stage("push to docker hub"){
            steps{
                
                withCredentials([usernamePassword(
                    credentialsId:'dockerhubcreds',
                    usernameVariable:'dockerHubUser',
                    passwordVariable:'dockerHUBPass')]){
                        sh "docker login -u ${env.dockerhubUser} -p ${env.dockerHubPass}"
                        sh "docker image tag flask-app ${env.dockerHubUser}/flask-app:latest"
                        sh "docker push ${env.dockerHubUser}/flask-app:latest"
                    }
            }
            
        }
        stage("deployment") {

            steps {

                sh 'docker-compose down'          // stop old containers
                sh 'docker-compose up -d'         // start fresh with new image

            }

        }

    }

}
