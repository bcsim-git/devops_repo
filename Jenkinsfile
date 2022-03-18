pipeline {
      agent any
      stages {
          stage('One') {
          steps {
            echo 'Begin of Pipeline: Stage one completes'
          }
          }
          stage('Two') {
          steps {
            input('Do you want to update to Development container?')
          }
          }
          stage('Three') {
          when {
                not {
                    branch "Development NOT updated"
                }
          }
          steps {
                 echo "Development container updated"
                 sh '''#!/bin/bash
                 bolt plan run module_web::plan_web_update -t puppetclient1 -u clientadm -p user123 --no-host-key-check --run-as root
                 '''
          }
          }
          stage('Four') {
          steps {
            input('Do you want to update to Production container: Proceed to Production')
                
          }
          }
          stage('Five') {
          when {
                not {
                    branch "Production NOT updated"
                }
          }
          steps {
                 echo "Prodcution container updated"
                 sh '''#!/bin/bash
                 bolt plan run module_web::plan_web_update -t puppetclient2 -u clientadm -p user123 --no-host-key-check --run-as root
                 '''

          }
          }



      }
}
