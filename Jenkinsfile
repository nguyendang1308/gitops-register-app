pipeline {
    agent { label "Jenkins-Agent"}
    environment {
        APP_NAME = "register-app-pipeline"

    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps { 
                git branch: 'jenkins', credentialsId: 'github', url: 'https://github.com/nguyendang1308/gitops-register-app'
            }
        }

        stage("Update the Deloyment Tag") {
            steps {
                sh """ 
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                sh """ 
                    git config --global user.name "nguyendang1308"
                    git config --global user.email "nguyendang1308@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Manifest"
                """
                withCredential([gitUsernamePassword(credentialsId: 'github', gitTooName: 'Default')] {
                    sh "git push https://github.com/nguyendang1308/gitops-register-app master"
                })
            }
        }
    }
}