# Scripts para SQL Server

Comandos básicos para gerenciamento do SQLServer.

```sql
-- Deletar um usuário especifico
DROP USER nome_usuario;

-- Criar um novo usuário com base no login do servidor
CREATE USER nome_usuario FOR LOGIN nome_usuario;

-- Conceder permissão de leitura
GRANT SELECT TO nome_usuario;

-- Conceder permissão de escrita
GRANT INSERT, UPDATE, DELETE TO nome_usuario;

-- Alterar a senha do usuario
ALTER LOGIN [nome_usuario] WITH PASSWORD = '123456asdfg';
```

Script para dar permissão para um usuário em todos os banco de dados de um SQL Server específico.

```sql
-- Percorre cada banco de dados e adiciona o usuário à função db_owner
USE master;
EXEC sp_MSforeachdb
N'USE [?];
IF DB_ID(''?'') > 4 -- Avoid system databases (master, model, msdb, tempdb)
BEGIN
    IF NOT EXISTS (SELECT * FROM sys.database_principals WHERE name = ''nome_usuario'')
    BEGIN
        CREATE USER [nome_usuario] FOR LOGIN [nome_usuario];
    END
    ALTER ROLE [db_owner] ADD MEMBER [nome_usuario];
END'
```
Este script percorre todas as bases de dados no servidor, exceto as bases de dados do sistema (master, model, msdb, tempdb). Para cada base de dados de usuário, ele verifica se o usuário daniela_jesus já existe. Se não existir, o script cria o usuário e, em seguida, adiciona este usuário ao papel db_owner para que tenha permissões administrativas nessa base de dados.