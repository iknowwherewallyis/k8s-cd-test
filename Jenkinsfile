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

/*

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
     //       container('jnlp'){ 
        sh "hostname"
        //sh "kubectl get po --all-namespaces" //this shouldn't work at all but it does
        checkout scm
        app = docker.build("getintodevops/hellonode")

    //}
    }

    stage('Build image') {
            container('jnlp'){
        sh "hostname"
        //sh "kubectl get svc -n kong"
        //sh "kubectl get po --all-namespaces"
        app = docker.build("getintodevops/hellonode")
            }
    }
    }
}


*/

/*
// CAN LIST RESOURCES JUST HAVING KUBECTL INSTALLED IN IMAGE. SHOULDN'T WORK THIS WAY (NO RBAC & ONLY WORKS ON MINIKUBE BECAUSE EVERYTHING RUNS ON MASTER NODE). 
// SHOULD USE KUBERNETES CLI PLUGININ PRODUCTION
node {
  stage('List pods') {
      echo "REACHED!!!!!!!!!!!"
      sh 'kubectl get pods'    
    }
}

*/


//SEE ABOVE. THIS IS HOW THIS SHOULD BE SETUP TO USE KUBECTL CMDS. REPLACEMENT FOR SCRIPTS 
node {
  stage('List pods') {
    withKubeConfig([credentialsId: '53b54779-b270-4125-a152-d3f280f41672',
                    serverUrl: 'https://kubernetes.default',
                    contextName: 'cct.marketing',
                    clusterName: 'cct.marketing',
                    ]) {
      sh 'kubectl get pods --all-namespaces'
    }
  }
}


