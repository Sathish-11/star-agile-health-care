pipeline {
    agent { label 'k8s-master' }

    environment {
        DOCKER_IMAGE = "sathish1102/medical-app"
        DOCKER_TAG   = "${env.BUILD_NUMBER ?: 'latest'}"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'medicur-proj', url: 'https://github.com/Sathish-11/star-agile-health-care.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh '''
                        echo $PASS | docker login -u $USER --password-stdin
                        docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                    '''
                }
            }
        }

        stage('Update Deployment Files') {
            steps {
                sh '''
                    sed -i "s|${DOCKER_IMAGE}:.*|${DOCKER_IMAGE}:${DOCKER_TAG}|g" k8s/dev-deployment.yaml
                    sed -i "s|${DOCKER_IMAGE}:.*|${DOCKER_IMAGE}:${DOCKER_TAG}|g" k8s/stage-deployment.yaml
                    sed -i "s|${DOCKER_IMAGE}:.*|${DOCKER_IMAGE}:${DOCKER_TAG}|g" k8s/prod-deployment.yaml
                    echo "✅ Updated image references:"
                    grep -h "image:" k8s/*-deployment.yaml
                '''
            }
        }

        stage('Deploy to Dev') {
            steps {
                sh '''
                    kubectl apply -f k8s/dev-deployment.yaml --namespace=dev
                    echo "✅ Dev deployment completed successfully"
                '''
            }
        }

        stage('Deploy to Stage') {
            steps {
                sh '''
                    kubectl apply -f k8s/stage-deployment.yaml --namespace=stage
                    echo "✅ Stage deployment completed successfully"
                '''
            }
        }
        stage('Deploy to Prod') {
            steps {
                sh '''
                    kubectl apply -f k8s/prod-deployment.yaml --namespace=prod
                    echo "✅ Production deployment completed successfully"
                '''
            }
        }
    }

    post {
        success {
            echo "🎉 Pipeline completed successfully!"
        }
        failure {
            echo "❌ Pipeline failed. Check logs."
        }
        always {
            sh "docker logout"
            echo "Pipeline finished with status: ${currentBuild.result}"
        }
    }
}

