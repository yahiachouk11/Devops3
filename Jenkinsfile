pipeline{
    agent any
    triggers {
    }
    tools{
        nodejs "node"
    }
    environment {
        // Initialize global variables here
        APP_NAME="monimage"
        DOCKER_USER = "yahiachouk"
        DOCKER_PASS="dockerhub"
        IMAGE_NAME="${DOCKER_USER}"+"/"+"${APP_NAME}"
        IMAGE_TAG = '1.0'   
    }
    stages{
        stage("Initialization"){
            steps{
                script {
                    // Initialize global variables or perform other setup tasks
                    ENV_VAR_1 = 'value1'
                    ENV_VAR_2 = 'value2'
                }
            }
        }
        stage("Cleanup Workspace"){
            steps{
                cleanWs()
            }
        }
        stage("Checkout from SCM"){
            steps{
                git branch: 'main', credentialsId:'github', url:'https://github.com/yahiachouk11/Devops3'
            }
        }
        
        stage("Test Application"){
            steps{  
                sh "npm test"
            }
        }

        stage("Push to Registry"){
            steps{
                script{
                    docker.withRegistry('',DOCKER_PASS){
                        docker_image=docker.build"${IMAGE_NAME}"
                    }
                    docker.withRegistry('',DOCKER_PASS){
                        docker_image.push("${IMAGE_TAG}")
                    }
                }
            }
        }
       
     stage('DeployToProduction') {
            steps {
                kubeconfig(caCertificate: 'IIDBjCCAe6gAwIBAgIBATANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwptaW5pa3ViZUNBMB4XDTI1MDEwOTE5MTkyNVoXDTM1MDEwODE5MTkyNVowFTETMBEGA1UEAxMKbWluaWt1YmVDQTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAOzfCgCvBXagizjawIjFzld+yME31WEXw6bBFxPo1iwoe/OfgEvQ/q3fcwjdnJDOjO61JV3nnBbI/uYD2xuotKP+2h64zcsjjXXiYCZEXmf/iJtqTShD0aGgJ+3wbpIMJp0i3nAVJxHrszfzj/ahXlsQptggwA0ShFOL+b/c4opRiGlytjZYT39uxbTD3MEhsXiqc8dFssRe7BcnzDgTt2uBp20vn2t7iKqyx9rzPcnuWGjTC31iYFpZdEDxqcEOfDAWxjAZvdjjsVUnodwUYZ9FsTu16bq3/eDh0Kl/Q5b1jZENdajSmycCxDyfuQWrwxhT54OFl0946vpZgAbsAUcCAwEAAaNhMF8wDgYDVR0PAQH/BAQDAgKkMB0GA1UdJQQWMBQGCCsGAQUFBwMCBggrBgEFBQcDATAPBgNVHRMBAf8EBTADAQH/MB0GA1UdDgQWBBT8lMeLzKENoqKWsnpz5/ZWYp+evzANBgkqhkiG9w0BAQsFAAOCAQEAlyRx/wYVKRlPzbkSn07PJtcEh0WciezvqbQJLBaUjteSSvZzRT6qf9Yt8tW2QEbLH5vXY+CMJIY6GAWxHzmPaZKx9BEof7ITcciRSVkOQ9g1Qfuds7PJqbAMWBGRGUYAJ11j2cZasKHYaD9ktNp+vzm4mOTeBJuIhbnkSOdNlEiMEimC6eHDrKLmGCL2CzW+R2twRp5d5/xHZO2O7tDSQPzrFjnKpSh0ik0aqsPU2hIfQ56+bjts6zL3Xg50VMbbCFJb87zfSi8VwyPvAW+tx03tKqJzfg7eFkXfYZ0I1C0L6XC7uUZgPstkM2cDrWWm9wCmF8OqYFIni19EK0eOFQ==', credentialsId: 'config', serverUrl: 'https://192.168.49.2:8443') {
                 sh 'kubectl apply -f configmap-a.yaml'
                 sh 'kubectl apply -f deployment-a.yaml'
                 sh 'kubectl rollout restart deployment todo-mi'
}
            }
        }
 stage('Cleanup Artifacts') {
    steps {
        script {
            // Supprimer l'image avec la balise spécifiée
            sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
            
            // Supprimer l'image 'latest'
            sh "docker rmi ${IMAGE_NAME}:latest"
        }
    }
}


     }
     
    
}
