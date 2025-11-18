# 📘 Projeto Laravel com Docker — Guia de Instalação

Este README explica como configurar, iniciar e desenvolver este projeto Laravel utilizando Docker, incluindo:

* Requisitos
* Como fazer o build e subir os containers
* Como executar o Composer dentro do container
* Como configurar o ambiente
* Como habilitar e utilizar o debug com Xdebug

---

## ✅ Requisitos

Antes de começar, instale:

* **Docker**
* **Docker Compose**
* (Opcional) **VSCode** com:

  * Extensão *PHP Debug*
  * Extensão *Docker*

---

## 📥 Clonando o Projeto

Para obter o projeto pela primeira vez:

```bash
git clone <URL_DO_REPOSITORIO>
cd <PASTA_DO_PROJETO>
```

---

## ▶️ Subindo o Ambiente

Na raiz do projeto, execute:

```bash
docker compose up -d --build
```

Isso irá iniciar:

* PHP-FPM
* Nginx
* MySQL

Após subir, o projeto estará disponível em:

👉 **[http://localhost:8080](http://localhost:8080)**

---

## 📁 Estrutura da Aplicação

O código do Laravel fica dentro de:

```
src/
```

---

## 📦 Instalando dependências com Composer

Para executar o Composer dentro do container PHP:

```bash
docker compose exec app bash
composer install
```

---

## ⚙️ Configuração do .env

O arquivo `.env` dentro de `src/` deve conter:

```
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=root
```

Após editar, execute:

```bash
docker compose exec app php artisan config:clear
```

---

## 🗄️ Executando Migrações

Para criar as tabelas no banco:

```bash
docker compose exec app php artisan migrate
```

---

## 🐞 Configurando Xdebug

O Xdebug já está habilitado no container. Os seguintes parâmetros foram configurados:

```
xdebug.mode=debug,develop
xdebug.start_with_request=yes
xdebug.client_host=host.docker.internal
xdebug.client_port=9003
xdebug.var_display_max_children=-1
xdebug.var_display_max_data=-1
xdebug.var_display_max_depth=-1
```

Isso permite visualizar arrays/objetos completos sem cortes.

---

## 🖥️ Configuração do Debug no VSCode

No arquivo `.vscode/launch.json`, configure:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Listen for Xdebug",
      "type": "php",
      "request": "launch",
      "port": 9003,
      "pathMappings": {
        "/var/www": "${workspaceFolder}/src"
      }
    }
  ]
}
```

### Como usar

1. Inicie o modo debug no VSCode (*Run → Start Debugging*)
2. Coloque um breakpoint em qualquer controller, service ou rota
3. Acesse a rota pelo navegador

---

## 🐳 Comandos Úteis

### Entrar no container da aplicação:

```bash
docker compose exec app bash
```

### Entrar no MySQL:

```bash
docker compose exec mysql bash
mysql -u root -p
```

### Derrubar tudo:

```bash
docker compose down
```

### Subir novamente:

```bash
docker compose up -d
```

---

## ✔ Ambiente pronto!
