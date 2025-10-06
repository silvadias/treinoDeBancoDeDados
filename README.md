# treinoDeBancoDeDados
Ambiente inicial para treinamento de banco de dados.

# 📊 Primeiros passos no SQLPad



## 1. Subir o Docker-composer e Fazer login no SQLPad  
  
1. É requisito ter o docker e docker-compose instalado no sistema.  
2. Clone este repositório ou cole o arquivo YML numa pasta do seu computador.  

---

## 🐳 Arquivo `docker-compose.yml`

```yaml
version: '3.8'

services:
  sqlserver:
    image: mcr.microsoft.com/mssql/server:2019-latest
    container_name: sqlserver
    environment:
      - SA_PASSWORD=Treino123!
      - ACCEPT_EULA=Y
      - MSSQL_PID=Developer        
    ports:
      - "1433:1433"
    volumes:
      - dadosSqlServer:/var/opt/mssql
      - ./scriptsSqlServer:/scripts

  sqlpad:
    image: sqlpad/sqlpad:latest
    container_name: sqlpad
    environment:
      - SQLPAD_ADMIN=Treino
      - SQLPAD_ADMIN_PASSWORD=Treino
    ports:
      - "3000:3000"
    depends_on:
      - sqlserver

volumes:
  dadosSqlServer:

networks:
  mssqlTreino:
    driver: bridge
```


3. Dentro da pasta, abra o terminal e escreva o comando:  
  
```bash
docker-compose up -d  
```
4. Aguarde o sistema criar as imagens e subir os serviços  
5. Abra [http://localhost:3000](http://localhost:3000)

Use:
- **Usuário**: `Treino`  
- **Senha**: `Treino`

---

## 2. ⚠️ Configurar primeira conexão
Após login:

1. Clique em **Manage Connections** (Gerenciar Conexões)  
2. Clique em **Add Connection** (Adicionar Conexão)  
3. Preencha os dados:

```yaml
Name: SQL Server Treino (Obs: Dê qualquer nome que considerar pertinente) 
Driver: Microsoft SQL Server
Host: sqlserver
Port: 1433
Database: master
Username: sa
Password: Treino123!
```

---

## 3. Testar a conexão
- Clique em **Test Connection**  
- Se tudo estiver verde, clique em **Save**

---

## 4. Executar primeira query
1. Clique em **New Query**  
2. Selecione a conexão `SQL Server Treino`  
3. Digite e rode:

```sql
SELECT name FROM sys.databases;
```

---

## 🗃 Bancos de dados disponíveis
- `master` - Banco de sistema principal  
- `tempdb` - Banco temporário  
- `model` - Modelo para novos bancos  
- `msdb` - Banco de jobs e alertas  

---

## 📁 Estrutura do projeto

```
./
├── docker-compose.yml
├── scriptsSqlServer/     # Pasta para seus scripts SQL
└── dadosSqlServer/       # Dados persistentes (volume Docker)
```

- **dadosSqlServer**: Volume Docker que mantém os dados do SQL Server  
- **scriptsSqlServer**: Pasta local para salvar seus scripts SQL  

---

## 🛑 Comandos úteis

Parar o ambiente:
```bash
docker-compose down
```

Parar e remover dados:
```bash
docker-compose down -v
```

Reiniciar o ambiente:
```bash
docker-compose restart
```

Ver logs:
```bash
docker-compose logs sqlserver
docker-compose logs sqlpad
```

---

## 🎯 Exemplos de queries para treino

Criar novo banco:
```sql
CREATE DATABASE MeuBancoTreino;
```

Criar tabela:
```sql
USE MeuBancoTreino;
CREATE TABLE Clientes (
    ID int PRIMARY KEY,
    Nome varchar(100),
    Email varchar(100)
);
```

Inserir dados:
```sql
INSERT INTO Clientes (ID, Nome, Email) 
VALUES (1, 'João Silva', 'joao@email.com');
```

Consultar dados:
```sql
SELECT * FROM Clientes;
```

---

## 🔧 Solução de problemas

### Erro de conexão no SQLPad
- Verifique se o container do SQL Server está rodando:  
  ```bash
  docker ps
  ```
- Aguarde **30-60 segundos** após `docker-compose up` para o SQL Server inicializar  

### Senha não funciona
Use exatamente:
- **SQLPad**: `Treino` / `Treino`  
- **SQL Server**: `sa` / `Treino123!`  

### Porta já em uso
Edite o `docker-compose.yml` e altere as portas:
```yaml
ports:
  - "1434:1433"   # Mude a primeira porta
```

### Não encontra "Manage Connections"
- Após login, procure no menu lateral ou superior por **Connections** ou **Manage Connections**  
- Em português pode aparecer como **Gerenciar Conexões**  

---

## 📚 Próximos passos para aprendizado
- ✅ Configurar ambiente  
- ✅ Conectar ao SQL Server  
- ✅ Executar queries básicas  
- 🎯 Criar bancos e tabelas  
- 🎯 Aprender `INSERT`, `UPDATE`, `DELETE`  
- 🎯 Praticar `SELECT` com `WHERE`, `JOIN`  
- 🎯 Estudar stored procedures e functions  

---

## ⚠️ Avisos importantes
- Este ambiente é **APENAS PARA TREINO**  
- Não use em produção  
- As senhas são simples para facilitar o aprendizado  
- Os dados persistem até você rodar:  

```bash
docker-compose down -v
```
