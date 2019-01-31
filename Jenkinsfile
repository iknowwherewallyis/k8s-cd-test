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
    def aws
           stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
                    checkout scm
                    }
                
            stage ('Build Application image') {
                container('jnlp') {
                    //def commit_id = sh(returnStdout: true, script: 'git rev-parse HEAD').trim().take(7)
                    //sh 'aws ecr get-login --no-include-email --region eu-central-1 > /auth.sh'
                    app = docker.build("ccthub/jenkins")
                    docker.withRegistry('https://167611661240.dkr.ecr.eu-central-1.amazonaws.com', 'ecr:eu-central-1:AWS'){

                    //app.push("test-delete")
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
