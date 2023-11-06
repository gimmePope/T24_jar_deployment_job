pipeline {
    agent any

    environment {
        inventory_file="/home/otulanaio/ansible_dev_files/cba_inventory.txt"
        playbook_path="/home/otulanaio/.jenkins/workspace/general_test_pipeline_with_file_demo"
        ansible_path="/home/otulanaio/.local/bin/"
    }
    
    parameters{
       string(name: 'JAR_NAME', defaultValue: 'application', description: 'Specify the JAR name')
       choice(name:'context', choices: ['sbnatm','sbnweb', 'sbntsa', 'sbnintf'])
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
                    ${ansible_path}ansible-playbook -i ${inventory_file}  ${playbook_path}/deploy_jar.yml -e "jar_name=${JAR_NAME}.jar"
                    '''
                    
                    script{
                        sh '''
                            if [ \$? -eq 0 ]; then
                                echo "jar deployment on course  for ${context}!!!"
                            else
                               echo "issues deteched"
                            fi
                        '''
                    }
            
            }
        }

    }

     post{
        always{
            sh 'echo "AM always on"'
            
        }
        failure{
            sh 'echo "I will show if build run fails"'
        }
        success{
            sh 'echo "I am associated with success"'
            emailext (attachLog: true, body: '''Dear Team,

Please note that  ${JAR_NAME}.jar has been successful deployed on T24 application servers and requires a restart of ${context} context, kindly expedite action.

Best Regards,''', subject: 'Successful deployment of ${JAR_NAME}.jar, requires action', to: 'jimiotulana@gmail.com')
        }
    }


}
