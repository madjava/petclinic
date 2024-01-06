pipeline {
  agent any
    tools {
      maven 'maven3'
                 jdk 'JDK8'
    }
    stages {
        stage('Build maven ') {
            steps {
                    sh 'pwd'
                    sh 'mvn  clean install package'
            }
        }

        stage('Copy Artifact') {
           steps {
                   sh 'pwd'
		           sh 'cp -r target/*.jar docker'
           }
        }

        stage('Build docker image') {
           steps {
               script {
                 def customImage = docker.build('madjava/petclinic', "./docker")
                 docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                 customImage.push("${env.BUILD_NUMBER}")
                 }
           }
        }
	  }

    stage('Build on K8s ') {
    steps {
                sh 'pwd'
                sh 'cp -R helm/* .'
                sh 'ls -ltr'
                sh 'pwd'
                sh '/usr/local/bin/helm upgrade --install petclinic-app petclinic  --set image.repository=registry.hub.docker.com/madjava/petclinic --set image.tag=${$BUILD_NUMBER}'

     }
    }

  }
}
