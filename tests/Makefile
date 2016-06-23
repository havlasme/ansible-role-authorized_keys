all: main

main:
	# syntax check
	ansible-playbook main.yml -i inventory --syntax-check

	# functional test
	ansible-playbook main.yml -i inventory -e "provision_docker__container_state=reloaded" --skip-tags=acceptance-test

	# idempotence test
	ansible-playbook main.yml -i inventory --skip-tags=pre-test,acceptance-test | tee /tmp/ansible_idempotence_test.txt\
	&& grep -E 'test_authorized_keys_main_centos7.*changed=0.*unreachable=0.*failed=0' /tmp/ansible_idempotence_test.txt\
	&& grep -E 'test_authorized_keys_main_debian8.*changed=0.*unreachable=0.*failed=0' /tmp/ansible_idempotence_test.txt\
	&& (echo 'Idempotence test: PASS' && exit 0)\
	|| (echo 'Idempotence test: FAIL' && exit 1)

	# acceptance test
	ansible-playbook main.yml -i inventory --tags=initialize-docker,acceptance-test

cleanall:
	# remove main test docker containers
	ansible-playbook main.yml -i inventory -e "provision_docker__container_state=absent" --tags=initialize-docker
