# RESTful API web server dockerized on PHP

This is a basic Hello-Wolrd appplication.

## Requirements

It is assumed you have Linux OS, but you can run this project in Windows or MacOS also, just use the alternative  lines commands.

**Linux**

- `Docker` installed
- `docker-compose` installed
- Any rest consumer application like `Postman` installed or any plugin REST client in any browser (like "Advanced REST client" in Chrome )

## Set up

Running in linux console.

### Prepare

Clone this repository.

```bash
git clone --recurse-submodules https://github.com/federicozacayan/restful-api-php.git .
```
Run the following command in the the laradok folder.

```bash
sudo docker-compose up -d nginx mysql
```

Go to the container.

```bash
sudo docker-compose exec workspace bash
```

Run composer to download the libraries need by laravel.
```bash
composer install
```

After runn `exit` to leave the container, go to the mysql container.
```bash
sudo docker-compose exec laradock_mysql_1 bash
```

After run `mysql -u root -p` and writing `root` as a password run the following queries.
```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'root';
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'root';
ALTER USER 'default'@'%' IDENTIFIED WITH mysql_native_password BY 'secret';
```

After run `exit` to leave the container, go to the workspace container again.
```bash
sudo docker-compose exec workspace bash
```

Now, generate fake data into the database.
```bash
php artisan migrate:refresh --seed
```


### Run

Test the application in the following url.

```url
http://localhost/api/Product/
```

### Stop

Run the following command in the the laradok folder.

```bash
sudo docker-compose down
```

### Clean your disk

Run the following command to remove the container first and his image after.

```bash
sudo docker rmi laradock_nginx laradock_php-fpm laradock_workspace docker:dind laradock_mysql
```

## Tutorial

You can find a tutorial of this project in the following site.

[https://federicozacayan.github.io/tutorial/restful-api-php/](https://federicozacayan.github.io/tutorial/restful-api-php/)

## Usage

All the responses have `ContentType application/json` header.

### List all products

**Definition**

`GET /Product`

**Response**

- `200 OK` on success.

```json
[
    {
        "id": 1,
        "name": "Federico Zacayan",
        "description": "effertz.maureen@yahoo.com",
        "created_at": "2019-09-19 06:48:04",
        "updated_at": "2019-09-19 06:48:04"
    },
    {
        "id": 2,
        "name": "Software Developer",
        "description": "obaumbach@schuster.com",
        "created_at": "2019-09-19 06:48:04",
        "updated_at": "2019-09-19 06:48:04"
    },
]
```

### Registering a new product

**Definition**

`POST /Product`

**Arguments**

- `"name":string` a friendly name for this product.
- `"description":string` a friendly name for this product.

**Response**

- `201 Created` on success.

```json
{
    "name": "Federico Zacayan",
    "description": "federico.zacayan@gmail.com",
    "updated_at": "2019-09-25 03:00:58",
    "created_at": "2019-09-25 03:00:58",
    "id": 1
}
```

## Lookup product details

`GET /Product/<identifier>`

**Response**

- `200 OK` on success.

```json
{
    "id": 1,
    "name": "Federico Zacayan",
    "description": "federico.zacayan@gmail.com",
    "created_at": "2019-09-19 06:48:04",
    "updated_at": "2019-09-19 06:48:04"
}
```
## update products

`PUT /Product/<identifier>`
**Arguments**
- `"name":string` a friendly name for this product.
- `"description":string` a friendly name for this product.

**Response**

- `200 OK` on success.

```json
{
    "name": "Federico Zacayan",
    "description": "federico.zacayan@gmail.com",
    "updated_at": "2019-09-25 03:00:58",
    "created_at": "2019-09-25 03:00:58",
    "id": 1
}
```

## Delete a product

**Definition**

`DELETE /products/<identifier>`

**Response**

- `204 No Content` on success.
