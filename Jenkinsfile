def CONTAINER_NAME="store-management"
def CONTAINER_TAG="latest"

pipeline {
    agent any
    stages {
        stage('build') {
            steps {
                sh 'mvn verify'
            }
        }
        stage ('Test') {  
            steps{
                sh 'mvn test'
                    echo "test successful";
                } 
        }
        stage("Image Prune"){
            imagePrune(CONTAINER_NAME)
        }
        stage('Image Build'){
            imageBuild(CONTAINER_NAME, CONTAINER_TAG)
        }
    
    }
    post {
        always {
            archiveArtifacts 'target/*.jar'
            junit 'target/*reports/*.xml'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
   
}

def imagePrune(containerName){
    try {
        sh "docker image prune -f"
        sh "docker stop $containerName"
    } catch(error){}
}

def imageBuild(containerName, tag){
    sh "docker build -t $containerName:$tag  -t $containerName --pull --no-cache ."
    echo "Image build complete"
}
