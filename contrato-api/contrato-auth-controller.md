# CONTRATO - AUTH_CONTROLLER

Gerencia a autenticação, registro de novos perfis e o controle de sessão no sistema:

## **Cadastro e Autenticação:**

### `POST /auth/register/cliente`
- **Request:**
  ````json
    {
      "email": "",
      "senha": "",
      "cidade": "",
      "estado": "",
      "cpf": "",
      "nome": "",
      "sobrenome": ""
    }
  ````
- **Response:** 
  - Sucesso: `201 Created`
  - Erro: 
### `POST /auth/register/lava-jato`
- **Request:**
  ````json
      {
        "email": "",
        "senha": "",
        "cidade": "",
        "estado": "",
        "razaoSocial": "",
        "nomeFantasia": "",
        "fluxoSimultaneo": 0,
        "cnpj": "",
        "numero": "",
        "logradouro": "",
        "bairro": "",
        "cep": "",
        "coordenadas": {
        "latitude": 0.0,
        "longitude": 0.0
        }
      }
  ````
- **Response:** 
  - Sucesso: `201 Created`
  - Erro: 
### `POST /auth/login`
- **Request:** 
```json
{
  "email": "",
  "senha": ""
}
```
- **Response:** 
  - Sucesso: `200 OK`
    ```json
    {
      "access_token": "",
      "refresh_token": "",
      "expires_in": 0
    }
    ```
  - Erro:

## **Gestão de Sessão:**

### `POST /auth/logout`
- **Request:** Header `Authorization: Bearer <token>`
- **Response:** 
  - Sucesso: `204 No Content`