# Домашнее задание к занятию "`Введение в Terraform`" - `Бакулев Евгений`

### Задание 1

В данном файле .gitignore указано, что файлы с расширением .tfstate и каталог .terraform будут проигнорированы и не попадут в контроль версий. Также указан файл personal.auto.tfvars, который не будет добавлен в репозиторий.

Поэтому личную и секретную информацию (логины, пароли, ключи, токены и т.д.) можно сохранить в файле personal.auto.tfvars, так как он будет проигнорирован при коммите и не попадет в репозиторий.


![Password](https://github.com/garrkiss/terraform_1/blob/main/img/randow_password.png)

![Terraform validate](https://github.com/garrkiss/terraform_1/blob/main/img/terraform_validate.png)

### Ошибки

1. 
```
24: resource "docker_image" {
All resource blocks must have 2 labels (type, name).
```
Ответ:
Пропущено название ресурса блока

2. 
```
29: resource "docker_container" "1nginx" {
A name must start with a letter or underscore and may contain only letters, digits, underscores, and dashes.
```
Ответ:
Название ресурса блока должно начинаться с буквы

3. 
```
31: name = "example_${random_password.random_string_FAKE.resulT}"
A managed resource "random_password" "random_string_FAKE" has not been declared in the root module.
```

4. 
```
31: name = "example_${random_password.random_string.resulT}"
This object has no argument, nested block, or exported attribute named "resulT".
```
Ответ:
Исправить название ресурса и его ключ в соответствии со значениями из файла terraform.tfstate


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

Опасность использования ключа -auto-approve:
Ключ -auto-approve в Terraform используется для того, чтобы пропустить шаг подтверждения перед применением изменений. Это может быть полезно в автоматизированных сценариях, где нужно избежать вмешательства пользователя, например, в CI/CD pipeline. Однако есть несколько рисков, связанных с его использованием:

Отсутствие проверки изменений: При использовании -auto-approve Terraform применяет все изменения без запроса подтверждения, что может привести к неожиданным последствиям, если изменения были неверными или несанкционированными.
Невозможность отката: Если что-то пошло не так, и были внесены изменения, которые повлияли на инфраструктуру или данные, откатить их будет сложнее, поскольку не было возможности вручную проверить план изменений.
Опасность при ошибках конфигурации: Если конфигурация содержит ошибку, которая может привести к сбоям (например, удаление ресурсов или создание новых с ошибками), Terraform автоматически применит эти изменения, что может привести к недоступности сервисов или потере данных.
Таким образом, ключ -auto-approve может быть полезен в автоматизированных процессах, но для ручных проверок лучше избегать его использования, чтобы удостовериться, что изменения не приведут к непредсказуемым последствиям.

![tfstate](https://github.com/garrkiss/terraform_1/blob/main/img/terraform-tfstate.png)


Keep_locally - (Необязательно, логическое значение) Если true, то образ Docker не будет удален при операции уничтожения. Если это ложь, он удалит изображение из локального хранилища докера при операции уничтожения.



