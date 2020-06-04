#!groovy

node {
    def myImg
    stage ("Construccion de Imagen") {
        // download the dockerfile to build from
        //git 'git@diyvb:repos/dockerResources.git'

        // build our docker image
        myImg = docker.build 'hello-image:snapshot-1'
    }
}
