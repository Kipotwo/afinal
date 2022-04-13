pipeline {
    // Aron Yang, 2022/04/13, Pipeline made for java app (sample 3)
    agent any
    parameters {
        booleanParam(name:'RUN', 
        defaultValue: false, 
        description: 'Whether to Run the Code')

        string(name:'BUILD_TYPE', 
        defaultValue: 'Development', 
        description: 'Type of Build ')
    }
    stages{
        stage('Build'){
            steps{
                sh 'echo "Starting Java Build..."'
                sh 'mvn -B -DskipTests clean install'
                sh 'echo "Java Build Complete"'
            }
        }

        stage('Code Quantity'){
            steps{
                script{
                    def app = findFiles(glob: '**/App.java')
                    for (i in app) {
                        sh "wc -l ${i}"
                    }
                }
                sh 'echo "Student Number: a01231482, Group number: 43"'
            }
        }

        stage('Test'){
            when {
                expression{
                    params.BUILD_TYPE == "Development"
                }
            }
            steps{
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Run'){
            when{
                expression{
                    params.RUN
                }
            }
            steps{
                sh 'chmod 777 ./deliver.sh'
                sh './deliver.sh'
            }
        }

        stage('Build Results'){
            steps{
                script{
                    params.each() { p, value ->
                        if (${p} == "BUILD_TYPE"){
                            sh 'echo "Build ${value} completed successfully"'
                        }
                    }
                }
                // sh 'echo "Build ${params.BUILD_TYPE} completed successfully"'
                sh 'echo "I have now completed ACIT 4850!"'
            }
        }

    }
}