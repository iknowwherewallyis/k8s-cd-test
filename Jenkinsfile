// USING DOCKER PLUGIN THIS WORKS WHEN RUN ON JENKINS MASTER 

/*
node {
    def app

    stage('Clone repository') {
        sh "hostname"
        checkout scm
    }

    stage('Build image') {
        sh "hostname"
        app = docker.build("getintodevops/hellonode")
    }
}
*/



//TRYING TO USE DOCKER PLUGIN WITH SLAVES DOESN'T WORK (THEY ARE ALL SEPERATE CONTAINERS IN A POD, SO NOT RUNNING ON JENKINS LEADER/MASTER WHERE DOCKER PLUGIN IS CONFIGURED. 
//ALL CONFIG FOR POD COMES FROM POD TEMPLATE, STILL NEED TO BIND TO HOST DOCKER SOCKET TO USE ANY DOCKER CMDS)
podTemplate(label: 'docker-test', 
            //serviceAccount: 'jenkins',
            volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')],
        
            containers: [
            containerTemplate(name: 'jnlp', alwaysPullImage: true, image: 'ccthub/jkslave')
            ])

{

    node ('docker-test'){
           //withKubeConfig([credentialsId: 'c52ca836-2101-4aa1-955e-a29ba4c8ba95',
            //        serverUrl: 'https://192.168.99.117:8443',
             //       contextName: 'minikube',
               //     clusterName: 'minikube',
                //      ]) {
    def app

            stage ('Build Application image') {
                container('jnlp') {
                    //def commit_id = sh(returnStdout: true, script: 'git rev-parse HEAD').trim().take(7)
                    //sh 'aws ecr get-login --no-include-email --region eu-central-1 > /auth.sh'
                    docker.withRegistry('https://cloud.docker.com/ccthub/', '282d475f-59e5-4487-a019-088461c228d0')
                    app = docker.build("ccthub/jenkins")
                    app.push("test-delete")
                    //sh '''printf "\n\nPushing image: \n\n"
                    //        docker push $1 | grep -v "Preparing" | grep -v "Waiting" | grep -v "Layer already exists"
                    //        printf "\n\n------------------------------\n\n"'''
                    
                    //sh "docker tag ccthub/jenkins:test-delete ccthub/jenkins:test-delete"
                    //sh "ecr_push.sh ${REPO_ADDRESS}/${PHP_REPO}:${commit_id}"
                }
            }
        }
}
