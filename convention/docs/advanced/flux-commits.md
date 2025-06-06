# FluxCD: настройка подписи коммитов

Чтобы настроить подпись коммитов у FluxCD, необходимо его бутстрапить с _бинарным файлом подписи. На примере `Terraform`:

1. Создаем подпись:

   ```hcl
   resource "gpg_private_key" "fluxcd" {
     name     = "fluxcd[bot]"
     email    = "XXXXXXXX+fluxcd[bot]@users.noreply.github.com"
     rsa_bits = 4096
   }
   ```
   Провайдеры:
   ```hcl
   terraform {
     required_version = ">= 1.10"

     required_providers {
       flux = {
         source  = "fluxcd/flux"
         version = ">= 1.4"
       }
       gpg = {
         source  = "Olivr/gpg"
         version = ">= 0.2"
       }
     }
   }
   ```

2. Коммитим приватный ключ в GitHub:

    ```hcl
    resource "github_repository_file" "default" {
      repository          = "ТЕКУЩИЙ_РЕПОЗИТОРИЙ"
      branch              = "master"
      file                = "ПУТЬ_ДО_ФАЙЛА"
      content             = gpg_private_key.fluxcd.private_key
      commit_message      = "СООБЩЕНИЕ_КОММИТА"
      overwrite_on_create = true
    }
    ```

3. Переводим закомиченный файл с ASCII ключом `gpg --dearmor ПУТЬ_ДО_ФАЙЛА_ИЗ_КОММИТА`
4. Выводим ID ключа `gpg --list-packets --verbose ПУТЬ_ДО_ФАЙЛА_ИЗ_КОММИТА`, берем его `keyid`
5. По итогу наш провайдера FluxCD будет выглядеть так:

    ```hcl
    provider "flux" {
      kubernetes = {
        ...
      }
      git = {
        ...
        gpg_key_ring   = "ПУТЬ_ДО_БИНАРНОГО_ФАЙЛА_ИЗ_ПУНКТА_3"
        gpg_key_id     = "keyid_ИЗ_ПУНКТА_4"
        gpg_passphrase = gpg_private_key.fluxcd.passphrase
        ...
        ssh = {
          username    = "git"
          ...
        }
      }
    }
    ```
