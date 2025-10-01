pipeline {
    agent { label 'contrôleur' }

    environment {
        KUBECONFIG = '/var/lib/jenkins/.kube/config'
    }

    stages {
        stage('Test cluster connection') {
            steps {
                echo 'Vérification de la connexion au cluster'
                sh 'kubectl get nodes'
            }
        }

        stage('Deploy Nginx Pod') {
            steps {
                echo 'Déploiement du pod Nginx'
                sh '''
                    cat <<EOF | kubectl apply -f -
                    apiVersion: v1
                    kind: Pod
                    metadata:
                      name: nginx-test
                      labels:
                        app: nginx-test
                    spec:
                      containers:
                      - name: nginx
                        image: nginx:alpine
                        ports:
                        - containerPort: 80
                    EOF
                '''
            }
        }

        stage('Check Pod status') {
            steps {
                echo 'Vérification du pod'
                sh 'kubectl get pods -l app=nginx-test'
            }
        }
    }
}
