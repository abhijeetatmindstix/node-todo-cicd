@NonCPS
def cancelPreviousBuilds() {
    def jobName = "docker-pipeline"
    def buildNumber = env.BUILD_NUMBER.toInteger()
    def branchName = "docker-pipeline"

    /* Get job name */
    log.info("cancelPreviousBuilds: ${buildNumber}")
    def currentJob = Jenkins.instance.getItemByFullName(jobName)

    /* Iterating over the builds for specific job */
    for (def build : currentJob.builds) {
        /* If there is a build that is currently running, it's not current build, and it's on the same branch */
        if (build.isBuilding() && build.number.toInteger() != buildNumber && build.getEnvironment().BRANCH_NAME == branchName) {
            /* Than stopping it */
            build.doStop()
        }
    }
}

// this is scripted pipeline
node('built-in') { // use whatever node name or label you want
    stage('Cancel older builds') {
        cancelPreviousBuilds()
    }
}



pipeline {
    agent any
    environment {
        registry = "675391935708.dkr.ecr.ap-south-1.amazonaws.com/aquila"
    }
   
    stages {
        stage("timeout") {
            steps {
                timeout(time: 10, unit: 'MINUTES') {
                    input message: "does this look good"
                }
            }
        }
    }
}
