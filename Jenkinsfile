node("maven") {
  stage("Build JAR") {
    git url: "INVENTORY-GIT-URL"
    sh "mvn clean package"
    stash name:"jar", includes:"target/inventory-1.0-SNAPSHOT-swarm.jar"
  }

  stage("Build Image") {
    unstash name:"jar"
    sh "oc start-build inventory-s2i --from-file=target/inventory-1.0-SNAPSHOT-swarm.jar"
    openshiftVerifyBuild bldCfg: "inventory-s2i", waitTime: '20', waitUnit: 'min'
  }

  stage("Deploy") {
    openshiftDeploy deploymentConfig: inventory
  }
}
