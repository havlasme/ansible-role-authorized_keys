all: syntax functional idempotency acceptance

syntax:
	ansible-playbook test.yml -i inventory --syntax-check

functional:
	ansible-playbook test.yml -i inventory -e "provision_docker__container_state=reloaded" --skip-tags=acceptance-test\
	&& (echo "Functional test: PASS" && exit 0)\
	|| (echo "Functional test: FAIL" && exit 1)

idempotency:
	unbuffer ansible-playbook test.yml -i inventory --skip-tags=pre-test,acceptance-test | tee /tmp/ansible_idempotence_test.txt\
	&& grep -E "test_authorized_keys_01_centos7.*changed=0.*unreachable=0.*failed=0" /tmp/ansible_idempotence_test.txt\
	&& grep -E "test_authorized_keys_01_debian8.*changed=0.*unreachable=0.*failed=0" /tmp/ansible_idempotence_test.txt\
	&& (echo "Idempotence test: PASS" && exit 0)\
	|| (echo "Idempotence test: FAIL" && exit 1)

acceptance:
	ansible-playbook test.yml -i inventory --tags=initialize-docker,acceptance-test\
	&& (echo "Acceptance test: PASS" && exit 0)\
	|| (echo "Acceptance test: FAIL" && exit 1)

clean:
	ansible-playbook test.yml -i inventory -e "provision_docker__container_state=absent" --tags=initialize-docker
