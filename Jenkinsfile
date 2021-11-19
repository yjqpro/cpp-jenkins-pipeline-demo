pipeline {
   agent none
   stages {
       stage('parallel') {
        parallel {
            stage('linux') {
                    agent {
                            label 'ubuntu'
                    }
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
                                   installation: 'InSearchPath',
                                   steps: [[args: '--target install', withCmake: true]]
                        // cpack installation: 'InSearchPath', workingDir: '.build'
                        // archiveArtifacts '.build/xyz-web-master.tar.gz'
                    }

            }

            stage('windows') {
                    agent {
                            label 'windows2016'
                    }
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

                        bat label: '', script: '''cd vcpkg
                        bootstrap-vcpkg.bat && vcpkg.exe install @%WORKSPACE%\\vcpkg_x64-windows-static.txt'''

                        cmakeBuild buildDir: '.build', buildType: 'Release', cleanBuild: true, cmakeArgs: '-DCMAKE_TOOLCHAIN_FILE=${WORKSPACE}\\vcpkg\\scripts\\buildsystems\\vcpkg.cmake -DVCPKG_TARGET_TRIPLET=x64-windows-static -DCMAKE_INSTALL_PREFIX=prefix', generator: 'Visual Studio 15 2017 Win64', installation: 'InSearchPath', steps: [[withCmake: true]]
                    }
            }

        }
       }
   }
}
