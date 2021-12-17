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
                ssh label: '', script: 'mvn test'
                    echo "test successful";
                } 
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
