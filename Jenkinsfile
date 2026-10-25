pipeline{

    agent any
    tools{
        maven "maven"
    }

    stages{

        stage("SCM checkout"){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Shiv-Raut/Jenkins-cicd.git']])
            }
        }

        stage("Build Process"){
            steps{
                script{
                     bat 'mvn clean install'
                }
            }
        }

        stage("Deploy to Container"){
            steps{
               deploy adapters: [tomcat9(alternativeDeploymentContext: '', credentialsId: 'tomcat-pwd', path: '', url: 'http://localhost:9090/')], contextPath: 'jenkinsCiCd', war: '**/*.war'
            }
        }


    }
    post{
        always{
            emailext attachLog: true, body: '''<html>
    <body>
        <p>Build Status: ${BUILD_STATUS}</p>
        <p>Build Number: ${BUILD_NUMBER}</p>
        <p>Check the <a href="${BUILD_URL}">console output</a>.</p>
    </body>
</html>''', mimeType: 'text/html', replyTo: 'shivkumar.raut2597@gmail.com', subject: 'Pipeline status : ${BUILD_NUMBER}', to: 'shivkumar.raut2597@gmail.com'
        }
    }

}