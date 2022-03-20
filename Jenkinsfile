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
                 sh '''#!/bin/bash
                 targets=puppetclient1;
                 docker cp /testdir/tasks/script_to_run $targets://testdir/tasks/script_to_run;
                 bolt script run '/testdir/tasks/script_to_run' -t $targets -u clientadm -p user123 --no-host-key-check --run-as root;
                 '''
                 echo "Development container updated"
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
                 sh '''#!/bin/bash
                 targets=puppetclient2;
                 docker cp /testdir/tasks/script_to_run $targets://testdir/tasks/script_to_run;
                 bolt script run '/testdir/tasks/script_to_run' -t $targets -u clientadm -p user123 --no-host-key-check --run-as root
                 '''
                 echo "Prodcution container updated"
          }
          }
          stage('Completed updating Operation') {
          steps {
            echo 'Completed updating to Production Container'
          }
          }
      }
}
