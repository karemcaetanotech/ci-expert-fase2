# Arquitetura do Ambiente de Verificação UVM da ALU do RVX

## 1. Objetivo

Representar a arquitetura inicial do ambiente UVM utilizado para verificar as operações da ALU do processador RVX conforme a especificação RV32I.

## 2. Arquitetura proposta

![Arquitetura do ambiente UVM para verificação da ALU do RVX](assets/arquitetura-uvm-alu-rvx.svg)

Arquivo editável: [arquitetura-uvm-alu-rvx.drawio](assets/arquitetura-uvm-alu-rvx.drawio)

## 3. Interface do DUT

O `alu_wrapper` permitirá acessar apenas a lógica da ALU implementada em `rtl/rvx_core.v`. A interface inicial deverá expor os seguintes sinais:

| Direção | Sinal | Função |
|---|---|---|
| Entrada | `rs1_data[31:0]` | Primeiro operando |
| Entrada | `rs2_data[31:0]` | Segundo operando das instruções entre registradores |
| Entrada | `immediate[31:0]` | Segundo operando das instruções imediatas |
| Entrada | `alu_2nd_operand_source` | Seleciona `rs2_data` ou `immediate` |
| Entrada | `alu_operation_code[3:0]` | Seleciona a operação e diferencia ADD/SUB e SRL/SRA |
| Saída | `alu_output[31:0]` | Resultado produzido pela ALU |

## 4. Componentes

- **Sequence:** cria os itens com operação e operandos.
- **Sequencer:** organiza e entrega os itens ao Driver.
- **Driver:** converte os itens em estímulos aplicados pela interface virtual.
- **Interface `alu_if`:** agrupa os sinais utilizados pelo Driver e pelo Monitor.
- **DUT `alu_wrapper`:** fornece acesso isolado à ALU existente no `rvx_core.v`.
- **Monitor:** amostra entradas e saída sem interferir no DUT e publica a transação observada.
- **Modelo de referência RV32I:** calcula o resultado esperado para cada operação.
- **Scoreboard:** compara o resultado observado com o esperado.
- **Coverage Subscriber:** registra operações, classes de operandos e casos de borda; a meta inicial é alcançar no mínimo 90% de cobertura.

## 5. Fluxo da verificação

1. A Sequence cria uma transação com a operação e os operandos.
2. O Sequencer entrega a transação ao Driver.
3. O Driver aplica os sinais ao `alu_wrapper` por meio da interface virtual.
4. O Monitor coleta os sinais de entrada e o resultado produzido.
5. O modelo de referência RV32I calcula o resultado esperado.
6. O Scoreboard compara o valor esperado com o valor observado.
7. O Coverage Subscriber registra os cenários exercitados.

## 6. Comunicação

A conexão entre Sequencer e Driver utiliza o mecanismo padrão de sequência do UVM. O Monitor distribui as transações para o modelo de referência, o Scoreboard e o Coverage Subscriber por portas de análise TLM 1.0.

## 7. Escopo inicial

O ambiente será desenvolvido inicialmente para a ALU e para as operações RV32I registradas no Test Plan. A cobertura de FSM não faz parte desta etapa. Após a validação da ALU, a estrutura poderá ser ampliada para outras partes do RVX e para os experimentos de injeção de falhas.
