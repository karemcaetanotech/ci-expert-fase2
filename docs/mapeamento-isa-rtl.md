# Mapeamento entre a ISA RV32I e o RTL da ALU do RVX

## 1. Objetivo

Relacionar as operações computacionais da especificação RV32I com a implementação da ALU presente no arquivo `rtl/rvx_core.v`.

## 2. Sinais principais

| Sinal | Função |
|---|---|
| `rs1_data` | Primeiro operando da ALU |
| `rs2_data` | Segundo operando das instruções entre registradores |
| `immediate` | Segundo operando das instruções imediatas |
| `alu_2nd_operand_source` | Seleciona `rs2_data` quando vale 1 e `immediate` quando vale 0 |
| `alu_operation_code[2:0]` | Seleciona a operação por meio do código `funct3` |
| `alu_operation_code[3]` | Diferencia ADD/SUB e SRL/SRA |
| `alu_output` | Resultado produzido pela ALU |

## 3. Seleção do segundo operando

O RTL seleciona o segundo operando da seguinte forma:

```verilog
assign alu_2nd_operand =
  alu_2nd_operand_source ?
  rs2_data :
  immediate;