node {
    def gitUrl = 'https://github.com/gem-Gungun/Dog_Images.git'
    def branch = 'main'
    def credentialsId = 'ca4e208b-871a-4f05-a4c6-5e6b046dad30'
    def nexus_credentials = 'c81f87fb-6f07-42b8-9b76-569b59b55e98'
    def nexusUrl = 'http://localhost:8081/repository/Dog_Images/'
    
    stage('Clone repository') {
        try {
            // Checkout the git repository using the credentials
            git branch: branch, url: gitUrl, credentialsId: credentialsId
        } catch (Exception e) {
            throw e
        }
    }
    
    stage('Building Docker Image') {
        bat 'docker build -t dog-image:latest .'
        echo "Build successfully..."
    }
    
    stage('Publish image to Nexus') {
        withDockerRegistry([credentialsId: nexus_credentials, url: nexusUrl]) {
            bat 'docker tag dog-image:latest ${nexusUrl}/dog-image:latest'
            bat 'docker push ${nexusUrl}/dog-image:latest'
            echo "Image published to Nexus repo..."
        }
    }
    
    stage('Deploy') {
        bat 'kubectl apply -f deployment.yaml'
        bat 'kubectl apply -f service.yaml'
    }
}
