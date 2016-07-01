all: main


main: main_syntax main_functional main_idempotency main_acceptance

main_syntax:
	ansible-playbook t1_main.yml -i inventory --syntax-check

main_functional:
	ansible-playbook t1_main.yml -i inventory -e "provision_docker__container_state=reloaded" --skip-tags=acceptance-test

main_idempotency:
	ansible-playbook t1_main.yml -i inventory --skip-tags=pre-test,acceptance-test | tee /tmp/ansible_idempotence_test.txt\
	&& grep -E 'test_authorized_keys_t1_centos7.*changed=0.*unreachable=0.*failed=0' /tmp/ansible_idempotence_test.txt\
	&& grep -E 'test_authorized_keys_t1_debian8.*changed=0.*unreachable=0.*failed=0' /tmp/ansible_idempotence_test.txt\
	&& (echo 'Idempotence test: PASS' && exit 0)\
	|| (echo 'Idempotence test: FAIL' && exit 1)

main_acceptance:
	ansible-playbook t1_main.yml -i inventory --tags=initialize-docker,acceptance-test


cleanall: clean_main

clean_main:
	ansible-playbook t1_main.yml -i inventory -e "provision_docker__container_state=absent" --tags=initialize-docker
