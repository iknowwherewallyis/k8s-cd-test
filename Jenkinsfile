podTemplate(label: 'docker-test', serviceAccount: 'jenkins'
        volumes: [hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')],
        containers: [
            containerTemplate(name: 'jnlp', alwaysPullImage: true, image: 'ccthub/jkslave')
        ]){
    node ('docker-test'){
    def app

    stage('Clone repository') {
            //container('jnlp'){ 
        checkout scm
        app = docker.build("getintodevops/hellonode")

    //}
    }

    stage('Build image') {
            container('jnlp'){
        sh "hostname"
        sh "pwd"
        sh "ls"
        sh "whoami"
        //sh "kubectl get svc -n kong"
        sh "kubectl get all --all-namespaces"
        app = docker.build("getintodevops/hellonode")
            }
    }
    }
}



/*

node {
    def app

    stage('Clone repository') {

        checkout scm
    }

    stage('Build image') {

        app = docker.build("getintodevops/hellonode")
    }
}

*/


/*
node {
  stage('List pods') {
      echo "REACHED!!!!!!!!!!!"
      sh 'kubectl get pods'    
    }
}

*/
/*
node {
  stage('List pods') {
    //withKubeConfig([credentialsId: '53b54779-b270-4125-a152-d3f280f41672',
      //              serverUrl: 'https://kubernetes.default',
        //            contextName: 'cct.marketing',
          //          clusterName: 'cct.marketing'
            //        ]) {
      sh 'kubectl get pods --all-namespaces'
    //}
  }
}
*/

