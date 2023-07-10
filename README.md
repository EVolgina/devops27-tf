# Чеклист готовности к домашнему заданию
- Скачайте и установите актуальную версию terraform >=1.4.X . Приложите скриншот вывода команды terraform --version.
```
  terraform --version
Terraform v1.4.6
on linux_amd64
+ provider registry.terraform.io/hashicorp/random v3.5.1
+ provider registry.terraform.io/kreuzwerker/docker v3.0.2
Your version of Terraform is out of date! The latest version
is 1.5.2. You can update by downloading from https://www.terraform.io/downloads.html
```
- Скачайте на свой ПК данный git репозиторий. Исходный код для выполнения задания расположен в директории 01/src.
 git clone https://github.com/netology-code/ter-homeworks.git 
- Убедитесь, что в вашей ОС установлен docker.
```
  docker --version
  Docker version 24.0.4, build 3713ee1
```
# Задание 1
- Перейдите в каталог src. Скачайте все необходимые зависимости, использованные в проекте.
- Изучите файл .gitignore. В каком terraform файле согласно этому .gitignore допустимо сохранить личную, секретную информацию?
### ответ: personal.auto.tfvars
- Выполните код проекта. Найдите в State-файле секретное содержимое созданного ресурса random_password, пришлите в качестве ответа конкретный ключ и его значение.

```
terraform init
Initializing the backend...
Initializing provider plugins...
- Finding kreuzwerker/docker versions matching "~> 3.0.1"...
- Finding latest version of hashicorp/random...
- Installing hashicorp/random v3.5.1...
- Installed hashicorp/random v3.5.1 (signed by HashiCorp)
- Installing kreuzwerker/docker v3.0.2...
- Installed kreuzwerker/docker v3.0.2 (self-signed, key ID BD080C4571C6104C)

Partner and community providers are signed by their developers.
If you'd like to know more about provider signing, you can read about it here:
https://www.terraform.io/docs/cli/plugins/signing.html
Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.

~/ter-homeworks/01/src$ terraform validate
Success! The configuration is valid.
```
- Выполним terraform plan
```
~/ter-homeworks/01/src$ terraform plan

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the
following symbols:
  + create
Terraform will perform the following actions:
  # random_password.random_string will be created
  + resource "random_password" "random_string" { 
      + bcrypt_hash = (sensitive value)
      + id          = (known after apply)
      + length      = 16
      + lower       = true
      + min_lower   = 1
      + min_numeric = 1
      + min_special = 0
      + min_upper   = 1
      + number      = true
      + numeric     = true
      + result      = (sensitive value)
      + special     = false
      + upper       = true
    }

Plan: 1 to add, 0 to change, 0 to destroy.
Note: You didn't use the -out option to save this plan, so Terraform can't guarantee to take exactly these actions if
you run "terraform apply" now.
```
![res](https://github.com/EVolgina/devops27-tf/blob/main/result.PNG)

- Раскомментируйте блок кода, примерно расположенный на строчках 29-42 файла main.tf. Выполните команду terraform validate. Объясните в чем заключаются намеренно допущенные ошибки? Исправьте их.
### ответ:
![cor](https://github.com/EVolgina/devops27-tf/blob/main/corect.PNG)
```
~/ter-homeworks/01/src$ terraform validate
Success! The configuration is valid.
```
- Выполните код. В качестве ответа приложите вывод команды docker ps
```
Plan: 3 to add, 0 to change, 0 to destroy.
Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.
  Enter a value: yes
docker_image.nginx_image: Creating...
random_password.random_string: Creating...
random_password.random_string: Creation complete after 1s [id=none]
docker_image.nginx_image: Still creating... [10s elapsed]
docker_image.nginx_image: Still creating... [20s elapsed]
docker_image.nginx_image: Still creating... [30s elapsed]
docker_image.nginx_image: Still creating... [40s elapsed]
docker_image.nginx_image: Creation complete after 48s [id=sha256:021283c8eb95be02b23db0de7f609d603553c6714785e7a673c6594a624ffbdanginx:latest]
docker_container.nginx_container: Creating...
docker_container.nginx_container: Creation complete after 3s [id=7e1004caf43534697cd966c1c16d6ce7676d981364667eefdf5c27a5dbb7c84c]

Apply complete! Resources: 3 added, 0 changed, 0 destroyed.
vagrant@sysadm-fs:~/tf/ter-homeworks/01/src$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                  NAMES
7e1004caf435   021283c8eb95   "/docker-entrypoint.…"   2 minutes ago   Up 2 minutes   0.0.0.0:8000->80/tcp   example_DYBXpA4vuU1tVisZ

```
- Замените имя docker-контейнера в блоке кода на hello_world, выполните команду terraform apply -auto-approve. Объясните своими словами, в чем может быть опасность применения ключа -auto-approve ? В качестве ответа дополнительно приложите вывод команды docker ps
### Ответ: флаг -auto-approve указывает Terraform автоматически утверждать и применять любые изменения, не требуя подтверждения вручную. Функция автоматического утверждения исключает возможность проверки и валидации вручную. Это означает, что любые неправильно сконфигурированные или небезопасные изменения могут быть автоматически применены без проверки, потенциально ставя под угрозу безопасность инфраструктуры
```
docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```
- Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены. Приложите содержимое файла terraform.tfstate.
```
terraform destroy
random_password.random_string: Refreshing state... [id=none]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the
following symbols:
  - destroy
Terraform will perform the following actions:
  # random_password.random_string will be destroyed
  - resource "random_password" "random_string" {
      - bcrypt_hash = (sensitive value) -> null
      - id          = "none" -> null
      - length      = 16 -> null
      - lower       = true -> null
      - min_lower   = 1 -> null
      - min_numeric = 1 -> null
      - min_special = 0 -> null
      - min_upper   = 1 -> null
      - number      = true -> null
      - numeric     = true -> null
      - result      = (sensitive value) -> null
      - special     = false -> null
      - upper       = true -> null
    }
Plan: 0 to add, 0 to change, 1 to destroy.
Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.
  Enter a value: yes
random_password.random_string: Destroying... [id=none]
random_password.random_string: Destruction complete after 0s
Destroy complete! Resources: 1 destroyed.
```
![st](https://github.com/EVolgina/devops27-tf/blob/main/state.PNG)
- Объясните, почему при этом не был удален docker образ nginx:latest ? Ответ подкрепите выдержкой из документации провайдера.
### Ответ: Keep_locally - (Необязательно, логическое значение) 
Если true, то образ Docker не будет удален при операции уничтожения.
Если это ложь, он удалит изображение из локального хранилища докера при операции уничтожения.
Образ nginx:latest Docker не будет автоматически удален командой terraform destroy.
![keep]()
