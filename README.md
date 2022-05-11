<h1 align="center">
	Fake-API para o app myPlant (capstone)
</h1>

<p align="center">API que possui uma base de dados de flores pública e gerenciamento de usuários.</p>

A API tem um total de 5 endpoints, sendo 2 de autenticação do usuário, e 3 de visualização, criação e atualização dos posts

O url base da API é https://my-plants-app.herokuapp.com/

json do insomnia


Tabela de conteúdos
=================
<!--ts-->
* [Usuário](#User)
	* [Signup](#Signup)
	* [Login](#Login)
	* [Listar Usuário](#ListUser)
  * [Plantas do Usuário](#Plants)
    * [Listar plantas](#ListUserPlant)
    * [Adicionar planta](#AddUserPlant)
    * [Editar planta](#EditUserPlant)
    * [Remover planta](#RemoveUserPlant)
<!--te-->

              
<h2 id='User'>Rotas para o usuário</h2>
<h3 id='Signup'align ='center'>Signup</h3>

`POST /signup -  FORMATO DA REQUISIÇÃO`
```json
{
	"email": "kenzinho@mail.com",
	"password": "123456",
	"name": "kenzinho",
}
```

Caso dê tudo certo, a resposta será assim:

`POST /signup -  FORMATO DA RESPOSTA - STATUS 201`
```json
{
	"accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InBhdWxvQGVtYWlsLmNvbSIsImlhdCI6MTY1MTgwMjAzMCwiZXhwIjoxNjUxODA1NjMwLCJzdWIiOiIyIn0.FohGG4i7LtMSZqyoW0uf9bKVID9q-N37fzFf6AaL_6w",
	"user": {
		"email": "kenzinho@mail.com",
		"name": "kenzinho",
		"id": 2
	}
}
```

<h4 align ='center'> Possíveis erros </h4>

Email já cadastrado:

`POST /signup - `
``  FORMATO DA RESPOSTA - STATUS 401``
```json
"Email already exists"
```

Senha com poucos caracteres:

`POST /signup - `
``  FORMATO DA RESPOSTA - STATUS 401``
```json
"Password is too short"
```


<h3 id='Login' align ='center'> Login </h3>

`POST /login - FORMATO DA REQUISIÇÃO`
```json
{
	"email": "kenzinho@mail.com",
	"password": "123456"
}
```

Caso dê tudo certo, a resposta será assim:

`POST /login -  FORMATO DA RESPOSTA - STATUS 201`
```json
{
	"accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6ImtlbnppbmhvQGVtYWlsLmNvbSIsImlhdCI6MTY1MTgwMzUwNSwiZXhwIjoxNjUxODA3MTA1LCJzdWIiOiIxIn0.wa6vPVNKp3G-NFSUrZwPRruhD-n3W6hG0-6J6ijDn_Q",
	"user": {
		"email": "kenzinho@mail.com",
		"name": "Kenzinho",
		"id": 1
	}
}
```

Com essa resposta, temos o id e o token do usuário, podendo assim criar e editar a sua lista de plantas, e fazer comentários na lista pública de plantas




<h3 id='ListUser' align ='center'> Listar Usuário </h3>

Para pegar os dados de um usuário é preciso do seu id, e fazer a autorização com seu token

`GET /users/:id - FORMATO DA REQUISIÇÃO`


Caso dê tudo certo, a resposta será assim:

`GET /users/:id -  FORMATO DA RESPOSTA - STATUS 201`
```json
{
	"email": "kenzinho@mail.com",
	"password": "$2a$10$eOVLQ/oykzNV/G7Y9rc3tOsfe9t9lq6BhlNcaiEWK4W8h3VDG0bP6",
	"name": "Kenzinho",
	"id": 1
}
```

<h4 align ='center'> Listar usuário e suas plantas </h4>

`GET /users/:id?_embed=plants - FORMATO DA REQUISIÇÃO`


Caso dê tudo certo, a resposta será assim:

`GET /users/:id?_embed=plants -  FORMATO DA RESPOSTA - STATUS 201`
```json
{
	"email": "kenzinho@mail.com",
	"password": "$2a$10$eOVLQ/oykzNV/G7Y9rc3tOsfe9t9lq6BhlNcaiEWK4W8h3VDG0bP6",
	"name": "Kenzinho",
	"id": 1,
	"plants": [
		{
			"name": "orquídea",
			"id": 3,
			"userId": 1
		},
		{
			"name": "samambaia",
			"userId": 1,
			"id": 4
		}
	]
}
```


<h2 id='Plants'>Rotas para as plantas do usuário</h2>

Todas as rotas sobre plantas do usuário precisam de autorização

### <h3 id='ListUserPlant' align ='center'> Listar plantas do usuário</h3>

`GET /plantas -  FORMATO DA REQUISIÇÃO`


Caso dê tudo certo, a resposta será assim:

`GET /plants -  FORMATO DA RESPOSTA - STATUS 201`
```json
[
	{
		"name": "orquídea",
		"id": 3,
		"userId": 1
	},
	{
		"name": "samambaia",
		"userId": 1,
		"id": 4
	}
]
```

<h3 id='AddUserPlant'align ='center'>Adicionar planta na lista do usuário</h3>

Não se esqueça de passar o id do usuário na propriedade userId

`POST /plants -  FORMATO DA REQUISIÇÃO`
```json
{
	"name": "samambaia",
	"userId": 1,
}
```

Caso dê tudo certo, a resposta será assim:

`POST /plants -  FORMATO DA RESPOSTA - STATUS 201`
```json
{
	"name": "samambaia",
	"userId": 1,
	"id": 4
}
```


<h3 id='EditUserPlant'align ='center'>Editar planta do usuário</h3>

Passe o id da planta na url, e no body as propriedades que deseja alterar

`PATCH /plants/:id -  FORMATO DA REQUISIÇÃO`
```json
{
	"name": "samambaia americana"
}
```

Caso dê tudo certo, a resposta será assim:

`PATCH /plants/:id -  FORMATO DA RESPOSTA - STATUS 201`
```json
{
	"name": "samambaia americana",
	"userId": 1,
	"id": 4
}
```



<h3 id='RemoveUserPlant'align ='center'>Delete a planta do usuário</h3>

Passe o id da planta na url, e no body as propriedades que deseja alterar

`DELETE /plants/:id -  FORMATO DA REQUISIÇÃO`


Caso dê tudo certo, a resposta será assim:

`DELETE /plants/:id -  FORMATO DA RESPOSTA - STATUS 201`
```json
{}
```






## Rotas que necessitam de autorização

Rotas que necessitam de autorização deve ser informado no cabeçalho da requisição o campo "Authorization", dessa forma:

> Authorization: Bearer {accessToken}


---

