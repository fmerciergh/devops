#!/usr/bin/env groovy

node {
    stage('checkout') {
        checkout scm
    }

    docker.image('jhipster/jhipster:v5.3.1').inside('-u root') {
        stage('check java') {
            sh "java -version"
        }

        stage('quality analysis') {
            withSonarQubeEnv('sonar') {
            }
        }
    }

    def dockerImage
    stage('build docker') {
	sh "cp -R src/main/docker target"
	sh "cp target/*.war target/docker/"
	dockerImage = docker.build('execut/devops-repo', 'target/docker')
    }

    stage('publish docker') {
        docker.withRegistry('https://registry.hub.docker.com', 'docker-login') {
            dockerImage.push 'latest'
        }
    }
}
