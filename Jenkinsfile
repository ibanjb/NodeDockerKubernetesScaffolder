pipeline {
    agent any
    stages {
        stage('DSU Bitbucket') {
            steps {
                echo "Running build ${env.BUILD_ID} on ${env.JENKINS_URL} due commit ${env.GIT_COMMIT}"
                sh("git config remote.origin.url ssh://git@52.166.71.39:7999/ciam/your_project.git")
            }
        }
        stage('Build Docker image') {
            steps {   
                sh("docker build -t your_project:${env.GIT_COMMIT} .")
            }
        }
        stage('Deliver for development') {
            when {
                not {
                    branch 'master'
                }
            }
            stages {
                stage('Create Docker tag') {
                    steps {
                        sh("docker tag your_project:${env.GIT_COMMIT} ${env.ACR_URL}/your_project:dev${env.GIT_COMMIT}")
                    }
                }
                stage('Push Docker image into ACR') {
                    steps {
                        // sh("az acr login --name CoreContainer")
                        sh("docker login ${env.ACR_URL} -u ${env.ACR_NAME} -p ${env.ACR_PASSWORD}")
                        sh("docker push ${env.ACR_URL}/your_project:dev${env.GIT_COMMIT}")
                    }
                }
                stage('Update AKS') {
                    steps {           
                        sh ("kubectl set image deploy/your_project your_project=${env.ACR_URL}/your_project:dev${env.GIT_COMMIT} --namespace=development")
                    }
                }
                stage('Delete local images for development') {
                    steps {   
                        //sh("docker rmi your_project:${env.GIT_COMMIT}")
                        //sh("docker rmi ${env.ACR_URL}/your_project:dev${env.GIT_COMMIT}")
                    }
                }
            }
        }
        stage('Deliver for production') {
            when {
                branch 'master'
            }
            stages {
                stage('Create Docker tag') {
                    steps {
                        sh("docker tag your_project:${env.GIT_COMMIT} ${env.ACR_URL}/your_project:pro${env.GIT_COMMIT}")
                    }
                }
                stage('Push Docker image into ACR') {
                    steps {
                        sh("docker login ${env.ACR_URL} -u ${env.ACR_NAME} -p ${env.ACR_PASSWORD}")
                        sh("docker push ${env.ACR_URL}/your_project:pro${env.GIT_COMMIT}")
                    }
                }
                stage('Update AKS') {
                    steps {           
                        sh ("kubectl set image deploy/your_project your_project=${env.ACR_URL}/your_project:pro${env.GIT_COMMIT} --namespace=production")
                    }
                }
                stage('Delete local images for production') {
                    steps {   
                        // sh("docker rmi your_project:${env.GIT_COMMIT}")
                        //sh("docker rmi ${env.ACR_URL}/your_project:pro${env.GIT_COMMIT}")
                    }
                }
            }
        }        
    }
}