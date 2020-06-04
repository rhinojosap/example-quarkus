#!groovy

pipeline {
  environment {
    registry = "gcr.io/cloud-week/"
    registryCredential = 'cloud-week-push-pull-registry'
  }

  agent {
    kubernetes {
      label 'spring-petclinic-demo'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: jenkins-1590612295
  containers:
  - name: maven
    image: maven:latest
    command:
    - cat
    tty: true
    volumeMounts:
      - mountPath: "/root/.m2"
        name: m2
  - name: docker
    image: docker:latest
    command:
    - cat
    tty: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-sock
  volumes:
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock
    - name: m2
      persistentVolumeClaim:
        claimName: pvc-jenkins-slave-kubernetes-maven-quarkus
"""
}
   }
  stages {
    stage ("Construccion de Imagen") {
      steps {
        container('docker') {
        // download the dockerfile to build from
        //git 'git@diyvb:repos/dockerResources.git'

        // build our docker image
        //myImg = docker.build 'hello-image:snapshot-1'
            sh '''
            docker build -t registry + "hello-image:snapshot-${env.BUILD_ID}" .
            '''
        }
      
}    }
    stage('Test') {
      steps {
        container('maven') {
          sh """
             #mvn test
             echo "mvn test"
          """
        }
      }
    }
    stage('Push') {
      steps {
        container('docker') {
          sh """
             #docker build -t spring-petclinic-demo:$BUILD_NUMBER .
             echo "docker push"
          """
        }
      }
    }
  }
}
