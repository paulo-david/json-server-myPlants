<h1 align="center">
	Fake-API para o app myPlant (capstone)
</h1>

<p align="center">API que possui uma base de dados de flores pública e gerenciamento de usuários.</p>

A API tem um total de 5 endpoints, sendo 2 de autenticação do usuário, e 3 de visualização, criação e atualização dos posts

O url base da API é https://my-plants-app.herokuapp.com

<a href='./Insomnia_myPlants.json'>Json do Insomnia</a>


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
* [Plantas Públicas](#PublicPlants)
	* [Listar plantas públicas](#ListPublicPlants)
	* [Adicionar planta pública](#AddPublicPlants)
	* [Editar planta pública](#EditPublicPlants)
	* [Remover planta pública](#RemovePublicPlants)
  * [Comentários das plantas públicas](#Comments)
    * [Listar comentários](#ListComments)
    * [Adicionar comentário](#AddComments)
    * [Editar comentário](#EditComments)
    * [Remover comentário](#RemoveComments)

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

<h2 id='PublicPlants'>Rotas para as plantas públicas</h2>

Todos podem ver as plantas públicas, mas somente usuários cadastrados podem modifica-lá ( é necessário fazer a autentificação nesse caso )

### <h3 id='ListPublicPlants' align ='center'> Listar plantas públicas</h3>

`GET /public_plants -  FORMATO DA REQUISIÇÃO`


Caso dê tudo certo, a resposta será assim:

`GET /public_plants -  FORMATO DA RESPOSTA - STATUS 201`
```json
[
	{
		"name": "planta",
		"sci_name": "sci planta",
		"info": "planta planta",
		"basic_care": "precisa de terra",
		"color": "red"
		"id": 2,
	},
	{
		"name": "samambaia",
		"sci_name": "sci samambaia",
		"info": "samambaia não samba",
		"basic_care": "precisa de sol",
		"color": "diverse"
		"id": 1,
	}
]
```

<h3 id='AddPublicPlants'align ='center'>Adicionar planta pública</h3>


`POST /public_plants -  FORMATO DA REQUISIÇÃO`
```json
{
	"name": "rosa",
	"sci_name": "rosa gallica",
	"info": "A rosa tem a cor rosa",
	"basic_care": "Bastante água (Se não tiver água não substitua por cachaça)",
	"color": "pink"
}
```

Caso dê tudo certo, a resposta será assim:

`POST /public_plants -  FORMATO DA RESPOSTA - STATUS 201`
```json
{
	"name": "rosa",
	"sci_name": "rosa gallica",
	"info": "A rosa tem a cor rosa",
	"basic_care": "Bastante água (Se não tiver água não substitua por cachaça)",
	"color": "pink",
	"id": 3
}
```

<h3 id='EditPublicPlants'align ='center'>Editar planta pública</h3>

Passe o id da planta na url

`PATCH /public_plants/:id -  FORMATO DA REQUISIÇÃO`
```json
{
	"name": "rosa rosinha"
}
```

Caso dê tudo certo, a resposta será assim:

`PATCH /public_plants/:id -  FORMATO DA RESPOSTA - STATUS 201`
```json
{
	"name": "rosa rosinha",
	"sci_name": "rosa gallica",
	"info": "A rosa tem a cor rosa",
	"basic_care": "Bastante água (Se não tiver água não substitua por cachaça)",
	"color": "pink",
	"id": 5
}
```

<h3 id='RemovePublicPlants'align ='center'>Delete a planta pública</h3>

Passe o id da planta na url

`DELETE /public_plants/:id -  FORMATO DA REQUISIÇÃO`


Caso dê tudo certo, a resposta será assim:

`DELETE /public_plants/:id -  FORMATO DA RESPOSTA - STATUS 201`
```json
{}
```

<h2 id='Comments'>Rotas para os comentários das plantas públicas</h2>

Os comentários estão visíveis para todo mundo, mas para criar, editar e deletar eles, 

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

## Rotas que necessitam de autorização

Rotas que necessitam de autorização deve ser informado no cabeçalho da requisição o campo "Authorization", dessa forma:

> Authorization: Bearer {accessToken}


---

