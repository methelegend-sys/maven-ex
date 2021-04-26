pipeline{
    agent any
    // tools {
    //     maven 'Maven 3.8.1'
    //     jdk 'jdk8'
    // }
    stages{
        stage("Initialize"){
            steps{
                echo "========executing Initialize========"
                git 'https://github.com/ankur198/maven-example'
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========Initialize executed successfully========"
                }
                failure{
                    echo "========Initialize execution failed========"
                }
            }
        }
        stage("Build"){
            steps{
                echo "====++++executing Build++++===="
                bat "mvn clean install package"
                
            }
            post{
                always{
                    echo "====++++always++++===="
                }
                success{
                    echo "====++++Build executed successfully++++===="
                }
                failure{
                    echo "====++++Build execution failed++++===="
                }
        
            }
        }
    }
    post{
        always{
            echo "========always========"
        }
        success{
            echo "========pipeline executed successfully ========"
        }
        failure{
            echo "========pipeline execution failed========"
        }
    }
}