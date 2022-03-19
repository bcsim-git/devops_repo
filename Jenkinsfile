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
                 bolt command run 'puppet resource package git ensure=present; puppet resource package vcsrepo ensure=present; puppet apply /root/pull_devops_repo/index_write' -t puppetclient1 -u clientadm -p user123 --no-host-key-check --run-as root;
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
                 bolt command run 'puppet resource package git ensure=present; puppet resource package vcsrepo ensure=present; puppet apply /root/pull_devops_repo/index_write' -t puppetclient2 -u clientadm -p user123 --no-host-key-check --run-as root;
                 '''
                 echo "Prodcution container updated"
          }
          }



      }
}
