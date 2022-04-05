node {
    def server = Artifactory.server "jfrogeval"
    def rtNpm = Artifactory.newNpmBuild()
    def buildInfo
    
    stage ('Artifactory configuration') {
        rtNpm.resolver repo: 'default-npm-virtual', server: server
        buildInfo = Artifactory.newBuildInfo()
        buildInfo.project = "x01" // jfrog project key
    }
    
    stage ('Git Clone') {
        git url: 'https://github.com/sr-ota/npm_example_jfrog', branch: 'main'
    }
    
    stage ('Package and create distribution archives') {
        rtNpm.tool = 'latest'
	      buildInfo = rtNpm.install path: '.'
    }

    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
    
    stage ('XRay') {
        xrayResult = xrayScan(
            serverId: "jfrogeval",
            failBuild: false
        )   
        println xrayResult
    }
}
