pipeline {
    agent any
    triggers {
  pollSCM '* * * * *'
}
    tools {
        maven 'M2_HOME' 
    }
    stages {
        stage('Build war') {
            steps {
                sh 'mvn clean'
                sh 'mvn install'
                sh 'mvn package'
            }
        }
        stage('Testing') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                script{
                    docker.withRegistry('', 'dockerid') {
                        def dockerimage = docker.build("lawrenceodedina/gggpipe:${env.BUILD_NUMBER}")
                        dockerimage.push()
                        dockerimage.push("latest")
                    }
                }
            }
        }
    }
}