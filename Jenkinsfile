pipeline{
    agent any
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
                    id: 'artifactory-server',
                    url: 'http://localhost:8081/artifactory',
                    credentialsId: 'artifactoryLocal',
                    timeout: 300
                )
                rtMavenDeployer (
                    id: 'MAVEN_DEPLOYER',
                    releaseRepo: 'demo-demoproject',
                    snapshotRepo: 'demo-demoproject',
                    serverId: 'artifactory-server'
                )
                // rtMavenResolver (
                //     id: "MAVEN_RESOLVER",
                //     serverId: "ARTIFACTORY_SERVER",
                //     releaseRepo: "local",
                //     snapshotRepo: "local"
                // )
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
                rtMavenRun (
                    tool: "Maven_3", // Tool name from Jenkins configuration
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",
                    // resolverId: "MAVEN_RESOLVER"
                )
                
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
                rtPublishBuildInfo (
                    serverId: "artifactory-server"
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
