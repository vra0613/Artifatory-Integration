pipeline {
    agent any

    environment {
        ARTIFACTORY_SERVER = 'artifactory'
        ARTIFACTORY_REPO = 'maven-repo'
        ARTIFACTORY_CRED = credentials('Artifactory Credentials')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/vra0613/Artifatory-Integration.git'
            }
        }

        stage('Build') {
            steps {
                // Example: Building a Maven project
                sh 'mvn clean install'
            }
        }

        stage('Publish to Artifactory') {
            steps {
                script {
                    def server = Artifactory.server(ARTIFACTORY_SERVER)
                    def uploadSpec = """{
                        "files": [
                            {
                                "pattern": "target/*.jar",
                                "target": "${ARTIFACTORY_REPO}/"
                            }
                        ]
                    }"""
                    server.upload(uploadSpec)
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
