\# Test Plan da ALU do RVX



\## 1. Objetivo



Verificar se as operações da Unidade Lógica e Aritmética do processador RVX estão de acordo com a especificação oficial RV32I.



\## 2. Referências



\- RISC-V RV32I Base Integer Instruction Set, seção 1.1.4 — Integer Computational Instructions.

\- Implementação da ALU no arquivo `rtl/rvx\_core.v`, linhas 1619 a 1681.



\## 3. Operações identificadas



\- Aritméticas: ADD, ADDI e SUB.

\- Lógicas: AND, ANDI, OR, ORI, XOR e XORI.

\- Comparações: SLT, SLTI, SLTU e SLTIU.

\- Deslocamentos: SLL, SLLI, SRL, SRLI, SRA e SRAI.



\## 4. Casos de teste



\### TP-ALU-001 — ADD



\*\*Requisito da ISA:\*\*  

A instrução `ADD` deve somar os valores de 32 bits armazenados em `rs1` e `rs2`. O resultado gravado em `rd` deve conter os 32 bits menos significativos da soma. O overflow deve ser ignorado.



\*\*Correspondência no RTL:\*\*  

A operação utiliza `FUNCT3\_ADD`, com o segundo operando vindo de `rs2\_data`.



\*\*Estímulos:\*\*



\- `0 + 0`

\- `1 + 1`

\- `10 + 20`

\- `-1 + 1`

\- `0x7FFFFFFF + 1`

\- `0xFFFFFFFF + 1`



\*\*Resultados esperados:\*\*



\- `0 + 0 = 0`

\- `1 + 1 = 2`

\- `10 + 20 = 30`

\- `-1 + 1 = 0`

\- `0x7FFFFFFF + 1 = 0x80000000`

\- `0xFFFFFFFF + 1 = 0x00000000`



\*\*Casos de borda:\*\*



\- Operandos iguais a zero.

\- Valores positivos máximos.

\- Valores negativos.

\- Overflow com descarte dos bits acima de 32 bits.



\*\*Forma de checagem:\*\*  

Calcular o resultado esperado no scoreboard e comparar com o valor produzido pela ALU e gravado em `rd`.



\*\*Cobertura:\*\*



\- Execução da instrução `ADD`.

\- Operandos zero, positivos e negativos.

\- Resultado zero.

\- Overflow e retorno ao valor zero.



\### TP-ALU-002 — SUB



\*\*Requisito da ISA:\*\*  

A instrução `SUB` deve subtrair o valor de 32 bits armazenado em `rs2` do valor armazenado em `rs1`. O resultado gravado em `rd` deve conter os 32 bits menos significativos. O overflow deve ser ignorado.



\*\*Correspondência no RTL:\*\*  

A operação utiliza `FUNCT3\_ADD`, com o controle de subtração habilitado e os operandos provenientes de `rs1\_data` e `rs2\_data`.



\*\*Estímulos:\*\*



\- `0 - 0`

\- `5 - 3`

\- `3 - 5`

\- `-1 - 1`

\- `0x80000000 - 1`

\- `0x00000000 - 1`



\*\*Resultados esperados:\*\*



\- `0 - 0 = 0`

\- `5 - 3 = 2`

\- `3 - 5 = -2` (`0xFFFFFFFE`)

\- `-1 - 1 = -2` (`0xFFFFFFFE`)

\- `0x80000000 - 1 = 0x7FFFFFFF`

\- `0x00000000 - 1 = 0xFFFFFFFF`



\*\*Casos de borda:\*\*



\- Operandos iguais a zero.

\- Resultado negativo.

\- Menor valor inteiro representável em 32 bits.

\- Underflow com descarte dos bits acima de 32 bits.



\*\*Forma de checagem:\*\*  

Calcular o resultado esperado no scoreboard e comparar com o valor produzido pela ALU e gravado em `rd`.



\*\*Cobertura:\*\*



\- Execução da instrução `SUB`.

\- Operandos zero, positivos e negativos.

\- Resultado zero, positivo e negativo.

\- Underflow e retorno ao maior valor positivo.

