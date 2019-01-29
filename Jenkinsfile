podTemplate(label: 'docker-test', volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')],
        containers: [
            containerTemplate(name: 'jnlp', alwaysPullImage: true, image: 'ccthub/jkslave')
        ]){
    node ('docker-test'){
    def app

    stage('Clone repository') {
            container('jnlp'){ 

        checkout scm
    }
    }

    stage('Build image') {
            container('jnlp'){
        sh "hostname"
        sh "pwd"
        sh "ls"
        sh "whoami"
        sh "kubectl get svc -n kong"
        sh "kubectl get all --all-namespaces"
        app = docker.build("getintodevops/hellonode")
            }
    }
    }
}

