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
    def app
           stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
                    checkout scm
                    }
                
            stage ('Build Application image') {
                container('jnlp') {
                    //def commit_id = sh(returnStdout: true, script: 'git rev-parse HEAD').trim().take(7)
                    //sh 'aws ecr get-login --no-include-email --region eu-central-1 > /auth.sh'
                    docker.withRegistry('https://cloud.docker.com/ccthub/', '282d475f-59e5-4487-a019-088461c228d0'){
                    app = docker.build("ccthub/jenkins")
                    app.push("ccthub/jenkins:test-delete")
                    }
                    //sh '''printf "\n\nPushing image: \n\n"
                    //        docker push $1 | grep -v "Preparing" | grep -v "Waiting" | grep -v "Layer already exists"
                    //        printf "\n\n------------------------------\n\n"'''
                    
                    //sh "docker tag ccthub/jenkins:test-delete ccthub/jenkins:test-delete"
                    //sh "ecr_push.sh ${REPO_ADDRESS}/${PHP_REPO}:${commit_id}"
                }
            }
        }
}
