pipeline{
    agent any
    tools {
        maven 'MAVEN_TOOL'
        // jdk 'jdk8'
    }
    stages{
        stage("Clone"){
            steps{
                echo "========executing Clone========"
                git 'https://github.com/ankur198/maven-example'
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========Clone executed successfully========"
                }
                failure{
                    echo "========Clone execution failed========"
                }
            }
        }
        stage("Initialize Artifactory"){
            steps{
                echo "====++++executing Initialize Artifactory++++===="
                rtServer (
                    id: 'ARTIFACTORY_SERVER',
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
                    id: 'MAVEN_DEPLOYER',
                    releaseRepo: 'local',
                    snapshotRepo: 'local',
                    serverId: 'ARTIFACTORY_SERVER'
                )
                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: "local",
                    snapshotRepo: "local"
                )
            }
            post{
                always{
                    echo "====++++always++++===="
                }
                success{
                    echo "====++++Initialize Artifactory executed successfully++++===="
                }
                failure{
                    echo "====++++Initialize Artifactory execution failed++++===="
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
        stage("Upload Artifacts"){
            steps{
                echo "====++++executing Upload Artifacts++++===="
                rtMavenRun (
                    tool: "MAVEN_TOOL", // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'deploy',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
                )
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
            }
            post{
                always{
                    echo "====++++always++++===="
                }
                success{
                    echo "====++++Upload Artifacts executed successfully++++===="
                }
                failure{
                    echo "====++++Upload Artifacts execution failed++++===="
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