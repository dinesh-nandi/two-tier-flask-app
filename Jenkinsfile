pipeline {
    agent { label "dev" };

    stages {
        stage("Code") {
            steps {
                git url: "https://github.com/dinesh-nandi/two-tier-flask-app",branch: "master"
            }
        }
        stage("Trivy File System Scan"){
            steps{
                sh "trivy fs . -o results.json"
            }
        }
        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
            }
        }

        stage("Test") {
            steps {
                echo "Tester tests likh ke dega ...."
            }
        }
        
        stage("Push to Docker Hub"){
            steps{
                script{
                    docker_push("dockerHubCreds","two-tier-flask-app")
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker compose up -d --build flask-app"
            }
        }
    }
    post {
    success {
        emailext(
            subject: "Build Successful",
            body: "Good News: Your build was successful!",
            to: "dineshnandi6@gmail.com"
        )
    }

    failure {
        emailext(
            subject: "Build Failed",
            body: "Bad News: Your build has failed!",
            to: "dineshnandi6@gmail.com"
        )
    }
}
}
