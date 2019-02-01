//TRYING TO USE DOCKER PLUGIN WITH SLAVES DOESN'T WORK (THEY ARE ALL SEPERATE CONTAINERS IN A POD, SO NOT RUNNING ON JENKINS LEADER/MASTER WHERE DOCKER PLUGIN IS CONFIGURED. 
//ALL CONFIG FOR POD COMES FROM POD TEMPLATE, STILL NEED TO BIND TO HOST DOCKER SOCKET TO USE ANY DOCKER CMDS)
withCredentials([
    string(credentialsId: 'PHP_REPO', variable: 'PHP_REPO'),
    string(credentialsId: 'REPO_ADDRESS', variable: 'REPO_ADDRESS'),
    //[$class: 'UsernamePasswordMultiBinding', credentialsId: 'K8s_CREDS', variable: 'K8S_CREDS'],
    //[$class: 'UsernamePasswordMultiBinding', credentialsId: 'AWS_CREDS', variable: 'AWS_CREDS'],
    //[$class: 'UsernamePasswordMultiBinding', credentialsId: 'DOCKERHUB_CREDS', variable: 'DOCKERHUB_CREDS']
]) {
podTemplate(label: 'docker-test', 
            //serviceAccount: 'jenkins',
            volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')],
        
            containers: [
            containerTemplate(name: 'jnlp', alwaysPullImage: true, image: 'ccthub/jkslave')
            ])

{

    node ('docker-test'){
	   docker.withRegistry("${REPO_ADDRESS}", "DOCKERHUB_CREDS"){
		   withKubeConfig([credentialsId: "K8s_CREDS",
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
		    def commit_id = sh(returnStdout: true, script: 'git rev-parse HEAD').trim().take(7)
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
			//echo "$commit_id"
                    if(branch == 'master') {
			sh "kubectl -n default set image cronjob.batch/test hello=${REPO_ADDRESS}/${PHP_REPO}:${commit_id}"
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
