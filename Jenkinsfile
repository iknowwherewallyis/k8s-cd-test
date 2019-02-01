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
           withKubeConfig([credentialsId: '53b54779-b270-4125-a152-d3f280f41672',
                    serverUrl: 'https://api.cct.marketing',
                    contextName: 'cct.marketing',
                    clusterName: 'cct.marketing',
			  ]){

    def app
    //def commit_id

           stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */
                    checkout scm
                    }
                
            stage ('Build Application image') {
                container('jnlp') {
   	        docker.withRegistry("${REPO_ADDRESS}", "282d475f-59e5-4487-a019-088461c228d0"){
		    def commit_id = sh(returnStdout: true, script: 'git rev-parse HEAD').trim().take(7)
			echo ${commit_id}
		    echo "BUILDING IMAGE"
		    app = docker.build("${PHP_REPO}", "-f Dockerfile.php .")
                    //docker.withRegistry('https://167611661240.dkr.ecr.eu-central-1.amazonaws.com', 'ecr:eu-central-1:581d148d-74b8-42c3-9d28-848c7f174a4f'){ 
		    echo "TAGGING IMAGE"
    		    app.push("$commit_id")
	            app.push("latest")
                    }
                }
            }
 	     stage ('Publishing new php image') {
                container('jnlp') {
                    def branch = sh(returnStdout: true, script: 'git name-rev --name-only HEAD|cut -f3 -d/').trim()
                    def commit_id = sh(returnStdout: true, script: 'git rev-parse HEAD').trim().take(7)
			echo ${commit_id}
                    if(branch == 'master') {
                        //sh "update_job.sh ${K8S_NAMESPACE} ${PHP_JOB} ${PHP_JOB} ${REPO_ADDRESS}/${PHP_REPO}:${commit_id}"
			sh 'kubectl -n default set image cronjob.batch/test hello=${REPO_ADDRESS}/${PHP_REPO}:${commit_id}'
                    }
                    if(branch != 'master') {
                        sh "echo 'Unsupported branch.'"
                        sh "exit 1"
                    }
                }
            }
        }
    }
}
}
