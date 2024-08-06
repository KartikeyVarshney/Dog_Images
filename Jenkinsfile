node{
    def gitUrl = 'https://github.com/KartikeyVarshney/Dog_Images.git'
    def branch = 'main'
    // def credentialsId = '78f52c88-4ff7-4b93-9637-fff00e450f4a'
    def docker_credentials = 'a87a3ab6-81e5-432b-a33b-15cce363f863'
    def nexusUrl = 'http://localhost:8081/repository/Dog_Image/'
    stage('Clone repository')
    {
        try
        {
            // Checkout the git repository using the creditials
            git branch: branch , url: gitUrl
        }
        catch (Exception e)
        {
            throw e
        }
    }
    stage('Building Docker Image')
    {
        sh 'docker build -t dog-image:latest .'
        echo "build succesfully..."
    }
    stage('push image to nexus')
    {
        withCredentials([usernamePassword(credentialsId: docker_credentials , usernameVariable : 'USERNAME' , passwordVariable: 'PASSWORD')])
        {
            sh "echo $PASSWORD |docker login ${nexusUrl} --username $USERNAME --password-stdin"
            sh "docker tag dog-image:latest"
            sh "docker push ${nexusUrl}"
            echo "Image pushed to nexus repo..."
        }
    }
    stage('Deploy')
    {
        sh 'kubectl apply -f deployment.yaml'
        sh 'kubectl apply -f service.yaml'
    }

}
