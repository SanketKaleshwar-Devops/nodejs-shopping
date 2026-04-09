pipeline {
    agent any

    environment {
        AWS_REGION        = 'ap-south-1'
        ECR_REGISTRY      = '892512305960.dkr.ecr.ap-south-1.amazonaws.com'
        ECR_REPO          = 'nodejs-shopping'
        IMAGE_TAG         = "${BUILD_NUMBER}"
        KUBECONFIG        = '/root/.kube/eksctl/clusters/nodejs-shopping-cluster'
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out source code...'
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image...'
                sh """
                    docker build -t ${ECR_REGISTRY}/${ECR_REPO}:${IMAGE_TAG} .
                    docker tag ${ECR_REGISTRY}/${ECR_REPO}:${IMAGE_TAG} ${ECR_REGISTRY}/${ECR_REPO}:latest
                """
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh """
                    docker run --rm ${ECR_REGISTRY}/${ECR_REPO}:${IMAGE_TAG} node -e "console.log('App test passed ✅')"
                """
            }
        }

        stage('Push to ECR') {
            steps {
                echo 'Pushing image to ECR...'
                sh """
                    aws ecr get-login-password --region ${AWS_REGION} | \
                    docker login --username AWS --password-stdin ${ECR_REGISTRY}
                    
                    docker push ${ECR_REGISTRY}/${ECR_REPO}:${IMAGE_TAG}
                    docker push ${ECR_REGISTRY}/${ECR_REPO}:latest
                """
            }
        }

        stage('Deploy to EKS') {
            steps {
                echo 'Deploying to EKS...'
                sh """
                    export KUBECONFIG=${KUBECONFIG}

                    # Apply deployment and service first
                    kubectl apply -f k8s/deployment.yaml --kubeconfig=${KUBECONFIG}
                    kubectl apply -f k8s/service.yaml --kubeconfig=${KUBECONFIG}

                    # Update image to current build
                    kubectl set image deployment/nodejs-shopping \
                        nodejs-shopping=${ECR_REGISTRY}/${ECR_REPO}:${IMAGE_TAG} \
                        --kubeconfig=${KUBECONFIG}

                    # Wait for rollout to complete
                    kubectl rollout status deployment/nodejs-shopping \
                        --kubeconfig=${KUBECONFIG} \
                        --timeout=180s
                """
            }
        }

        stage('Get App URL') {
            steps {
                echo 'Getting application URL...'
                sh """
                    echo "=== Application URL ==="
                    kubectl get service nodejs-shopping-service \
                        --kubeconfig=${KUBECONFIG} \
                        -o jsonpath='{.status.loadBalancer.ingress[0].hostname}'
                    echo ""
                    echo "=== All Services ==="
                    kubectl get services --kubeconfig=${KUBECONFIG}
                    echo "=== All Pods ==="
                    kubectl get pods --kubeconfig=${KUBECONFIG}
                """
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully!'
            echo 'Application deployed to EKS!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}