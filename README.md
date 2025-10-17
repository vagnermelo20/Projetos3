# 🚀 Nome do Projeto - Arquitetura Back-end

<p align="center">
  <img src="https://img.shields.io/badge/Java-17-blue?logo=java" alt="Java 17">
  <img src="https://img.shields.io/badge/Spring%20Boot-3.x.x-brightgreen?logo=spring" alt="Spring Boot 3.x.x">
  <img src="https://img.shields.io/badge/Arquitetura-CQRS%20%26%20Mediator-purple" alt="Arquitetura CQRS">
  <img src="https://img.shields.io/badge/Licen%C3%A7a-MIT-blue" alt="Licença MIT">
</p>

## 📝 Descrição

Este projeto é a implementação de um back-end robusto utilizando **Spring Boot**. Sua principal característica não são apenas as funcionalidades que ele expõe, mas a arquitetura de software utilizada, que se baseia em princípios de **Clean Architecture** e no padrão **CQRS (Command Query Responsibility Segregation)**, orquestrado pelo padrão **Mediator**.

O objetivo é criar um sistema altamente organizado, testável, escalável e de fácil manutenção, onde cada componente possui uma responsabilidade única e bem definida.

---

## 🏛️ Arquitetura do Projeto

A arquitetura foi desenhada para garantir uma clara separação de responsabilidades (Separation of Concerns). O fluxo de uma requisição segue um caminho bem definido, garantindo que a lógica de negócio permaneça isolada e protegida.

**Fluxo de uma Requisição:**
`Cliente (API)  ➡️  Controller  ➡️  Mediator  ➡️  Handler (Command/Query)  ➡️  Repository  ➡️  Banco de Dados`

A estrutura de pastas reflete essa separação:

```
/src/main/java/com/suaempresa/seuprojeto/
|
├── application/         # O CÉREBRO: Lógica de negócio e orquestração.
|   ├── commands/        # Intenções de escrita (CRIAR, ATUALIZAR, DELETAR).
|   ├── queries/         # Intenções de leitura (BUSCAR).
|   └── handlers/        # Executores da lógica para cada command/query.
|
├── domain/              # O CORAÇÃO: As regras e o estado do negócio.
|   └── entities/        # Mapeamento das tabelas do banco de dados.
|
├── infrastructure/      # OS BRAÇOS: Comunicação com o mundo exterior.
|   └── persistence/     # Implementação do acesso ao banco de dados (Repositories).
|
├── web/                 # A FACE: Camada de Apresentação (API REST).
|   └── controllers/     # Pontos de entrada da API (Endpoints).
|
└── SeuProjetoApplication.java
```

### Detalhes de Cada Camada

#### 🌐 Camada Web (`/web/controllers/`)
* **Responsabilidade:** Receber requisições HTTP e retornar respostas HTTP. É a única camada que "fala" a linguagem da web.
* **O que contém?:** Classes anotadas com `@RestController` que definem os endpoints da API (`@GetMapping`, `@PostMapping`, etc.).
* **Princípio Chave:** Controllers devem ser **"magros" (thin)**. Eles não contêm lógica de negócio. Sua função é validar a requisição (formato, dados básicos), empacotar os dados em um `Command` ou `Query` e delegar a execução para a camada de Aplicação através do Mediator.

#### 🧠 Camada de Aplicação (`/application/`)
É o centro da arquitetura, onde a lógica de negócio é orquestrada.

* **Commands (`/commands/`)**:
    * **Responsabilidade:** Representar uma **intenção de alterar o estado** do sistema. São objetos DTO que carregam os dados para uma operação de escrita.
    * **Exemplo:** `CriarProdutoCommand` contém o nome, preço e estoque do produto a ser criado.

* **Queries (`/queries/`)**:
    * **Responsabilidade:** Representar uma **solicitação de dados** do sistema. Não alteram estado.
    * **Exemplo:** `ObterProdutoPorIdQuery` contém apenas o `id` do produto a ser consultado.

* **Handlers (`/handlers/`)**:
    * **Responsabilidade:** Conter a **lógica de negócio** real. Cada `Command` e `Query` possui um `Handler` correspondente.
    * **O que faz?:** Recebe o `Command`/`Query`, aplica regras de negócio, utiliza os `Repositories` para acessar o banco de dados e retorna o resultado.

