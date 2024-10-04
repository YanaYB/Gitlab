
                            *Инструкция по запуску gitlab*
1.Запустите докер файл : docker-compose up -d
2. GitLab будет доступен по адресу: http://localhost:80. Запуск может занять некоторое время. Если GitLab не доступен, перезапустите контейнер: docker restart gitlab-ce
3. Для авторизации используйте логин root. Чтобы сгенерировать пароль, выполните команду: docker exec -it gitlab-ce grep 'Password:' /etc/gitlab/initial_root_password/.

                             *Настройка gitlab-runner*
1. Зарегистрируйте Runner командой:docker exec -it gitlab-runner gitlab-runner register
2. URL GitLab: http://172.19.0.2 (или ваш фактический URL GitLab)
3. Token: ваш токен (его можно найти на странице настройки Runners в вашем проекте GitLab)
4. Executor: docker
5. Image: docker: dind
После этого в файле config.toml измените следующие параметры:
volumes = ["/var/run/docker.sock:/var/run/docker.sock", "/cache"]
network_mode = "gitlab-network"

                             *Container registry*

registry_external_url 'http://172.19.0.2'
gitlab_rails['registry_enabled'] = true
gitlab_rails['registry_host'] = "172.19.0.2"

registry['enable'] = true
registry['registry_http_addr'] = "0.0.0.0:5000"  
registry['log_directory'] = "/var/log/gitlab/registry"
registry['env_directory'] = "/opt/gitlab/etc/registry/env"
registry_nginx['enable'] = false  




