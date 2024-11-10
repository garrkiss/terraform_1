# Домашнее задание к занятию "`Введение в Terraform`" - `Бакулев Евгений`

### Задание 1

В данном файле .gitignore указано, что файлы с расширением .tfstate и каталог .terraform будут проигнорированы и не попадут в контроль версий. Также указан файл personal.auto.tfvars, который не будет добавлен в репозиторий.

Поэтому личную и секретную информацию (логины, пароли, ключи, токены и т.д.) можно сохранить в файле personal.auto.tfvars, так как он будет проигнорирован при коммите и не попадет в репозиторий.


![Password](https://github.com/garrkiss/terraform_1/blob/main/img/randow_password.png)

![Terraform validate](https://github.com/garrkiss/terraform_1/blob/main/img/terraform_validate.png)


### Исправленый код
```
resource "docker_image" "nginx" {
  name         = "nginx:latest"
  keep_locally = true
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "example_${random_password.random_string.result}"

  ports {
    internal = 80
    external = 9090
  }
}

```

![Dockeps](https://github.com/garrkiss/terraform_1/blob/main/img/dockerps.png)

![Dockeps](https://github.com/garrkiss/terraform_1/blob/main/img/dockerps_hello.png)

![tfstate](https://github.com/garrkiss/terraform_1/blob/main/img/terraform-tfstate.png)


Keep_locally - (Необязательно, логическое значение) Если true, то образ Docker не будет удален при операции уничтожения. Если это ложь, он удалит изображение из локального хранилища докера при операции уничтожения.