* **Mediator (Padrão de Design)**:
    * **Responsabilidade:** Desacoplar o `Controller` dos `Handlers`. O Controller não sabe qual `Handler` vai executar a ação. Ele apenas envia o `Command` ou `Query` para o Mediator, que se encarrega de encontrar e invocar o `Handler` correto. Isso simplifica o Controller e facilita a adição de novas funcionalidades.

#### ❤️ Camada de Domínio (`/domain/entities/`)
* **Responsabilidade:** Representar o estado e as regras fundamentais do negócio.
* **O que contém?:** As **Entidades**, que são classes POJO mapeadas para as tabelas do banco de dados com anotações da JPA (`@Entity`, `@Id`, `@Column`, etc.). Elas são a representação canônica dos dados do sistema.

#### 🏗️ Camada de Infraestrutura (`/infrastructure/`)
* **Responsabilidade:** Lidar com detalhes técnicos e comunicação com sistemas externos (banco de dados, APIs de terceiros, filas de mensagens, etc.).
* **O que contém?:** Os **Repositories** (ou DAOs), que são interfaces (`JpaRepository`) responsáveis por abstrair o acesso aos dados. Esta camada implementa a persistência das entidades de Domínio.

---

## 🛠️ Tecnologias Utilizadas

* **Linguagem:** **Java 17+**
* **Framework Principal:** **Spring Boot 3**
* **Acesso a Dados:** **Spring Data JPA** (Hibernate)
* **Banco de Dados:** **PostgreSQL** (Produção) e **H2** (Testes)
* **Segurança:** **Spring Security**
* **Gerenciador de Dependências:** **Maven** / **Gradle**
* **Documentação da API:** **SpringDoc (Swagger)**
* **Padrões de Arquitetura:** **CQRS**, **Mediator**, **N-Tier**
* **Utilitários:** **Lombok** para redução de código boilerplate.

---

## 🚀 Como Executar o Projeto

Siga os passos abaixo para configurar e executar o projeto em seu ambiente local.

### **Pré-requisitos**

* [JDK 17 ou superior](https://www.oracle.com/java/technologies/javase-jdk17-downloads.html)
* [Maven](https://maven.apache.org/download.cgi) ou [Gradle](https://gradle.org/install/)
* [PostgreSQL](https://www.postgresql.org/download/)
* Um cliente de API como [Postman](https://www.postman.com/) ou [Insomnia](https://insomnia.rest/)

### **Passo a Passo**

1.  **Clone o repositório:**
    ```bash
    git clone [https://github.com/vagnermelo20/Projetos3.git](https://github.com/vagnermelo20/Projetos3.git)
    cd Projetos3
    ```

2.  **Configure o Banco de Dados:**
    * Crie um banco de dados no seu PostgreSQL (ex: `meu_projeto_db`).
    * Renomeie o arquivo `src/main/resources/application.properties.example` para `application.properties`.
    * Abra o arquivo e altere as propriedades do `spring.datasource` com suas credenciais:
      ```properties
      spring.datasource.url=jdbc:postgresql://localhost:5432/meu_projeto_db
      spring.datasource.username=seu_usuario_postgres
      spring.datasource.password=sua_senha_postgres
      ```

3.  **Execute a Aplicação:**
    * **Usando Maven:**
      ```bash
      mvn spring-boot:run
      ```
    * **Usando Gradle:**
      ```bash
      ./gradlew bootRun
      ```

4.  A API estará disponível em `http://localhost:8080`.

---

## 📖 Documentação da API

A documentação completa dos endpoints, com modelos de requisição e resposta, é gerada automaticamente pelo SpringDoc e pode ser acessada pela interface do Swagger.

* **URL do Swagger UI:** [http://localhost:8080/swagger-ui.html](http://localhost:8080/swagger-ui.html)

---

## 📬 Contato

**Vagner Melo** - []

**Link do Projeto:** [https://github.com/vagnermelo20/Projetos3](https://github.com/vagnermelo20/Projetos3)
