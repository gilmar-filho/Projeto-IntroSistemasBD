# Projeto de Banco de Dados para Jogo de RPG

![Status: Concluído](https://img.shields.io/badge/status-concluído-brightgreen)

Projeto acadêmico da disciplina "GCC 263 - Introdução a Sistemas de Bancos de Dados" da Universidade Federal de Lavras (UFLA). O objetivo foi modelar e implementar um banco de dados completo para um jogo de RPG, aplicando conceitos avançados de SQL e desenvolvendo uma interface web simples para gerenciamento.

## Funcionalidades Implementadas

### 1. Modelagem do Banco de Dados
- **Entidades Principais**: `Jogavel`, `NaoJogavel`, `Classe`, `Missao`, `Item` (comum e único).
- **Herança**: Implementação do conceito de herança para as `Classes` de personagens (Guerreiro, Arqueiro, Mago, etc.).
- **Relacionamentos**: Definição de relacionamentos complexos, como o N:M entre jogadores que realizam missões (`Realiza`) e jogadores que compram itens (`Compra`).

### 2. Lógica de Negócio com Triggers
O banco de dados utiliza gatilhos para garantir a integridade e automatizar regras de negócio do jogo:
- `AtualizarSaldoAposCompra`: Debita automaticamente o valor de um item do saldo do jogador após uma compra ser registrada.
- `before_insert_jogavel`: Impede a criação de personagens com saldo negativo.
- `before_update_xpJogador`: Garante que a experiência (XP) de um jogador nunca seja reduzida.
- `VerificarVendedorMercador`: Assegura que apenas NPCs do tipo 'Mercador' possam ser associados à venda de itens comuns.
- `VerificarMercadorEEstoque`: Valida se uma compra está sendo feita de um 'Mercador' e se ele possui o item em estoque.

### 3. Consultas, Views e Rotinas Armazenadas
- **Consultas Avançadas**: O arquivo `sql/letra_f.sql` contém dezenas de exemplos de consultas, utilizando `JOIN`, `GROUP BY`, `HAVING`, `UNION`, `ANY`, `ALL`, `EXISTS` e subqueries para extrair informações ricas do banco de dados.
- **Views**: Foram criadas views para simplificar o acesso a dados recorrentes:
  - `View_Jogadores_Classes`: Lista os jogadores e o nome de suas classes.
  - `View_Missoes_Nao_Realizadas`: Mostra missões que ainda não foram completadas por nenhum jogador.
  - `View_Itens_Comuns_Precos`: Exibe os itens à venda, seu preço e o nome do vendedor.
- **Stored Procedures e Functions**:
  - `ClassificarDificuldadeMissao(id)`: Retorna a dificuldade de uma missão ('Fácil', 'Média', 'Difícil') com base no XP.
  - `CalcularNivelJogador(id)`: Calcula o nível de um personagem com base na sua experiência acumulada.
  - `ClassificarJogadores()`: Gera um ranking de jogadores ('Lendário', 'Veterano', etc.) baseado no XP.
  - `ListarItensComunsEVendedores()`: Lista todos os itens comuns disponíveis e seus respectivos vendedores (NPCs).

### 4. Interface Web com PHP
Uma interface web simples foi desenvolvida para interagir com o banco de dados, permitindo:
- **Listar** todos os personagens jogáveis.
- **Incluir** novos personagens no jogo.
- **Editar** informações de personagens existentes.
- **Excluir** personagens do banco de dados.

**Nota sobre Segurança**: O código PHP implementa SQL Injection de forma proposital para fins didáticos e educacionais, demonstrando vulnerabilidades de segurança comuns. Em aplicações em produção, deve-se sempre usar prepared statements para prevenir este tipo de ataque.

## Tecnologias Utilizadas
- **Banco de Dados**: MySQL
- **Backend**: PHP
- **Frontend**: HTML

## Como Executar o Projeto

1.  **Pré-requisitos**:
    - É necessário um ambiente de servidor local como XAMPP, WAMP ou MAMP, que inclua Apache, MySQL e PHP.

2.  **Configuração do Banco de Dados**:
    - Inicie o seu servidor MySQL.
    - Crie um novo banco de dados com o nome `projetoISBD`.
    - Execute os scripts da pasta `sql/` na seguinte ordem para criar a estrutura e popular o banco:
      1. `sql/letra_a.sql` (Exemplos de operações DDL com ALTER TABLE e DROP TABLE - opcional para aprendizado).
      2. `sql/letra_b.sql` (Criação das tabelas: Classe, Jogavel, NaoJogavel, Missao, Realiza, Item, Comum, Unico, Compra).
      3. `sql/letra_c.sql` (Inserção de dados de exemplo e criação de alguns triggers iniciais).
      4. `sql/letra_j.sql` (Criação dos demais triggers para garantir integridade de dados).
      5. `sql/letra_g.sql` (Criação das Views para facilitar consultas recorrentes).
      6. `sql/letra_i.sql` (Criação das Stored Procedures e Functions para lógica de negócio).
      
    **Motivo da Ordem**:
    - `letra_a.sql` é opcional e apenas exemplar
    - `letra_b.sql` deve ser executado primeiro para criar a estrutura base
    - `letra_c.sql` e `letra_j.sql` juntos garantem que todos os triggers estejam em place antes de qualquer operação
    - `letra_g.sql` cria views que facilitam consultas nos dados já inseridos
    - `letra_i.sql` por último, pois as procedures podem referenciar as views criadas

3.  **Configuração da Aplicação Web**:
    - Copie a pasta `php/` para o diretório raiz do seu servidor web (geralmente `htdocs` no XAMPP).
    - Verifique se as credenciais no arquivo `php/config.php` estão corretas para o seu ambiente MySQL (o padrão `root` com senha vazia já está configurado).
    - Abra seu navegador e acesse `http://localhost/php/index.php`.

## Estrutura do Projeto
```
./
├── php/              # Código-fonte da aplicação web
│   ├── config.php    # Configurações de conexão com o banco de dados
│   ├── index.php     # Página principal - listagem de personagens
│   ├── form_incluir.php  # Formulário para incluir/editar personagens
│   ├── incluir.php   # Processamento de inserção/atualização
│   └── excluir.php   # Processamento de exclusão
└── sql/              # Scripts para criação e manipulação do banco de dados
    ├── letra_a.sql   # Exemplos de ALTER TABLE e DROP TABLE (DDL)
    ├── letra_b.sql   # Criação das tabelas (DDL)
    ├── letra_c.sql   # Inserção de dados (DML) + alguns triggers
    ├── letra_d.sql   # Exemplos de UPDATE (DML)
    ├── letra_e.sql   # (Exemplos adicionais)
    ├── letra_f.sql   # Exemplos de consultas avançadas (SELECT)
    ├── letra_g.sql   # Criação de Views
    ├── letra_h.sql   # (Exemplos adicionais)
    ├── letra_i.sql   # Stored Procedures e Functions
    └── letra_j.sql   # Criação de Triggers
```

## Participantes
- [Gabriel Fagundes Mesquita Sousa](https://github.com/gabrafo)
- [Gilmar Silva de Medeiros Filho](https://github.com/gilmar-filho)
- [Hugo Dias Pontello](https://github.com/DPontello)
- Melissa de Carvalho Vila Real
- [Samuel de Oliveira Vanoni](https://github.com/SamuVanoni)