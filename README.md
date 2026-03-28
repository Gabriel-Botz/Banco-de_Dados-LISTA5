# 📚 Sistema de Biblioteca - Índices e Views

## 📖 Contextualização

Em sistemas de banco de dados, especialmente aqueles que lidam com grande volume de informações como bibliotecas, é fundamental garantir que as consultas sejam executadas de forma rápida e eficiente. Para isso, utilizam-se recursos como índices e views, que ajudam tanto na performance quanto na organização dos dados.

Neste trabalho, foram aplicadas técnicas de otimização utilizando índices, além da criação de uma view para facilitar a visualização do histórico de empréstimos. Também foi analisado o impacto dessas otimizações na performance das consultas.

---

## 🎯 Objetivo

* Criar índices para otimizar consultas em tabelas;
* Implementar uma view para facilitar a visualização de dados;
* Compreender o funcionamento dos índices;
* Analisar a performance de consultas com e sem índices.

---

## 🛠️ Tecnologias Utilizadas

* SQL
* PostgreSQL (ou outro SGBD relacional)

---

## 📌 Atividades Desenvolvidas

### 1. Criação de índice para busca por título de livro

```sql
CREATE INDEX idx_livro_titulo
ON biblioteca.livro(titulo);
```

---

### 2. Criação de índice para data de empréstimo

```sql
CREATE INDEX idx_emprestimo_data
ON biblioteca.emprestimo(data_emprestimo);
```

---

### 3. Criação da VIEW de histórico de empréstimos

```sql
CREATE VIEW biblioteca.vw_historico_emprestimos AS
SELECT 
    u.nome AS nome_usuario,
    l.titulo AS titulo_livro,
    e.data_emprestimo,
    e.data_devolucao
FROM biblioteca.emprestimo e
JOIN biblioteca.usuario u 
     ON e.id_usuario = u.id_usuario
JOIN biblioteca.livro l 
     ON e.id_livro = l.id_livro;
```

Consulta da view:

```sql
SELECT * FROM biblioteca.vw_historico_emprestimos;
```

---

### 4. Explicação sobre índices

Os índices funcionam de maneira semelhante ao índice de um livro, permitindo que o banco de dados encontre informações de forma mais rápida.

* **Sem índice:** o banco realiza uma varredura completa na tabela (FULL SCAN), analisando linha por linha, o que pode ser lento em tabelas grandes.
* **Com índice:** o banco consegue localizar os dados diretamente, reduzindo o tempo de busca.

#### ⚠️ Impactos negativos do uso excessivo de índices:

* Aumentam o consumo de armazenamento;
* Podem deixar operações de inserção (`INSERT`), atualização (`UPDATE`) e exclusão (`DELETE`) mais lentas;
* Exigem manutenção adicional pelo banco de dados.

---

### 5. Teste de performance com EXPLAIN ANALYZE

#### 🔴 Sem índice

```sql
DROP INDEX IF EXISTS biblioteca.idx_livro_titulo;

EXPLAIN ANALYZE
SELECT *
FROM biblioteca.livro
WHERE titulo = 'A Sombra do Amanhã';
```

---

#### 🟢 Com índice

```sql
CREATE INDEX idx_livro_titulo
ON biblioteca.livro(titulo);

ANALYZE biblioteca.livro;

EXPLAIN ANALYZE
SELECT *
FROM biblioteca.livro
WHERE titulo = 'A Sombra do Amanhã';
```

---

## 📊 Resultados Esperados

Com a aplicação dos índices, espera-se:

* Redução no tempo de execução das consultas;
* Melhoria na eficiência da busca por dados específicos;
* Melhor organização e reutilização de consultas através da view.

---

## ✅ Conclusão

A utilização de índices e views é essencial para otimizar o desempenho e a organização de um banco de dados. Neste exercício, foi possível observar na prática como os índices impactam diretamente na velocidade das consultas, além de facilitar o acesso a informações através de uma view estruturada.

Esses conceitos são amplamente utilizados no desenvolvimento de sistemas reais, sendo fundamentais para garantir eficiência e escalabilidade em aplicações que manipulam grandes volumes de dados.

---

## 📎 Observação

Os resultados de performance podem variar de acordo com o volume de dados presente no banco e a configuração do sistema.
