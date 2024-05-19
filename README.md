# Desafio 6 - Escola DNC
O projeto consiste em desenvolver um banco de dados relacional usando MySQL e criar uma API para interagir com esse banco de dados.

### Índice
<ul>
  <a href="#instalação"><li>Instalação</li></a>
  <a href="#banco-de-dados"><li>Banco de Dados</li></a>
  <a href="#estrutura-do-projeto"><li>Estrutura do projeto</li></a>
  <a href="#documentação-da-api"><li>Documentação da API</li></a>
  <a href="#uml"><li>UML</li></a>
  <a href="#demonstração"><li>Demonstração</li></a>
</ul>

---

### Instalação

##### Clone o repositório:
```sh
$ git clone git@github.com:Brandolt55/desafio_6-dnc.git
```

##### Acesse a pasta criada:
```sh
$ cd desafio_6-dnc
```

---

### Banco de Dados
##### Crie o banco de dados MySQL e as tabelas a seguir:

```mysql
CREATE TABLE `clients` (
	`id` INT(10) NOT NULL AUTO_INCREMENT,
	`name` VARCHAR(255) NOT NULL COLLATE 'utf8mb4_0900_ai_ci',
	`email` VARCHAR(255) NOT NULL COLLATE 'utf8mb4_0900_ai_ci',
	`password` VARCHAR(255) NOT NULL COLLATE 'utf8mb4_0900_ai_ci',
	PRIMARY KEY (`id`) USING BTREE
)
COLLATE='utf8mb4_0900_ai_ci'
ENGINE=InnoDB
```

```mysql
CREATE TABLE `stock` (
	`id` INT(10) NOT NULL AUTO_INCREMENT,
	`product_id` INT(10) NOT NULL,
	`quantity` INT(10) NULL DEFAULT '0',
	PRIMARY KEY (`id`) USING BTREE,
	INDEX `product_id` (`product_id`) USING BTREE,
	CONSTRAINT `product_id` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`) ON UPDATE NO ACTION ON DELETE NO ACTION
)
COLLATE='utf8mb4_0900_ai_ci'
ENGINE=InnoDB
```

```mysql
CREATE TABLE `product` (
	`id` INT(10) NOT NULL AUTO_INCREMENT,
	`name` VARCHAR(255) NOT NULL COLLATE 'utf8mb4_0900_ai_ci',
	`description` VARCHAR(255) NOT NULL COLLATE 'utf8mb4_0900_ai_ci',
	`price` FLOAT NOT NULL,
	`author` VARCHAR(255) NOT NULL COLLATE 'utf8mb4_0900_ai_ci',
	`category` VARCHAR(255) NOT NULL COLLATE 'utf8mb4_0900_ai_ci',
	PRIMARY KEY (`id`) USING BTREE
)
COLLATE='utf8mb4_0900_ai_ci'
ENGINE=InnoDB
```

```mysql
CREATE TABLE `products_sales` (
	`id` INT(10) NOT NULL AUTO_INCREMENT,
	`sales_id` INT(10) NOT NULL,
	`product_id` INT(10) NOT NULL,
	`quantity` INT(10) NULL DEFAULT '0',
	PRIMARY KEY (`id`) USING BTREE,
	INDEX `sales_id` (`sales_id`) USING BTREE,
	INDEX `product__id` (`product_id`) USING BTREE,
	CONSTRAINT `product__id` FOREIGN KEY (`product_id`) REFERENCES `product` (`id`) ON UPDATE NO ACTION ON DELETE NO ACTION,
	CONSTRAINT `sales_id` FOREIGN KEY (`sales_id`) REFERENCES `sales` (`id`) ON UPDATE NO ACTION ON DELETE NO ACTION
)
COLLATE='utf8mb4_0900_ai_ci'
ENGINE=InnoDB
```

```mysql
CREATE TABLE `sales` (
	`id` INT(10) NOT NULL AUTO_INCREMENT,
	`client_id` INT(10) NOT NULL,
	`total_amount` FLOAT NOT NULL,
	`quantity` INT(10) NULL DEFAULT '0',
	PRIMARY KEY (`id`) USING BTREE,
	INDEX `client_id` (`client_id`) USING BTREE,
	CONSTRAINT `client_id` FOREIGN KEY (`client_id`) REFERENCES `clients` (`id`) ON UPDATE NO ACTION ON DELETE NO ACTION
)
COLLATE='utf8mb4_0900_ai_ci'
ENGINE=InnoDB
```

##### Configure o banco de dados em connectDB.js (src/middlewares/connectDB.js)
```js
const dbConfig = {
  host: 'localhost',
  user: 'root',
  password: 'senha',
  database: 'name_database',
};
```

##### Instale as dependências:
```sh
$ npm i
```

##### Inicie o projeto com:
```sh
$ npm run dev
```

---

### Estrutura do projeto
#### Parte 1 - Estrutura e Banco de Dados.
##### Tecnologias que foram utilizadas
<div style="display: inline_block">
  
  [![My Skills](https://skillicons.dev/icons?i=express,nodejs,mysql)](https://skillicons.dev)
</div>



---

### Documentação da API
#### CLIENTES
###### Retornar todos os clientes

```sh
  GET /clients
