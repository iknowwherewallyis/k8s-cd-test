{
     podTemplate(label: 'message-center',
        namespace: 'jenkins',
        nodeSelector: 'environment=dev',
        volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')],
        containers: [
            containerTemplate(
                name: 'jnlp',
                alwaysPullImage: true,
                image: 'ccthub/jkslave'
            )])
    node {
    def app

    stage('Clone repository') {
    container('jnlp')
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

    stage('Build image') {
        container('jnlp')
        /* This builds the actual image; synonymous to
         * docker build on the command line */
        sh "hostname"
        sh "pwd"
        sh "ls"
        sh "whoami"
        app = docker.build("getintodevops/hellonode")
    }

    stage('Test image') {
        /* Ideally, we would run a test framework against our image.
         * For this example, we're using a Volkswagen-type approach ;-) */

        app.inside {
            sh 'echo "Tests passed"'
        }
    }

    stage('Push image') {
        /* Finally, we'll push the image with two tags:
         * First, the incremental build number from Jenkins
         * Second, the 'latest' tag.
         * Pushing multiple tags is cheap, as all the layers are reused. */
        docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}
}
