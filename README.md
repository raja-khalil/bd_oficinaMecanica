# Esquema Conceitual para Sistema de Controle de Ordens de Serviço - Oficina Mecânica

## Descrição Geral
Este projeto consiste no desenvolvimento de um esquema conceitual de um sistema para controle e gerenciamento de ordens de serviço (OS) em uma oficina mecânica. O sistema abrange o controle de clientes, veículos, mecânicos, serviços, peças e equipes, além das ordens de serviço e seus respectivos detalhes de execução.

## Estrutura do Banco de Dados

### Tabelas Principais e Relacionamentos

1. **Cliente**
   - Armazena os dados dos clientes.
   - **Colunas**:
     - `ClienteID` (INT, PK, Auto Increment): Identificador único de cada cliente.
     - `Nome` (VARCHAR(100), NOT NULL): Nome do cliente.
     - `Endereco` (VARCHAR(150)): Endereço do cliente.
     - `Telefone` (VARCHAR(15)): Telefone de contato do cliente.
   - **Relacionamento**:
     - Relacionamento 1:N com `Veiculo`.

2. **Veiculo**
   - Representa os veículos dos clientes.
   - **Colunas**:
     - `VeiculoID` (INT, PK, Auto Increment): Identificador único do veículo.
     - `Placa` (VARCHAR(10), NOT NULL): Placa do veículo.
     - `Modelo` (VARCHAR(50), NOT NULL): Modelo do veículo.
     - `Ano` (YEAR, NOT NULL): Ano de fabricação do veículo.
     - `ClienteID` (INT, FK, NOT NULL): FK para `Cliente`.
   - **Relacionamento**:
     - Relacionamento 1:N com `OrdemServico`.

3. **Mecanico**
   - Contém informações dos mecânicos.
   - **Colunas**:
     - `MecanicoID` (INT, PK, Auto Increment): Identificador único do mecânico.
     - `Nome` (VARCHAR(100), NOT NULL): Nome do mecânico.
     - `Endereco` (VARCHAR(150)): Endereço do mecânico.
     - `Especialidade` (VARCHAR(50), NOT NULL): Especialidade do mecânico.
   - **Relacionamento**:
     - Relacionamento 1:N com `Equipe`.

4. **Equipe**
   - Designa uma equipe de mecânicos para cada OS.
   - **Colunas**:
     - `EquipeID` (INT, PK, Auto Increment): Identificador único da equipe.
   - **Relacionamento**:
     - Relacionamento 1:N com `Mecanico`.
     - Relacionamento 1:N com `OrdemServico`.

5. **OrdemServico (OS)**
   - Gerencia as ordens de serviço de cada veículo.
   - **Colunas**:
     - `OS_ID` (INT, PK, Auto Increment): Número da OS.
     - `DataEmissao` (DATE, NOT NULL): Data de emissão da OS.
     - `Status` (ENUM: "Aberto", "Em Progresso", "Concluído"): Status da OS.
     - `ValorTotal` (DECIMAL(10,2)): Valor total da OS (serviços e peças).
     - `DataConclusao` (DATE): Data de conclusão dos trabalhos.
     - `VeiculoID` (INT, FK, NOT NULL): FK para `Veiculo`.
     - `EquipeID` (INT, FK, NOT NULL): FK para `Equipe`.
   - **Relacionamento**:
     - Relacionamento N:M com `ServicoReferencia` através de `OS_Servico`.
     - Relacionamento N:M com `Peca` através de `OS_Peca`.

6. **ServicoReferencia**
   - Contém dados sobre os serviços oferecidos na oficina.
   - **Colunas**:
     - `ServicoID` (INT, PK, Auto Increment): Identificador único do serviço.
     - `Descricao` (VARCHAR(100), NOT NULL): Descrição do serviço (ex.: "Troca de óleo").
     - `ValorMaoDeObra` (DECIMAL(10,2), NOT NULL): Valor da mão de obra.
   - **Relacionamento**:
     - Relacionamento N:M com `OrdemServico` através de `OS_Servico`.

7. **OS_Servico**
   - Tabela de associação entre `OrdemServico` e `ServicoReferencia`.
   - **Colunas**:
     - `OS_ID` (INT, FK, NOT NULL): FK para `OrdemServico`.
     - `ServicoID` (INT, FK, NOT NULL): FK para `ServicoReferencia`.
     - `Quantidade` (INT, NOT NULL): Quantidade de vezes que o serviço foi executado.

8. **Peca**
   - Lista as peças utilizadas em serviços.
   - **Colunas**:
     - `PecaID` (INT, PK, Auto Increment): Identificador único da peça.
     - `Descricao` (VARCHAR(100), NOT NULL): Descrição da peça (ex.: "Filtro de óleo").
     - `ValorUnitario` (DECIMAL(10,2), NOT NULL): Valor unitário da peça.
   - **Relacionamento**:
     - Relacionamento N:M com `OrdemServico` através de `OS_Peca`.

9. **OS_Peca**
   - Tabela de associação entre `OrdemServico` e `Peca`.
   - **Colunas**:
     - `OS_ID` (INT, FK, NOT NULL): FK para `OrdemServico`.
     - `PecaID` (INT, FK, NOT NULL): FK para `Peca`.
     - `Quantidade` (INT, NOT NULL): Quantidade da peça utilizada.

## Observações Importantes
- **Relacionamentos**: Este projeto utiliza diferentes tipos de relacionamentos:
  - `Cliente` e `Veiculo`: 1:N
  - `Equipe` e `Mecanico`: 1:N
  - `OrdemServico` e `ServicoReferencia`: N:M (com tabela intermediária `OS_Servico`)
  - `OrdemServico` e `Peca`: N:M (com tabela intermediária `OS_Peca`)

- **Gerenciamento Automático**: Algumas FK e tabelas intermediárias serão geradas automaticamente pelo Workbench ao definir os relacionamentos.

## Instruções de Exportação
Para exportar o esquema para SQL, vá em `File > Export > Forward Engineer SQL CREATE Script...` no MySQL Workbench. Esse script SQL representa a estrutura completa do sistema para uso em ambiente de produção.

---

Este arquivo fornece uma visão completa da estrutura e dos relacionamentos entre as entidades do sistema de ordens de serviço para uma oficina mecânica. Certifique-se de verificar se os dados nos campos e relacionamentos atendem aos requisitos operacionais da oficina.

