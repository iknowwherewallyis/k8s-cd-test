/*
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
	   docker.withRegistry("${REPO_ADDRESS}", "DOCKERHUB_CREDS"){
		   //withKubeConfig([credentialsId: "K8S_CREDS",
                   // serverUrl: 'https://api.cct.marketing',
                    //contextName: 'cct.marketing',
                    //clusterName: 'cct.marketing',
			//  ]){
		   

    def app
   

           stage('Clone repository') {

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
	     stage ('Publish tagged php image') {
                container('jnlp') {
                    def branch = sh(returnStdout: true, script: 'git name-rev --name-only HEAD|cut -f3 -d/').trim()
                    def commit_id = sh(returnStdout: true, script: 'git rev-parse HEAD').trim().take(7)
                    if(branch == 'master') {
                        echo "Pushing image to remote registry with TAG 'production'..."
			app = docker.build("${PHP_REPO}", "-f Dockerfile.php .")
                        app.push("production")
                    }
                }
            }
        }
    }
}
}
*/

node {
  stage('List pods') {
           withKubeConfig([credentialsId: 'c52ca836-2101-4aa1-955e-a29ba4c8ba95',
                    serverUrl: 'https://192.168.99.117:8443',
                    contextName: 'minikube',
                    clusterName: 'minikube',
                      ]) {
      sh 'kubectl get pods --all-namespaces'
      //sh 'kubectl cluster-info'
      //sh 'kubectl -n jenkins set image deployment/jenkins-leader jenkins-leader=ccthub/jenkins:1.1'
      //kubectl set image deployment.v1.apps/nginx-deployment nginx=nginx:1.91 --record=true
    }
  }
  //  sh 'kubectl cluster-info'
}