```

###### Retornar um cliente pelo ID

```
  GET /clients/${id}
```

| Parâmetro   | Tipo       | Descrição                                      | Body?                                  |
| :---------- | :--------- | :--------------------------------------------- | :------------------------------------- |
| `id`        | `int`      | **Obrigatório**                                | **Não**                                |

###### Criar um cliente

```
  POST /clients/create
```

| Parâmetro   | Tipo       | Descrição                                      | Body?                                  |
| :---------- | :--------- | :--------------------------------------------- | :------------------------------------- |
| `name`      | `string`   | **Obrigatório**                                | **Sim**                                |
| `email`     | `string`   | **Obrigatório**                                | **Sim**                                |
| `password`  | `string`   | **Obrigatório**                                | **Sim**                                |

###### Exemplo

```json
{
	"name": "dnc",
	"email":"escola@teste.com",
	"password":"desafio123"
}
```

###### Editar um cliente pelo id

```
  PUT /clients/${id}
```

| Parâmetro   | Tipo       | Descrição                                      | Body?                                  |
| :---------- | :--------- | :--------------------------------------------- | :------------------------------------- |
| `id`        | `int`      | **Obrigatório**                                | **Não**                                |
| `name`      | `string`   | **Opcional**                                   | **Sim**                                |
| `email`     | `string`   | **Opcional**                                   | **Sim**                                |
| `password`  | `string`   | **Opcional**                                   | **Sim**                                |

###### Exemplo

```json
{
	"email":"desafio6@teste.com"
}
```

###### Apagar um cliente pelo ID

```
  DELETE /clients/${id}
```

| Parâmetro   | Tipo       | Descrição                                      | Body?                                  |
| :---------- | :--------- | :--------------------------------------------- | :------------------------------------- |
| `id`        | `int`      | **Obrigatório**                                | **Não**                                |

---

#### PRODUTOS
###### Retornar todos os produtos

```
  GET /products
```

###### Retornar um produto pelo ID

```
  GET /products/${id}
```

| Parâmetro   | Tipo       | Descrição                                      | Body?                                  |
| :---------- | :--------- | :--------------------------------------------- | :------------------------------------- |
| `id`        | `int`      | **Obrigatório**                                | **Não**                                |

###### Criar um produto

```
  POST /products/create
```

| Parâmetro   | Tipo       | Descrição                                      | Body?                                  |
| :---------- | :--------- | :--------------------------------------------- | :------------------------------------- |
| `name`      | `string`   | **Obrigatório**                                | **Sim**                                |
| `description` | `string`   | **Obrigatório**                                | **Sim**                                |
| `price`     | `float`    | **Obrigatório**                                | **Sim**                                |
| `author`     | `string`    | **Obrigatório**                                | **Sim**                                |
| `category`     | `string`    | **Obrigatório**                                | **Sim**                                |

###### Exemplo

```json
{
	"name": "Geladeira",
	"description": "Geladeira 2 portas",
	"price": 2990.90,
	"author": "Loja de Eletrodomesticos",
	"category": "Eletrodomesticos"
}
```

###### Editar um produto pelo ID

```
  PUT /products/${id}
