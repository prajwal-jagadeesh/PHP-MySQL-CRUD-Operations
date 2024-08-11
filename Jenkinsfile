pipeline {
    agent any
    
    environment {
        // Define environment variables
        DB_HOST = 'phpdb.cp4oyg4e6tsj.ap-south-1.rds.amazonaws.com'
        DB_USER = 'admin'
        DB_PASSWORD = 'admin123'
        DB_NAME = 'phpdb'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Pull the latest code from the repository
                git 'https://github.com/kunalbagnial/PHP-MySQL-CRUD-Operations.git'
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t php-mysql-crud .'
                }
            }
        }
        
        stage('Deploy Docker Container') {
            steps {
                script {
                    // Stop and remove existing container if it exists
                    sh '''
                    docker stop php-mysql-crud || true
                    docker rm php-mysql-crud || true
                    '''
                    
                    // Run the Docker container
                    sh '''
                    docker run -d -p 8080:80 \
                      -e DB_HOST=${DB_HOST} \
                      -e DB_USER=${DB_USER} \
                      -e DB_PASSWORD=${DB_PASSWORD} \
                      -e DB_NAME=${DB_NAME} \
                      --name php-mysql-crud php-mysql-crud
                    '''
                }
            }
        }
        
        stage('Cleanup') {
            steps {
                script {
                    // Optionally clean up old Docker images
                    sh '''
                    docker image prune -f
                    '''
                }
            }
        }
    }
    
    post {
        success {
            echo 'Deployment was successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
        always {
            // Clean up workspace if necessary
            cleanWs()
        }
    }
}
