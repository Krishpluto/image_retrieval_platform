pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from the repository
                git 'https://github.com/skx6/image_retrieval_platform.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Install dependencies
                sh 'pip install -r requirements.txt'
            }
        }

        stage('Download Pretrained Model') {
            steps {
                // Download pretrained CNN model
                sh 'wget -P retrieval/models/ <link_to_pretrained_model>'
            }
        }

        stage('Run Python Script') {
            steps {
                // Run the Python script
                sh 'python image_retrieval_cnn.py'
            }
        }

        stage('Expose Output') {
            steps {
                // Start a simple HTTP server to expose the output
                sh 'python -m http.server 8080 &'
            }
        }
    }

    post {
        success {
            // Print message with the URL to access the output
            script {
                def serverIp = sh(script: "ifconfig | grep 'inet ' | grep -v '127.0.0.1' | awk '{print \$2}'", returnStdout: true).trim()
                echo "Access the output at: http://${serverIp}:8080/"
            }
        }
    }
}
