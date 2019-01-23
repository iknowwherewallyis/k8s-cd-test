podTemplate(label: 'docker-test', volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')],
        containers: [
            containerTemplate(name: 'jnlp', alwaysPullImage: true, image: 'ccthub/jkslave')
        ]){
    node ('docker-test'){
    def app

    stage('Clone repository') {
            container('jnlp'){
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }
    }

    stage('Build image') {
            container('jnlp'){
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        sh "hostname"
        sh "pwd"
        sh "ls"
        sh "whoami"
        sh "kubectl get clusterrolebindings --all-namespaces" 
        sh "kubectl get clusterroles --all-namespaces"
        sh "kubectl get rolebindings --all-namespaces"
        sh "kubectl get roles --all-namespaces"
        //app = docker.build("getintodevops/hellonode")
            }
    }
    }
}
