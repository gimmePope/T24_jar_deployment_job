pipeline {
    agent any
    
    parameters{
       string(name: 'JAR_NAME', defaultValue: 'application', description: 'Specify the JAR name')

      stashedFile(name: "JAR_FILE", description: "provide the jar file to deploy")
         }
   

    stages {
        stage('Upload JAR') {
            steps {
                unstash 'JAR_FILE'
                sh 'mv JAR_FILE ./${JAR_NAME}.jar'
            }
        }
        
        stage('deploy jar to all required environment using ansible'){
            steps{
              sh  '''
            echo "Invoking ansible playbook to deploy ${JAR_NAME}.jar across all necessary servers"
            echo "deployment time is $(date)"
            '''
            }
        }

    }
}
