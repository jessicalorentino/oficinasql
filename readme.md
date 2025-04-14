```markdown
# Projeto Banco de Dados Oficina

Este é um projeto para a criação de um banco de dados para uma oficina mecânica. O objetivo é modelar e implementar o banco de dados para gerenciar as informações dos clientes, veículos, serviços, ordens de serviço e pagamentos.

## Tabelas Criadas

O banco de dados contém as seguintes tabelas:

- **Clientes**: Armazena os dados dos clientes da oficina (nome, e-mail, telefone).
- **Funcionários**: Contém as informações dos funcionários da oficina.
- **Veículos**: Dados sobre os veículos dos clientes.
- **Serviços**: Lista os serviços oferecidos pela oficina (troca de óleo, alinhamento, etc).
- **Ordens de Serviço**: Registra as ordens de serviço feitas pelos clientes.
- **Itens de OS**: Detalha os serviços realizados dentro de uma ordem de serviço.
- **Pagamentos**: Contém os pagamentos realizados pelos clientes.

## Esquema Relacional

Aqui estão os scripts para criar o banco de dados e as tabelas.

### Criando o Banco de Dados e Tabelas

```sql
CREATE DATABASE oficina;

USE oficina;

CREATE TABLE clientes (
    cliente_id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL,
    telefone VARCHAR(15) NOT NULL
);

CREATE TABLE funcionarios (
    funcionario_id INT PRIMARY KEY AUTO_INCREMENT,
    nome VARCHAR(100) NOT NULL,
    cargo VARCHAR(50) NOT NULL
);

CREATE TABLE veiculos (
    veiculo_id INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT,
    modelo VARCHAR(50),
    placa VARCHAR(10) UNIQUE,
    FOREIGN KEY (cliente_id) REFERENCES clientes(cliente_id)
);

CREATE TABLE servicos (
    servico_id INT PRIMARY KEY AUTO_INCREMENT,
    descricao VARCHAR(100) NOT NULL,
    valor DECIMAL(10,2) NOT NULL
);

CREATE TABLE ordens_servico (
    os_id INT PRIMARY KEY AUTO_INCREMENT,
    veiculo_id INT,
    funcionario_id INT,
    data DATE,
    status VARCHAR(20),
    FOREIGN KEY (veiculo_id) REFERENCES veiculos(veiculo_id),
    FOREIGN KEY (funcionario_id) REFERENCES funcionarios(funcionario_id)
);

CREATE TABLE itens_os (
    os_id INT,
    servico_id INT,
    quantidade INT,
    preco_total DECIMAL(10,2),
    PRIMARY KEY (os_id, servico_id),
    FOREIGN KEY (os_id) REFERENCES ordens_servico(os_id),
    FOREIGN KEY (servico_id) REFERENCES servicos(servico_id)
);

CREATE TABLE pagamentos (
    pagamento_id INT PRIMARY KEY AUTO_INCREMENT,
    os_id INT,
    data_pagamento DATE,
    valor_pago DECIMAL(10,2),
    FOREIGN KEY (os_id) REFERENCES ordens_servico(os_id)
);
```

### Inserindo Dados de Exemplo

Aqui estão alguns dados de exemplo para começar a testar o banco de dados.

```sql

INSERT INTO clientes (nome, email, telefone) VALUES
('Carlos Silva', 'carlos@gmail.com', '123456789'),
('Maria Oliveira', 'maria@yahoo.com', '987654321');


INSERT INTO funcionarios (nome, cargo) VALUES
('João Santos', 'Mecânico'),
('Ana Pereira', 'Assistente');

INSERT INTO veiculos (cliente_id, modelo, placa) VALUES
(1, 'Fusca', 'ABC1234'),
(2, 'Civic', 'XYZ5678');

INSERT INTO servicos (descricao, valor) VALUES
('Troca de Óleo', 100.00),
('Alinhamento de Rodas', 150.00);

INSERT INTO ordens_servico (veiculo_id, funcionario_id, data, status) VALUES
(1, 1, '2025-04-10', 'Concluído'),
(2, 2, '2025-04-11', 'Em andamento');

INSERT INTO itens_os (os_id, servico_id, quantidade, preco_total) VALUES
(1, 1, 1, 100.00),
(2, 2, 1, 150.00);

INSERT INTO pagamentos (os_id, data_pagamento, valor_pago) VALUES
(1, '2025-04-10', 100.00);
```

---

## Consultas SQL

Aqui estão algumas consultas SQL que você pode usar para testar o banco de dados.

### 1. Seleção de todos os clientes

```sql
SELECT * FROM clientes;
```

### 2. Selecionar ordens de serviço com status "Concluído"

```sql
SELECT * FROM ordens_servico
WHERE status = 'Concluído';
```

### 3. Calcular o total pago por ordem de serviço

```sql
SELECT os_id, SUM(preco_total) AS total_pago
FROM itens_os
GROUP BY os_id;
```

### 4. Ordenar os clientes pelo nome

```sql
SELECT nome, telefone FROM clientes
ORDER BY nome;
```

### 5. Selecionar ordens de serviço com total superior a 100

```sql
SELECT os_id, SUM(preco_total) AS total_os
FROM itens_os
GROUP BY os_id
HAVING total_os > 100;
```

### 6. Mostrar detalhes da ordem de serviço, incluindo o nome do cliente, modelo do veículo e descrição do serviço realizado

```sql
SELECT c.nome AS cliente, v.modelo AS veiculo, s.descricao AS servico
FROM ordens_servico os
JOIN veiculos v ON os.veiculo_id = v.veiculo_id
JOIN clientes c ON v.cliente_id = c.cliente_id
JOIN itens_os io ON os.os_id = io.os_id
JOIN servicos s ON io.servico_id = s.servico_id;
```

---

## Como Usar

1. **Clone o repositório**:
   ```bash
   git clone https://github.com/SEU-USUARIO/oficina-bd.git
   ```

2. **Execute o script SQL** em um banco de dados MySQL ou MariaDB para criar as tabelas.

3. **Insira os dados** de exemplo usando o script de inserção.

4. **Teste as consultas SQL** para verificar se o banco de dados está funcionando corretamente.

---



