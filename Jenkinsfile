pipeline {
    agent any
   
    stages {

        stage('Run Selenium Tests with pytest') {
            steps {
                    echo "Running Selenium Tests using pytest"
                    bat 'pip install -r requirements.txt'
                    bat 'start /B python app.py'
                    bat 'ping 127.0.0.1 -n 5 > nul'
                    bat 'pytest -v'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Build Docker Image"
                bat "docker build -t hangmanapp:v1 ."
            }
        }
        stage('Docker Login') {
            steps {
                  bat 'docker login -u siri2409 -p AnjaliYash@2409'
                }
            }
        stage('push Docker Image to Docker Hub') {
            steps {
                echo "push Docker Image to Docker Hub"
                bat "docker tag hangmanapp:v1 siri2409/devopsassignment:hangmanapp"

                bat "docker push siri2409/devopsassignment:hangmanapp"

            }
        }
        stage('Deploy to Kubernetes') { 
            steps { 
                    // apply deployment & service 
                    bat 'kubectl apply -f deployment.yaml --validate=false' 
                    bat 'kubectl apply -f service.yaml' 
            } 
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Please check the logs.'
        }
    }
}