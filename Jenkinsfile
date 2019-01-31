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
           withKubeConfig([credentialsId: 'f398c71e-c372-459b-bb87-e93d03eb332c',
                    serverUrl: 'https://api.cct.marketing',
                    contextName: 'cct.marketing',
                    clusterName: 'cct.marketing',
                    ]) {
        sh "hostname"
        //sh "kubectl get po --all-namespaces" //this shouldn't work at all but it does
        //checkout scm
        //app = docker.build("getintodevops/hellonode")

      //sh 'kubectl get pods --all-namespaces'
      sh 'kubectl cluster-info'
    }

    }
    //}

    stage('Build image') {
            container('jnlp'){
        sh "hostname"
        //sh "kubectl get svc -n kong"
        //sh "kubectl get po --all-namespaces"
        //app = docker.build("getintodevops/hellonode")
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
    withKubeConfig([credentialsId: 'f398c71e-c372-459b-bb87-e93d03eb332c',
                    serverUrl: 'https://api.cct.marketing',
                    contextName: 'cct.marketing',
                    clusterName: 'cct.marketing',
                    ]) {
      //sh 'kubectl get pods --all-namespaces'
      sh 'kubectl cluster-info'
    }
  }
  //  sh 'kubectl cluster-info'
}


/*
//MINIKUBE TEST
node {
  stage('List pods') {
    withKubeConfig([serverUrl: 'https://192.168.99.117:8443',
                    contextName: 'minikube',
                    clusterName: 'minikube',
                    ]) {
      //sh 'kubectl get pods --all-namespaces'
      sh 'kubectl cluster-info'
    }
  }
  //  sh 'kubectl cluster-info'
}
*/
