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
        stage("Generate Artifacts"){
            steps{
                echo "====++++executing Generate Artifacts++++===="
                rtServer (
                    id: 'ArtifactoryLocal',
                    url: 'http://localhost:8081/artifactory',
                    // // If you're using username and password:
                    // username: 'user',
                    // password: 'password',
                    // If you're using Credentials ID:
                    credentialsId: 'artifactoryLocal',
                    // // If Jenkins is configured to use an http proxy, you can bypass the proxy when using this Artifactory server:
                    // bypassProxy: true,
                    // Configure the connection timeout (in seconds).
                    // The default value (if not configured) is 300 seconds:
                    timeout: 300
                )
                rtMavenDeployer (
                    id: 'ProjectDeployer',
                    releaseRepo: 'local',
                    snapshotRepo: 'local',
                    serverId: 'ArtifactoryLocal'
                )
            }
            post{
                always{
                    echo "====++++always++++===="
                }
                success{
                    echo "====++++Generate Artifacts executed successfully++++===="
                }
                failure{
                    echo "====++++Generate Artifacts execution failed++++===="
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