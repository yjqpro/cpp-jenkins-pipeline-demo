pipeline {
   agent any
   stages {
    stage('linux') {
      steps {
          checkout scm: [
              $class: 'GitSCM',
              branches: scm.branches,
              doGenerateSubmoduleConfigurations: false,
              extensions: [[$class: 'SubmoduleOption',
                          disableSubmodules: false,
                          parentCredentials: false,
                          recursiveSubmodules: true,
                          reference: '',
                          trackingSubmodules: false]],
              submoduleCfg: [],
              userRemoteConfigs: scm.userRemoteConfigs
          ]

          cmakeBuild buildDir: '.build', buildType: 'Release', cleanBuild: true, 
                     cmakeArgs: '-DCMAKE_INSTALL_PREFIX=prefix',
                     installation: 'InSearchPath'
          // cpack installation: 'InSearchPath', workingDir: '.build'
          // archiveArtifacts '.build/xyz-web-master.tar.gz'
      }

      }
   }
}
