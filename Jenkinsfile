/*podTemplate(label: 'docker-test', volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')],
        containers: [
            containerTemplate(name: 'jnlp', alwaysPullImage: true, image: 'ccthub/jkslave')
        ]){
    node ('docker-test'){
    def app

    stage('Clone repository') {
            container('jnlp'){
         Let's make sure we have the repository cloned to our workspace 

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
        //app = docker.build("getintodevops/hellonode")
            }
    }
    }
}
*/
node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */
                sh "hostname"
        sh "pwd"
        sh "ls"
        sh "whoami"
        //sh "docker version"
        sh 'groupadd docker'
        sh 'usermod -aG docker $USER'
        sh 'docker build -t getintodevops/hellonode .'
        //app = docker.build("getintodevops/hellonode")
    }
}
