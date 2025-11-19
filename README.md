# Sistema de Serviços - API Spring Boot

Este projeto é uma API REST desenvolvida em Java utilizando o Framework Spring Boot. O sistema demonstra conceitos fundamentais de desenvolvimento backend, incluindo herança, relacionamentos JPA, DTOs, tratamento de exceções e validações.

## 📋 Índice

- [Tecnologias Utilizadas](#tecnologias-utilizadas)
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Conceitos Implementados](#conceitos-implementados)
- [Configuração](#configuração)
- [Como Executar](#como-executar)
- [Testando a API com Postman](#testando-a-api-com-postman)
- [Endpoints Disponíveis](#endpoints-disponíveis)

## 🛠 Tecnologias Utilizadas

- **Java 17**
- **Spring Boot 3.5.6**
- **Spring Data JPA**
- **Spring Validation**
- **MySQL / PostgreSQL**
- **Maven**
- **Swagger/OpenAPI** (Documentação da API)

## 📁 Estrutura do Projeto

```
src/main/java/com/example/demo/
├── controller/          # Camada de controle (REST Controllers)
├── service/            # Camada de serviço (Lógica de negócio)
├── repository/         # Camada de acesso a dados
├── model/              # Entidades JPA
│   └── enums/          # Enumeradores
├── dto/                # Data Transfer Objects
└── exceptions/         # Tratamento de exceções
```

## 📚 Conceitos Implementados

### 1. Conceito de Herança Aplicado nas classes Cliente e Técnico

As classes `Cliente` e `Tecnico` herdam da classe abstrata `Pessoa`, reutilizando atributos e métodos comuns como `id`, `nome`, `cpf`, `email` e `senha`.

**Exemplo de implementação:**

- `Pessoa` é uma classe abstrata com `@MappedSuperclass`, contendo os atributos comuns
- `Cliente` e `Tecnico` estendem `Pessoa` e são marcadas com `@Entity`
- Isso evita duplicação de código e facilita a manutenção

**Arquivos relacionados:**

- `model/Pessoa.java` - Classe base abstrata
- `model/Cliente.java` - Herda de Pessoa
- `model/Tecnico.java` - Herda de Pessoa

### 2. Relacionamentos @OneToMany e @ManyToOne

O sistema implementa relacionamentos entre entidades:

- Um `Tecnico` pode ter vários `Chamados` (OneToMany)
- Um `Cliente` pode ter vários `Chamados` (OneToMany)
- Um `Chamado` pertence a um `Tecnico` e um `Cliente` (ManyToOne)

**Exemplo:**

```java
@ManyToOne
@JoinColumn(name = "tecnico_id")
private Tecnico tecnico;

@ManyToOne
@JoinColumn(name = "cliente_id")
private Cliente cliente;
```

**Arquivo relacionado:**

- `model/Chamado.java`

### 3. Configurações de classes, métodos e atributos com Annotations

O projeto utiliza diversas annotations do Spring e JPA para configurar o comportamento das classes:

- `@Entity` - Define uma entidade JPA
- `@MappedSuperclass` - Classe base que não é uma entidade
- `@Id` e `@GeneratedValue` - Configuração de chave primária
- `@ManyToOne` e `@JoinColumn` - Relacionamentos
- `@RestController` - Define um controller REST
- `@Service` - Define um serviço
- `@Repository` - Define um repositório
- `@Autowired` - Injeção de dependência
- `@RequestMapping`, `@GetMapping`, `@PostMapping`, etc. - Mapeamento de endpoints
- `@Valid` - Validação de dados
- `@JsonFormat` - Formatação de datas no JSON

### 4. Criação e configurações de Enums

O sistema utiliza enums para representar valores fixos:

- `Prioridade` - Define os níveis de prioridade (BAIXA, MEDIA, ALTA)
- `Status` - Define os status dos chamados (ABERTO, ANDAMENTO, ENCERRADO)

**Arquivos relacionados:**

- `model/enums/Prioridade.java`
- `model/enums/Status.java`

Os enums são armazenados no banco como inteiros (ordinal) e convertidos automaticamente nos métodos getter/setter.

### 5. Conceito e implementação de Entidade @Entity

As entidades são classes Java marcadas com `@Entity` que representam tabelas no banco de dados:

- `Pessoa` - Classe base (abstrata, não é entidade)
- `Tecnico` - Entidade que representa técnicos
- `Cliente` - Entidade que representa clientes
- `Chamado` - Entidade que representa chamados de serviço

Cada entidade possui:

- Chave primária (`@Id`)
- Atributos mapeados para colunas do banco
- Relacionamentos com outras entidades

### 6. Conceito e implementação de DTOs

DTOs (Data Transfer Objects) são objetos usados para transferir dados entre camadas, evitando expor a estrutura interna das entidades:

- `TecnicoDTO` - DTO para técnicos
- `ClienteDTO` - DTO para clientes
- `ChamadoDTO` - DTO para chamados

**Vantagens:**

- Controle sobre quais dados são expostos na API
- Validações específicas com `@NotEmpty`, `@NotNull`, etc.
- Desacoplamento entre camadas

**Arquivos relacionados:**

- `dto/TecnicoDTO.java`
- `dto/ClienteDTO.java`
- `dto/ChamadoDTO.java`

### 7. Conceito e implementação de Repositórios @Repository

Repositórios são interfaces que estendem `JpaRepository` e fornecem métodos para acesso a dados:

- `TecnicoRepository` - Operações CRUD para técnicos
- `ClienteRepository` - Operações CRUD para clientes
- `ChamadoRepository` - Operações CRUD para chamados

**Funcionalidades:**

- Herdam métodos como `findAll()`, `findById()`, `save()`, `deleteById()`
- Podem ter métodos customizados usando query methods

**Arquivos relacionados:**

- `repository/TecnicoRepository.java`
- `repository/ClienteRepository.java`
- `repository/ChamadoRepository.java`

### 8. Conceito e implementação de Serviços @Service

A camada de serviço contém a lógica de negócio da aplicação:

- `TecnicoService` - Lógica de negócio para técnicos
- `ClienteService` - Lógica de negócio para clientes
- `ChamadoService` - Lógica de negócio para chamados

**Responsabilidades:**

- Validações de negócio
- Conversão entre DTOs e entidades
- Tratamento de regras específicas
- Chamadas aos repositórios

**Arquivos relacionados:**

- `service/TecnicoService.java`
- `service/ClienteService.java`
- `service/ChamadoService.java`

### 9. Conceito e implementação de Resources @RestController

Controllers são responsáveis por receber requisições HTTP e retornar respostas:

- `TecnicoController` - Endpoints para técnicos
- `ClienteController` - Endpoints para clientes
- `ChamadoController` - Endpoints para chamados

**Características:**

- Marcados com `@RestController`
- Mapeiam URLs para métodos
- Retornam objetos serializados em JSON
- Utilizam códigos HTTP apropriados (200, 201, 204, 404, etc.)

**Arquivos relacionados:**

- `controller/TecnicoController.java`
- `controller/ClienteController.java`
- `controller/ChamadoController.java`

### 10. Implementação de CRUD de todas as classes

Todas as entidades possuem operações CRUD completas:

- **Create (POST)** - Criar novo registro
- **Read (GET)** - Buscar por ID ou listar todos
- **Update (PUT)** - Atualizar registro existente
- **Delete (DELETE)** - Remover registro

**Endpoints implementados:**

- `GET /tecnicos` - Listar todos os técnicos
- `GET /tecnicos/{id}` - Buscar técnico por ID
- `POST /tecnicos` - Criar técnico
- `PUT /tecnicos/{id}` - Atualizar técnico
- `DELETE /tecnicos/{id}` - Deletar técnico

(Os mesmos padrões se aplicam para `/clientes` e `/chamados`)

### 11. Tratamento de exceções Exception

O sistema possui tratamento centralizado de exceções:

- `ResourceExceptionHandler` - Classe que trata todas as exceções
- `ObjectNotFoundException` - Exceção customizada para objetos não encontrados
- `DataIntegrityViolationException` - Exceção para violações de integridade
- `StandardError` - Classe para padronizar respostas de erro
- `ValidationError` - Classe para erros de validação

**Funcionalidades:**

- Retorna mensagens de erro padronizadas
- Códigos HTTP apropriados
- Detalhes sobre o erro e a URI requisitada

**Arquivos relacionados:**

- `exceptions/ResourceExceptionHandler.java`
- `exceptions/ObjectNotFoundException.java`
- `exceptions/StandardError.java`
- `exceptions/ValidationError.java`

### 12. Validações @Valid

O sistema utiliza validações do Bean Validation:

- `@Valid` - Ativa validação em DTOs
- `@NotEmpty` - Campo não pode ser vazio
- `@NotNull` - Campo não pode ser nulo
- Mensagens customizadas para cada validação

**Exemplo:**

```java
@NotEmpty(message = "O campo NOME é requerido")
private String nome;
```

Quando uma validação falha, o sistema retorna um `ValidationError` com detalhes dos campos inválidos.

### 13. Testes de requisição à API com Insomnia ou Postman

A API pode ser testada usando ferramentas como Postman ou Insomnia. Veja a seção [Testando a API com Postman](#testando-a-api-com-postman) abaixo.

## ⚙️ Configuração

### Pré-requisitos

- Java 17 ou superior
- Maven 3.6+
- MySQL ou PostgreSQL (configurado)

### Configuração do Banco de Dados

Edite o arquivo `src/main/resources/application.properties` ou os arquivos de perfil específicos:

**application-dev.properties:**

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/seu_banco
spring.datasource.username=seu_usuario
spring.datasource.password=sua_senha
spring.jpa.hibernate.ddl-auto=update
```

## 🚀 Como Executar

1. Clone o repositório
2. Configure o banco de dados no arquivo `application.properties`
3. Execute o projeto:

```bash
mvn spring-boot:run
```

Ou usando o Maven Wrapper:

```bash
./mvnw spring-boot:run
```

4. A aplicação estará disponível em: `http://localhost:8080`

5. Acesse a documentação Swagger em: `http://localhost:8080/swagger-ui.html`

## 🧪 Testando a API com Postman

### Configuração Inicial

1. Abra o Postman
2. Crie uma nova Collection chamada "Sistema de Serviços"
3. Configure a variável de ambiente `base_url` com o valor: `http://localhost:8080`

### Endpoints - Técnicos

#### 1. Criar Técnico (POST)

**URL:** `{{base_url}}/tecnicos`

**Método:** POST

**Headers:**

```
Content-Type: application/json
```

**Body (raw JSON):**

```json
{
  "nome": "João Silva",
  "cpf": "123.456.789-00",
  "email": "joao@email.com",
  "senha": "123456"
}
```

**Resposta esperada:** Status 201 Created

#### 2. Listar Todos os Técnicos (GET)

**URL:** `{{base_url}}/tecnicos`

**Método:** GET

**Resposta esperada:** Status 200 OK com array de técnicos

#### 3. Buscar Técnico por ID (GET)

**URL:** `{{base_url}}/tecnicos/{id}`

**Método:** GET

**Exemplo:** `{{base_url}}/tecnicos/1`

**Resposta esperada:** Status 200 OK com dados do técnico

#### 4. Atualizar Técnico (PUT)

**URL:** `{{base_url}}/tecnicos/{id}`

**Método:** PUT

**Headers:**

```
Content-Type: application/json
```

**Body (raw JSON):**

```json
{
  "nome": "João Silva Atualizado",
  "cpf": "123.456.789-00",
  "email": "joao.novo@email.com",
  "senha": "novaSenha123"
}
```

**Resposta esperada:** Status 200 OK com dados atualizados

#### 5. Deletar Técnico (DELETE)

**URL:** `{{base_url}}/tecnicos/{id}`

**Método:** DELETE

**Resposta esperada:** Status 204 No Content

### Endpoints - Clientes

Os endpoints de clientes seguem o mesmo padrão dos técnicos:

- `POST /clientes` - Criar cliente
- `GET /clientes` - Listar todos
- `GET /clientes/{id}` - Buscar por ID
- `PUT /clientes/{id}` - Atualizar
- `DELETE /clientes/{id}` - Deletar

**Exemplo de Body para criar cliente:**

```json
{
  "nome": "Maria Santos",
  "cpf": "987.654.321-00",
  "email": "maria@email.com",
  "senha": "senha123"
}
```

### Endpoints - Chamados

#### 1. Criar Chamado (POST)

**URL:** `{{base_url}}/chamados`

**Método:** POST

**Headers:**

```
Content-Type: application/json
```

**Body (raw JSON):**

```json
{
  "titulo": "Problema no computador",
  "observacoes": "Computador não liga",
  "prioridade": 2,
  "status": 0,
  "tecnico": 1,
  "cliente": 1
}
```

**Nota:**

- `prioridade`: 0=BAIXA, 1=MEDIA, 2=ALTA
- `status`: 0=ABERTO, 1=ANDAMENTO, 2=ENCERRADO
- `tecnico` e `cliente` devem ser IDs válidos existentes no banco

**Resposta esperada:** Status 201 Created

#### 2. Listar Todos os Chamados (GET)

**URL:** `{{base_url}}/chamados`

**Método:** GET

**Resposta esperada:** Status 200 OK com array de chamados

#### 3. Buscar Chamado por ID (GET)

**URL:** `{{base_url}}/chamados/{id}`

**Método:** GET

**Resposta esperada:** Status 200 OK com dados do chamado

#### 4. Atualizar Chamado (PUT)

**URL:** `{{base_url}}/chamados/{id}`

**Método:** PUT

**Body (raw JSON):**

```json
{
  "titulo": "Problema resolvido",
  "observacoes": "Computador foi consertado",
  "prioridade": 2,
  "status": 2,
  "tecnico": 1,
  "cliente": 1
}
```

**Resposta esperada:** Status 200 OK com dados atualizados

### Testando Validações

#### Teste de Validação - Campo Vazio

**URL:** `{{base_url}}/tecnicos`

**Método:** POST

**Body (raw JSON):**

```json
{
  "nome": "",
  "cpf": "123.456.789-00",
  "email": "teste@email.com",
  "senha": "123456"
}
```

**Resposta esperada:** Status 400 Bad Request com mensagem de validação:

```json
{
  "timestamp": "...",
  "status": 400,
  "error": "Erro de validação",
  "message": "Erro na validação dos campos",
  "path": "/tecnicos",
  "errors": [
    {
      "fieldName": "nome",
      "message": "O campo NOME é requerido"
    }
  ]
}
```

### Testando Tratamento de Exceções

#### Teste - Objeto Não Encontrado

**URL:** `{{base_url}}/tecnicos/999`

**Método:** GET

**Resposta esperada:** Status 404 Not Found:

```json
{
  "timestamp": "...",
  "status": 404,
  "error": "Objeto não encontrado",
  "message": "Técnico não encontrado! Id: 999",
  "path": "/tecnicos/999"
}
```

## 📝 Endpoints Disponíveis

### Técnicos

- `GET /tecnicos` - Listar todos
- `GET /tecnicos/{id}` - Buscar por ID
- `POST /tecnicos` - Criar
- `PUT /tecnicos/{id}` - Atualizar
- `DELETE /tecnicos/{id}` - Deletar

### Clientes

- `GET /clientes` - Listar todos
- `GET /clientes/{id}` - Buscar por ID
- `POST /clientes` - Criar
- `PUT /clientes/{id}` - Atualizar
- `DELETE /clientes/{id}` - Deletar

### Chamados

- `GET /chamados` - Listar todos
- `GET /chamados/{id}` - Buscar por ID
- `POST /chamados` - Criar
- `PUT /chamados/{id}` - Atualizar

### Documentação

- `GET /swagger-ui.html` - Interface Swagger UI
- `GET /v3/api-docs` - Documentação OpenAPI em JSON

## 📄 Licença

Este projeto é um exemplo educacional para demonstração de conceitos Spring Boot.

## 👨‍💻 Autor

Desenvolvido como projeto de aprendizado em Spring Boot.

---

**Nota:** Certifique-se de que o banco de dados está configurado e a aplicação está rodando antes de testar os endpoints no Postman.
