/*podTemplate(label: 'docker-test',
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
        //sh "kubectl get all --all-namespaces"
        app = docker.build("getintodevops/hellonode")
            }
    }
    }
}

*/

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

node {
  stage('List pods') {
    withKubeConfig([credentialsId: '53b54779-b270-4125-a152-d3f280f41672',
                    caCertificate: '''
-----BEGIN CERTIFICATE-----
MIIG2DCCBcCgAwIBAgIRAJhGgWJ0E34NdcIG2BbFe24wDQYJKoZIhvcNAQELBQAw
gZAxCzAJBgNVBAYTAkdCMRswGQYDVQQIExJHcmVhdGVyIE1hbmNoZXN0ZXIxEDAO
BgNVBAcTB1NhbGZvcmQxGjAYBgNVBAoTEUNPTU9ETyBDQSBMaW1pdGVkMTYwNAYD
VQQDEy1DT01PRE8gUlNBIERvbWFpbiBWYWxpZGF0aW9uIFNlY3VyZSBTZXJ2ZXIg
Q0EwHhcNMTgwNDA5MDAwMDAwWhcNMjAwNDA4MjM1OTU5WjBdMSEwHwYDVQQLExhE
b21haW4gQ29udHJvbCBWYWxpZGF0ZWQxHjAcBgNVBAsTFUVzc2VudGlhbFNTTCBX
aWxkY2FyZDEYMBYGA1UEAwwPKi5jY3QubWFya2V0aW5nMIIBIjANBgkqhkiG9w0B
AQEFAAOCAQ8AMIIBCgKCAQEArhUKVo0dKvU01P3WSdEHCuY8w+sLo25HIOog+WQV
FtHvKybvANNklARkZjARoMsPuCo2xD8xGyqZruxGtHS2Zy8zEqU1/3oj/vlR5Nkf
3x0R02MG2nJT1oL8KVuGaO7aOei8jNGz+CJ1yVkQkfq7ceGo2pNs1CGckaARDygf
7/FxTGl+fPSveVz6QaqmGHJsaYXorsw2JhjQcXzFdf/WEO86euvwl32PQoWsXCz6
jNLiNi4cCExXEr4MiaGxTBe84xS0qB0sTalZzeD3u1Hkbd9IGwpDttU/oXwrRS82
09/hOoM1kEoqCh1Y3BowQ+TzUbxnCzOq2dZl+kP4ccPkOwIDAQABo4IDXTCCA1kw
HwYDVR0jBBgwFoAUkK9qOpRaC9iQ6hJWc99DtDoo2ucwHQYDVR0OBBYEFF6sc6eY
UiUuAbR3fTNE9zRZlDD/MA4GA1UdDwEB/wQEAwIFoDAMBgNVHRMBAf8EAjAAMB0G
A1UdJQQWMBQGCCsGAQUFBwMBBggrBgEFBQcDAjBPBgNVHSAESDBGMDoGCysGAQQB
sjEBAgIHMCswKQYIKwYBBQUHAgEWHWh0dHBzOi8vc2VjdXJlLmNvbW9kby5jb20v
Q1BTMAgGBmeBDAECATBUBgNVHR8ETTBLMEmgR6BFhkNodHRwOi8vY3JsLmNvbW9k
b2NhLmNvbS9DT01PRE9SU0FEb21haW5WYWxpZGF0aW9uU2VjdXJlU2VydmVyQ0Eu
Y3JsMIGFBggrBgEFBQcBAQR5MHcwTwYIKwYBBQUHMAKGQ2h0dHA6Ly9jcnQuY29t
b2RvY2EuY29tL0NPTU9ET1JTQURvbWFpblZhbGlkYXRpb25TZWN1cmVTZXJ2ZXJD
QS5jcnQwJAYIKwYBBQUHMAGGGGh0dHA6Ly9vY3NwLmNvbW9kb2NhLmNvbTApBgNV
HREEIjAggg8qLmNjdC5tYXJrZXRpbmeCDWNjdC5tYXJrZXRpbmcwggF+BgorBgEE
AdZ5AgQCBIIBbgSCAWoBaAB1AO5Lvbd1zmC64UJpH6vhnmajD35fsHLYgwDEe4l6
qP3LAAABYqmZu6kAAAQDAEYwRAIgSRAbNh3gMXO4ouvZ9hBUF5bjJTikXbMkyWc9
MTLFiNQCICFLU7ELE67wh5v+Wo1BlzO/uGcLtrxmjrGZVm8JbhLDAHYAXqdz+d9W
wOe1Nkh90EngMnqRmgyEoRIShBh1loFxRVgAAAFiqZmtHwAABAMARzBFAiEAuoGF
w5M/coY4Dq1JbV5ggRkHg8FWNTTNd/uAeKWy4JkCIDtVxEdC53D+bviwGV7nfxHP
59plg7NOjEYNQvVwcnUFAHcAb1N2rDHwMRnYmQCkURX/dxUcEdkCwQApBo2yCJo3
2RMAAAFiqZmsJAAABAMASDBGAiEA/s1S+lTRUjoM0UYBR6N2kP+BTBsfIA4ExgHI
om2GFagCIQCVX0Vbfbf3S1r0qLFL19wDcr0YaFNcx7VPQ6MSWESG5DANBgkqhkiG
9w0BAQsFAAOCAQEASRQnXLCs7r4BDu6odAyw2ytrQXXQDltailk+d82XFa4i4NbU
LSI1yupDTX1tvzOj9rRDCBS9ixc+svZ37VTOWj9iYmZPV9mudJj+EqQW40RRWsXL
v6A8LakjJ4y5txSxwY7cvV1COinNwP1aiI32Fa9vPnxTERF1E0nbfYi18ai7qg0m
DqJcOpZGGl7kv9imToguzeA9WiRt/r5/k/tglwk5FjhQoIyNW9xpFlgMDsrM0/mm
w59nF+qJCMx3zTijh0gKLmdRt2wFEBI6aRcnybok5BxRsHzvzO7ogK8b6WZZWX/Z
Tq4mQ/mBYueU/+My3zEIIsofD1DEN0K0R7gjOQ==
-----END CERTIFICATE-----''',
                    serverUrl: 'https://kubernetes.default',
                    contextName: 'cct.marketing',
                    clusterName: 'cct.marketing'
                    ]) {
      sh 'kubectl get pods'
    }
  }
}