```

| Parâmetro   | Tipo       | Descrição                                      | Body?                                  |
| :---------- | :--------- | :--------------------------------------------- | :------------------------------------- |
| `id`        | `int`      | **Obrigatório**                                | **Não**                                |
| `description` | `string`   | **Opcional**                                | **Sim**                                |
| `price`     | `float`    | **Opcional**                                | **Sim**                                |
| `author`     | `string`    | **Opcional**                                | **Sim**                                |
| `category`     | `string`    | **Opcional**                                | **Sim**                                |

###### Exemplo

```json
{
	"price": 19999.00
}
```

###### Apagar um produto pelo ID

```
  DELETE /products/${id}
```

| Parâmetro   | Tipo       | Descrição                                      | Body?                                  |
| :---------- | :--------- | :--------------------------------------------- | :------------------------------------- |
| `id`        | `int`      | **Obrigatório**                                | **Não**                                |

---

#### ESTOQUE
###### Retornar o estoque de todos os produtos

```
  GET /stock
```

###### Retornar o estoque de um produto pelo ID

```
  GET /stock/${id}
```

| Parâmetro    | Tipo       | Descrição                                      | Body?                                  |
| :----------- | :--------- | :--------------------------------------------- | :------------------------------------- |
| `id`         | `int`      | **Obrigatório** - ID do produto                | **Não**                                |

###### Corrigir o estoque de um produto pelo ID

```
  PUT /stock/correction/${id}
```

| Parâmetro    | Tipo       | Descrição                                      | Body?                                  |
| :----------- | :--------- | :--------------------------------------------- | :------------------------------------- |
| `id`         | `int`      | **Obrigatório**                                | **Não**                                |
| `quantity` | `int`      | **Obrigatório**                                | **Sim**                                |

###### Exemplo

```json
{
	"quantity": 9
}
```

###### Atualizar o estoque de um produto pelo ID

```
  PUT /stock/update/${id}
```

| Parâmetro    | Tipo       | Descrição                                      | Body?                                  |
| :----------- | :--------- | :--------------------------------------------- | :------------------------------------- |
| `id`         | `int`      | **Obrigatório**                                | **Não**                                |
| `quantity` | `int`      | **Obrigatório**                                | **Sim**                                |

###### Exemplo

```json
{
	"quantity": 2
}
```

---

#### VENDAS
###### Retornar todos as vendas

```
  GET /sales
```

###### Retornar uma venda pelo id

```
  GET /sales/${id}
```

| Parâmetro   | Tipo       | Descrição                                      | Body?                                  |
| :---------- | :--------- | :--------------------------------------------- | :------------------------------------- |
| `id`        | `int`      | **Obrigatório**                                | **Não**                                |

###### Criar uma venda

```
  POST /sales/create
```

| Parâmetro    | Tipo       | Descrição                                      | Body?                                  |
| :----------- | :--------- | :--------------------------------------------- | :------------------------------------- |
| `products`   | `array`   | **Obrigatório**                                | **Sim**                                |
| `product_id` | `int`      | **Obrigatório**                                | **Sim**                                |
| `quantity` | `int`      | **Obrigatório**                                | **Sim**                                |
| `price`      | `float`    | **Obrigatório**                                | **Sim**                                |
| `cliente_id` | `int`      | **Obrigatório**                                | **Sim**                                |

###### Exemplo

```json
{
	"products":[
		{
			"product_id": 1,
			"quantity": 2,
			"price": 19000.99
		}
	],
	"cliente_id": 1
}
```

---

### UML

![image](https://github.com/Brandolt55/desafio_6-dnc/assets/139605432/bdfe65c9-cd5e-4b8a-8ba5-9a3426217ddd)

---

### Demonstração

![UML](https://github.com/Brandolt55/desafio_6-dnc/public/demonstration.mp4)
