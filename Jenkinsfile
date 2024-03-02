pipeline{
    agent any
    tools{
        maven "maven"
    }
    stages{

        stage("SCM checkout"){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Application4/jenkins-demo.git']])
            }
        }

        stage("MVN Build"){
            steps{
                script{
                    sh 'mvn clean install'
                }
            }
        }

        stage("Deploy to tomcat"){
            steps{
                deploy adapters: [tomcat9(credentialsId: 'tomcat-pwd', path: '', url: 'http://localhost:9090/')], contextPath: 'jenkinsCiCd', war: '**/*.war'
            }
        }

    }

    post{
        always{
            emailext body: '''<html>
                                <body>
                                    <p>Build Status: ${BUILD_STATUS}</p>
                                    <p>Build Number: ${BUILD_NUMBER}</p>
                                    <p>Check the <a href="${BUILD_URL}">console output</a>.</p>
                                </body>
                            </html>''', compressLog: true, mimeType: 'text/html', replyTo: 'javatechie.learning@gmail.com', subject: 'Pipeline Status: ${BUILD_NUMBER}', to: 'javatechie.learning@gmail.com'
        }
        success{
            echo 'Code checkout, Maven build and deployment completed successfully!'
        }
        failure{
            echo 'Code checkout, Maven build and deployment failed!'
        }

    }
}