# Nicolas Massuda
# Vitor Alencar

# Comparação entre CRUD MVC e Arquitetura de API

##  Sumário

- [1. Diferenças Arquiteturais](#1-diferenças-arquiteturais)
- [2. Responsabilidades de Cada Camada](#2-responsabilidades-de-cada-camada)
- [3. Relação entre as Camadas](#3-relação-entre-as-camadas)
- [4. Persistência de Dados](#4-persistência-de-dados)
- [5. O Padrão Mais Utilizado no Mercado](#5-o-padrão-mais-utilizado-no-mercado)
- [6. Como Testar uma API RESTful](#6-como-testar-uma-api-restful)
- [7. Como Documentar uma API RESTful](#7-como-documentar-uma-api-restful)
- [8. Como Consumir uma API no Front-end](#8-como-consumir-uma-api-no-front-end)

---

## 1. Diferenças Arquiteturais

| Critério | CRUD MVC Tradicional | API |
| :--- | :--- | :--- |
| **Acoplamento** | **Monolítico:** Front-end e Back-end juntos. | **Desacoplado:** Front-end e Back-end totalmente independentes. |
| **Resposta** | Retorna **HTML renderizado** (com CSS/JS). | Retorna **dados puros** (JSON). |
| **Estado** | **Stateful:** Mantém sessão no servidor (`HttpSession`). | **Stateless:** Cada requisição carrega suas próprias credenciais. |
| **Clientes** | Navegadores Web. | Multiplataforma (Web, Mobile, IoT). |
| **Renderização** | **Server-Side Rendering (SSR)** | **Client-Side Rendering (CSR)** |

---

## 2. Responsabilidades de Cada Camada

### Padrão MVC
* **Model:** Regras de negócio, dados e persistência.
* **View:** Interface visual (Templates HTML como Thymeleaf).
* **Controller:** Processa requisições HTTP e escolhe a View que será exibida.

### Padrão API
* **Controller / Resource:** Expõe os *endpoints*, recebe DTOs e retorna JSON com HTTP Status Codes.
* **Service:** Contém a regra de negócio, validações e orquestração de lógica.
* **Repository / DAO:** Comunica-se exclusivamente com o Banco de Dados.
* **Model / Entity:** Mapeamento estruturado das tabelas do banco na aplicação.

---

## 3. Relação entre as Camadas

### Fluxo MVC
```text
[ Cliente (Navegador) ] <---> [ Controller ] <---> [ Model ] <---> [ Banco de Dados ]
                                    |
                                    +----------> [ View (HTML) ]
                            

```

### Fluxo API

```text
[ Cliente (SPA / Mobile) ] <-- JSON --> [ Controller ] ---> [ Service ] ---> [ Repository ] ---> [ Banco de Dados ]

```

---

## 4. Persistência de Dados

Ambos os padrões utilizam **ORMs** (como Hibernate/JPA, Entity Framework, Prisma) ou bibliotecas de consulta SQL.

* **No MVC:** Os formulários enviados são convertidos diretamente para os objetos do Model.
* **Na API:** A comunicação utiliza **DTOs (Data Transfer Objects)**:
1. **Input:** O payload JSON é recebido como `CreateDTO`.
2. **Mapeamento:** A Service converte o DTO para a `Entity` do banco de dados.
3. **Output:** A resposta da requisição é mapeada para um `ResponseDTO`, ocultando dados sensíveis (ex: senhas).



---

## 5. O Padrão Mais Utilizado no Mercado

A **API RESTful (Arquitetura Desacoplada)** é o padrão dominante no mercado:

* **Reuso do Back-end:** Uma única API atende Web e Apps Mobile (iOS/Android).
* **Divisão de Papéis:** Separação clara entre times de Front-end e Back-end.
* **Escalabilidade:** Possibilidade de escalar a interface e o servidor de forma independente.

---

## 6. Como Testar uma API RESTful

* **Manualmente:** Utilizando clientes HTTP como **Postman**, **Insomnia**, **Bruno** ou **curl**.
* **Testes Unitários:** Validação isolada de regras de negócio na camada `Service` utilizando Mocks.
* **Testes de Integração:** Simulação de requisições HTTP reais testando o fluxo completo de *endpoints*.
* **Testes de Carga:** Ferramentas como **k6** e **JMeter** para avaliar performance sob estresse.

---

## 7. Como Documentar uma API RESTful

* **OpenAPI / Swagger:** Padrão   que gera uma interface visual interativa para explorar e testar a API.
* **Informações essenciais na documentação:**
* URL Base e controle de versão (ex: `/api/v1`).
* Esquema de autenticação (ex: `Bearer JWT`).
* Rotas e Métodos HTTP (`GET`, `POST`, `PUT`, `DELETE`).
<img width="800" height="461" alt="image" src="https://github.com/user-attachments/assets/b1b91958-5b0d-4564-8488-1ee61decc266" />

* Estrutura de Payload de requisição e resposta.
* Mapeamento de Erros e Códigos HTTP (`200`, `201`, `400`, `401`, `404`, `500`).



---

## 8. Como Consumir uma API no Front-end

O consumo de uma API RESTful no front-end é feito através de **requisições HTTP assíncronas** iniciadas pela aplicação cliente (Web, Mobile ou Desktop).

### Mecanismos e Ferramentas
* **Nativos da Linguagem:** Ferramentas nativas do navegador (como a **Fetch API** no JavaScript) que gerenciam requisições assíncronas via *Promises* ou *async/await*.
* **Bibliotecas HTTP:** Clientes especializados como **Axios** que oferecem recursos avançados, tratamento global de erros e conversão automática de dados.

### Fluxo de Comunicação
1. **Disparo do Evento:** Uma ação do usuário na interface (como clicar em um botão, enviar um formulário ou carregar a página) aciona a chamada à API.
2. **Envio da Requisição:** O cliente envia uma requisição HTTP para o *endpoint* da API, incluindo o método (`GET`, `POST`, `PUT`, `DELETE`), cabeçalhos de autenticação (como tokens JWT) e o *payload* JSON quando necessário.
3. **Estado de Carregamento:** A interface pode exibir um indicador visual (*spinner* ou *loading*) enquanto aguarda a resposta assíncrona do servidor.
4. **Tratamento da Resposta:**
   * **Em caso de Sucesso (Status 2xx):** Os dados em formato JSON retornados pela API são processados para atualizar o estado da aplicação e renderizar as informações na tela.
   * **Em caso de Erro (Status 4xx/5xx):** A aplicação captura o status e exibe uma mensagem amigável para o usuário (ex: "Sessão expirada", "Dados inválidos" ou "Servidor indisponível").
