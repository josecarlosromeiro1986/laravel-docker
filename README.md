# Ambiente Docker para Laravel (Com Explicações)
Este repositório fornece um ambiente Docker completo para desenvolvimento Laravel utilizando PHP-FPM 8.4, MySQL, Redis e Nginx.  
Além disso, este README explica **para que serve cada arquivo, pasta e configuração** — ideal para quem quer entender o funcionamento completo do ambiente.

---

## 📁 Estrutura do Projeto

```
├── docker/ → Contém os Dockerfiles e configurações dos serviços  
│   ├── mysql/ → Configurações específicas do MySQL  
│   ├── nginx/ → Configurações do servidor Nginx  
│   ├── php/ → Dockerfile e customizações do PHP  
│   └── redis/ → Configurações do Redis  
│
├── logs/ → Todos os logs são armazenados aqui (fora dos containers)  
│   ├── mysql/ → Logs do MySQL  
│   ├── nginx/ → Logs de acesso e erro do Nginx  
│   ├── php/ → Logs de erro do PHP-FPM  
│   └── redis/ → Logs do Redis  
│
├── src/ → A pasta onde o Laravel será instalado  
│
├── docker-compose.yml → Arquivo que orquestra todos os containers  
│
└── README.md → Você está aqui!
```

---

# 🔧 Versões Utilizadas

Serviço | Versão | Explicação
------- | ------ | ----------
Laravel | qualquer | Instalado dentro do container PHP
PHP     | 8.4-fpm | Versão atual estável do PHP com FPM
MySQL   | 8.0     | Banco mais utilizado com Laravel
Redis   | latest  | Usado para cache/queue
Nginx   | latest  | Servidor web eficiente para produção e dev

---

# ▶️ Subindo o Ambiente

```
docker-compose up -d
```

Isso faz o Docker:
- Baixar as imagens necessárias
- Buildar o container PHP personalizado
- Subir todos os serviços em segundo plano

---

# 📥 Instalando o Laravel (explicado)

Entre no container app (PHP):

```docker exec -it app bash```

Dentro dele:

1. **Limpa a pasta** para garantir instalação limpa:
```rm -rf /var/www/* /var/www/.*```

2. **Instala o Laravel:**
```composer create-project laravel/laravel ./```

3. **Permissões (importantíssimo)**  
Esses comandos permitem que o Nginx e o PHP-FPM escrevam nos diretórios necessários:

```chown -R $USER:www-data storage bootstrap/cache```
```chmod -R 775 storage bootstrap/cache```

4. **Configura variáveis de ambiente do banco (.env):**

```
DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=laravel_pass
```

5. **Limpa cache e executa as migrations:**

```php artisan config:clear```
```php artisan migrate```

---

# 🌐 Acessos

Item | Endereço | Explicação
---- | -------- | ----------
Aplicação | http://localhost:8080 | Nginx servindo o Laravel
MySQL | porta 3306 | Acesso externo ao banco
Redis | porta 6379 | Acesso de cache/filas

---

# 📝 Explicação dos Arquivos e Pastas

## 📁 docker/php/Dockerfile
- Define a imagem PHP usada
- Instala extensões essenciais (pdo, mysql, redis, mbstring etc.)
- Define o diretório de trabalho (/var/www)

## 📁 docker/nginx/default.conf
- Configura o Virtual Host
- Aponta Nginx para /var/www/public (pasta pública do Laravel)
- Define regras para acessar index.php via PHP-FPM

## 📁 docker/mysql
- Contém arquivos de inicialização do MySQL caso queira criar tabelas automaticamente

## 📁 docker/redis
- Define parâmetros customizados caso necessário  
(por padrão funciona sem tocar)

## 📁 logs/
- Mantém logs persistentes fora dos containers  
(se um container for apagado, os logs continuam)

## 📁 src/
- Onde o Laravel realmente fica  
- Montado como volume dentro do container app

---

# 🔧 Comandos Úteis

## Entrar no container da aplicação:
```docker compose exec app bash```

## Entrar no MySQL:
```docker compose exec mysql bash```
```mysql -u root -p```

## Derrubar tudo:
```docker compose down```

## Subir novamente:
```docker compose up -d```

---

# 📚 Documentação Oficial
https://laravel.com/docs

---

# 📄 Licença
MIT License
