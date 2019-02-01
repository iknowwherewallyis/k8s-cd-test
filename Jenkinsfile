//TRYING TO USE DOCKER PLUGIN WITH SLAVES DOESN'T WORK (THEY ARE ALL SEPERATE CONTAINERS IN A POD, SO NOT RUNNING ON JENKINS LEADER/MASTER WHERE DOCKER PLUGIN IS CONFIGURED. 
//ALL CONFIG FOR POD COMES FROM POD TEMPLATE, STILL NEED TO BIND TO HOST DOCKER SOCKET TO USE ANY DOCKER CMDS)
withCredentials([
    string(credentialsId: 'PHP_REPO', variable: 'PHP_REPO'),
    string(credentialsId: 'REPO_ADDRESS', variable: 'REPO_ADDRESS'),
]) {
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
                //container('jnlp') {
                    //def commit_id = sh(returnStdout: true, script: 'git rev-parse HEAD').trim().take(7)
                    //sh '''aws ecr get-login --no-include-email --region eu-central-1 > /auth.sh
                    //	chmod 777 /auth.sh
	            //source /auth.sh 2> /dev/null'''
		    def commit_id = sh(returnStdout: true, script: 'git rev-parse HEAD').trim().take(7)
		    sh 'ls'
		    echo "BUILDING IMAGE"
		    app = docker.build('${PHP_REPO}', '-f Dockerfile.php .')
                    //docker.withRegistry('https://167611661240.dkr.ecr.eu-central-1.amazonaws.com', 'ecr:eu-central-1:581d148d-74b8-42c3-9d28-848c7f174a4f'){ 
		    docker.withRegistry('${REPO_ADDRESS}', '282d475f-59e5-4487-a019-088461c228d0'){
		    echo "TAGGING IMAGE"
                    app.tag("latest")
    		    app.push("$commit_id")
	            app.push("latest")
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
