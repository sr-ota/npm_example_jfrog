node {
    def server = Artifactory.server "jfrogeval"
    def rtNpm = Artifactory.newNpmBuild()
    def buildInfo
    
    stage ('Artifactory configuration') {
        rtNpm.resolver repo: 'default-npm-virtual', server: server
        buildInfo = Artifactory.newBuildInfo()
    }
    
    stage ('Git Clone') {
        git url: 'https://github.com/sr-ota/jfrog_py_example', branch: 'main'
    }
    
    stage ('Package and create distribution archives') {
        rtNpm.tool = 'latest'
        sh 'ls'
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
