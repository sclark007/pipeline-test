#!/usr/bin/env groovy

/**
 * Jenkinsfile for Chef cookbook for EOS
 * from https://github.com/aristanetworks/chef-eos/edit/develop/Jenkinsfile
 */

node('master') {

    currentBuild.result = "SUCCESS"

    try {

        stage ('Checkout') {

            checkout scm
            sh """
                cat README.md
            """
        }

        stage ('Cleanup') {

            echo 'Cleanup'

            step([$class: 'WarningsPublisher',
                  canComputeNew: false,
                  canResolveRelativePaths: false,
                  consoleParsers: [
                                   [parserName: 'Rubocop'],
                                   [parserName: 'Foodcritic']
                                  ],
                  defaultEncoding: '',
                  excludePattern: '',
                  healthy: '',
                  includePattern: '',
                  unHealthy: ''
            ])

           mail body: "${env.BUILD_URL} build successful.\n" +
                      "Started by ${env.BUILD_CAUSE}",
                from: 'eosplus-dev+jenkins@arista',
                replyTo: 'eosplus-dev@arista',
                subject: "Chef-eos ${env.JOB_NAME} (${env.BUILD_NUMBER}) build successful",
                to: 'steve@bigsteve.us'

        }

    }

    catch (err) {

        currentBuild.result = "FAILURE"

            mail body: "${env.JOB_NAME} (${env.BUILD_NUMBER}) cookbook build error " +
                       "is here: ${env.BUILD_URL}\nStarted by ${env.BUILD_CAUSE}" ,
                 from: 'jenkins+steve@bigsteve.us',
                 replyTo: 'jenkins+steve@bigsteve.us',
                 subject: "Chef ${env.JOB_NAME} (${env.BUILD_NUMBER}) build failed",
                 to: 'steve@bigsteve.us'

            throw err
    }

}
