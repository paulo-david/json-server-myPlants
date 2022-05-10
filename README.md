<h1 align="center">
   Atividade - JSON-Server: Fake-API do início ao Deploy
</h1>

<p align="center">
  <a href="#endpoints">Endpoints</a>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</p>


# **Endpoints**

A API tem um total de 5 endpoints, sendo 2 de autenticação do usuário, e 3 de visualização, criação e atualização dos posts

O url base da API é https://s3b11.herokuapp.com/
                    
                    
<h2>Rotas para o usuário</h2>
### <h3 align ='center'> Registrar usuário </h3>

`POST /users -  FORMATO DA REQUISIÇÃO`
```json
{
	"email": "kenzinho@mail.com",
	"password": "123456",
	"name": "kenzinho",
	"age": 99
}
```

Caso dê tudo certo, a resposta será assim:

`POST /users -  FORMATO DA RESPOSTA - STATUS 201`
```json
{
	"accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InBhdWxvQGVtYWlsLmNvbSIsImlhdCI6MTY1MTgwMjAzMCwiZXhwIjoxNjUxODA1NjMwLCJzdWIiOiIyIn0.FohGG4i7LtMSZqyoW0uf9bKVID9q-N37fzFf6AaL_6w",
	"user": {
		"email": "kenzinho@mail.com",
		"name": "kenzinho",
		"age": 99,
		"id": 2
	}
}
```

<h4 align ='center'> Possíveis erros </h4>

Email já cadastrado:

`POST /users - `
``  FORMATO DA RESPOSTA - STATUS 401``
```json
"Email already exists"
```

### <h3 align ='center'> Login </h3>

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
		"age": 38,
		"id": 1
	}
}
```

Com essa resposta, temos o token, o qual possibilita, visualizar todos os posts, criar um posts, editar o próprio post

## Rotas para os posts

Todas as rotas sobre os posts precisam de autorização, somente usuários logados podem ver todos os posts e somente o criador do post pode edita-lo

### <h3 align ='center'> Criar post </h3>

`PATCH /posts -  FORMATO DA REQUISIÇÃO`
```json
{
	"msg": "O terceiro módulo foi muito legaaaal",
	"type": "text",
	"userId": 1
}
```

Caso dê tudo certo, a resposta será assim:

`PATCH /posts -  FORMATO DA RESPOSTA - STATUS 201`
```json
{
	"msg": "O terceiro módulo foi muito legaaaal",
	"type": "text",
	"userId": 1,
	"id": 2
}
```

### <h3 align ='center'> Editar post </h3>

É passado o ID do post na url do endpoint. 
<br>
É possível editar o type e a msg do post, dependendo do que é passado no body.

`PATCH /posts/:postID -  FORMATO DA REQUISIÇÃO`
```json
{
  "msg": "O módulo atual é o meu favorito"
}
```

Caso dê tudo certo, a resposta será assim:

`PATCH /posts/:postID -  FORMATO DA RESPOSTA - STATUS 201`
```json
{
  "msg": "O módulo atual é o meu favorito",
  "type": "text",
  "userId": 1,
  "id": 2
}
```

### <h3 align ='center'> Listar posts </h3>

Nessa aplicação o usuário deve fazer o login para ver a lista de posts

`GET /posts -  FORMATO DA RESPOSTA - STATUS 200`
```json
[
	{
		"msg": "capstone la vamos nos",
		"type": "text",
		"userId": 2,
		"id": 1
	},
	{
		"msg": "O módulo atual é o meu favorito",
		"type": "text",
		"userId": 1,
		"id": 2
	},
	{
		"msg": "testesss",
		"type": "text",
		"userId": 1,
		"id": 3
	}
]
```
Podemos utilizar os query params para mudar a lista, mudando a paginação, assim alterar quantos posts queremos no perPage.
No exemplo abaixo capturamos somente os dois primeiros posts

`GET /posts?_page=1&_limit=2 - FORMATO DA RESPOSTA - STATUS 200`
```json
[
	{
		"msg": "capstone la vamos nos",
		"type": "text",
		"userId": 2,
		"id": 1
	},
	{
		"msg": "O módulo atual é o meu favorito",
		"type": "text",
		"userId": 1,
		"id": 2
	}
]
```

Podemos também pesquisar por uma palavra em específico nas msg.
No exemplo abaixo somente é retornado os posts que possuem a palavra favorito

`GET /posts?msg_like=favorito - FORMATO DA RESPOSTA - STATUS 200`
```json
[
	{
		"msg": "O módulo atual é o meu favorito",
		"type": "text",
		"userId": 1,
		"id": 2
	}
]
```

## Rotas que necessitam de autorização

Rotas que necessitam de autorização deve ser informado no cabeçalho da requisição o campo "Authorization", dessa forma:

> Authorization: Bearer {accessToken}


---

