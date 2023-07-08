# Чеклист готовности к домашнему заданию
- Скачайте и установите актуальную версию terraform >=1.4.X . Приложите скриншот вывода команды terraform --version.
  ![](https://github.com/EVolgina/devops27-tf/blob/main/tfv.PNG)
- Скачайте на свой ПК данный git репозиторий. Исходный код для выполнения задания расположен в директории 01/src.
  ![]()
- Убедитесь, что в вашей ОС установлен docker.
```
  docker --version
  Docker version 20.10.21, build 20.10.21-0ubuntu1~22.04.3
```
# Задание 1
- Перейдите в каталог src. Скачайте все необходимые зависимости, использованные в проекте.
  ```
devops@WORKBOOK:~/ter-homeworks/01$ cd src
  ```
  - Изучите файл .gitignore. В каком terraform файле согласно этому .gitignore допустимо сохранить личную, секретную информацию?
ответ: personal.auto.tfvars
- Выполните код проекта. Найдите в State-файле секретное содержимое созданного ресурса random_password, пришлите в качестве ответа конкретный ключ и его значение.
  ```
devops@WORKBOOK:~/ter-homeworks/01/src$ terraform init
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

devops@WORKBOOK:~/ter-homeworks/01/src$ terraform validate
Success! The configuration is valid.
  ```
- Выполним terraform plan
```
devops@WORKBOOK:~/ter-homeworks/01/src$ terraform plan

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

- Раскомментируйте блок кода, примерно расположенный на строчках 29-42 файла main.tf. Выполните команду terraform validate. Объясните в чем заключаются намеренно допущенные ошибки? Исправьте их.
ответ:
![cor](https://github.com/EVolgina/devops27-tf/blob/main/corect.PNG)
```
devops@WORKBOOK:~/ter-homeworks/01/src$ terraform validate
Success! The configuration is valid.
```
- Выполните код. В качестве ответа приложите вывод команды docker ps
```

```
Замените имя docker-контейнера в блоке кода на hello_world, выполните команду terraform apply -auto-approve. Объясните своими словами, в чем может быть опасность применения ключа -auto-approve ? В качестве ответа дополнительно приложите вывод команды docker ps
Уничтожьте созданные ресурсы с помощью terraform. Убедитесь, что все ресурсы удалены. Приложите содержимое файла terraform.tfstate.
Объясните, почему при этом не был удален docker образ nginx:latest ? Ответ подкрепите выдержкой из документации провайдера.
