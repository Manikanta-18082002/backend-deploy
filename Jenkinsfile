pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        disableConcurrentBuilds() // No Multiple  Builds
        ansiColor('xterm')
    }
     parameters {
        string(name: 'appVersion', defaultValue: '1.0.0', description: 'What is the application version?')
    }
    environment{
        def appVersion = '' // variable declaration
        nexusUrl = 'nexus.dawsmani.site:8081'
    }
    stages {
        stage ('print the version'){
            steps{ // Variable can be accessed with in that stage only
                script{ // Groovy Script
                // def packageJson = readJSON file: 'package.json'
                // appVersion = packageJson.version
                // echo "application version: $appVersion"
                echo "Application version: ${params.appVersion}"
                }
            }
        }
        stage('Init'){ //
            steps{
               sh """
                cd terraform
                terraform init
               """
            } // Now we are not in terraform folder? -- After stage done it comes into root folder (backend-deploy)
        }
        stage('Plan'){ 
            steps{ // Why cd again -- by default we are in root folder
               sh """
                pwd
                cd terraform 
                terraform plan -var="app_version=${params.appVersion}"  
               """
            } // Sending appVersion to terraform that will create instance
        }
        // stage('Deploy'){ 
        //     steps{
        //        sh """
        //         cd terraform
        //         terraform Deploy
        //        """
        //     }
        // }
    }
    post {  //This will catch the event and send Alerts to Mail/Slack
        always { 
            echo 'I will always say Hello again!'
            deleteDir()
        }
        success { 
            echo 'I will run when pipeline is success'
        }
        failure { 
            echo 'I will run when pipeline is failure'
        }
    }
}