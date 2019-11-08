.DEFAULT: all
.PHONY: up down test clean all deps ssh

export ANSIBLE_HOST_KEY_CHECKING=false
export ANSIBLE_SSH_ARGS='-o UserKnownHostsFile=/dev/null -o ControlMaster=auto -o ControlPersist=60s'

ANSIBLE_OPTS:=
ANSIBLE_OPTS+= --private-key=~/.vagrant.d/insecure_private_key
ANSIBLE_OPTS+= --user=vagrant
#ANSIBLE_OPTS+= -vvvv

all: up deps test

up:
	vagrant up

deps:
	cd provision && ansible-galaxy install -r roles/requirements.yml

test:
	cd provision && ansible-playbook $(ANSIBLE_OPTS) playbook.yml

down:
	vagrant destroy -f

clean: down
	find provision/roles -mindepth 1 -maxdepth 1 -type d | xargs rm -rf

ssh:
	vagrant ssh k8s-node-0
