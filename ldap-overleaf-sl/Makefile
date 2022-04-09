include .env

build: 
	docker build --build-arg login_text="${LOGIN_TEXT}" \
		     --build-arg collab_text="${COLLAB_TEXT}" \
		     --build-arg admin_is_sysadmin="${ADMIN_IS_SYSADMIN}" \
		     -t "local/ldap-overleaf-base:3.0.1" ldap-overleaf-sl -f ldap-overleaf-sl/Dockerfile.base 
	docker build --build-arg login_text="${LOGIN_TEXT}" \
		     --build-arg collab_text="${COLLAB_TEXT}" \
		     --build-arg admin_is_sysadmin="${ADMIN_IS_SYSADMIN}" \
		     -t "local/ldap-overleaf-sl:3.0.1" ldap-overleaf-sl  -f ldap-overleaf-sl/Dockerfile

clean: check_clean
	docker-compose down
	docker volume prune 
	docker container prune 

check_clean:
	@echo -n "Are you sure? [y/N] " && read ans && [ $${ans:-N} = y ]


.PHONY: clean check_clean
