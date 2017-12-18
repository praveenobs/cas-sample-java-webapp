node {
    def server = Artifactory.newServer url: 'http://artifact.stage.shutterfly.com:8081/artifactory', credentialsId: 'ARTIFACTORYCREDENTIALS'
    def rtMaven = Artifactory.newMavenBuild()
    def buildInfo

    stage ('Clone') {
        git url: 'https://github.com/praveenobs/cas-sample-java-webapp.git'
    }

    stage ('Artifactory configuration') {
        rtMaven.tool = "Apache Maven 3.5.5" // Tool name from Jenkins configuration
        rtMaven.deployer releaseRepo: 'sbs-java-releases-local', snapshotRepo: 'sbs-java-snapshot-local', server: server
        rtMaven.resolver releaseRepo: 'sbs-java-releases-local', snapshotRepo: 'sbs-java-snapshot-local', server: server
        buildInfo = Artifactory.newBuildInfo()
    }

    stage ('Exec Maven') {
        rtMaven.run pom: 'pom.xml', goals: 'clean package', buildInfo: buildInfo
    }

    stage ('Publish build info') {
        server.publishBuildInfo buildInfo
    }
}
