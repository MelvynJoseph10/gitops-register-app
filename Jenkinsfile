pipeline {
    agent { label "Jenkins-Agent" }

    environment {
        APP_NAME = "register-app-pipeline"
        IMAGE_TAG = "1.0.0-13" // or dynamically set via a param/env var
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/MelvynJoseph10/gitops-register-app'
            }
        }

        stage("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's|${APP_NAME}:.*|${APP_NAME}:${IMAGE_TAG}|g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }

        stage("Push the changed deployment file to Git") {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                    sh """
                        git config --global user.name "MelvynJoseph10"
                        git config --global user.email "melvynjoseph69@gmail.com"
                        git add deployment.yaml
                        git commit -m "Updated Deployment Manifest" || echo "No changes to commit"
                        git push https://${GIT_USER}:${GIT_PASS}@github.com/MelvynJoseph10/gitops-register-app.git main
                    """
                }
            }
        }
    }
}
