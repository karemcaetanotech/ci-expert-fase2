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

### TP-ALU-003 — ADDI

**Requisito da ISA:**  
A instrução `ADDI` deve somar o valor de 32 bits armazenado em `rs1` a um imediato de 12 bits estendido com sinal. O resultado gravado em `rd` deve conter os 32 bits menos significativos da soma. O overflow deve ser ignorado.

**Correspondência no RTL:**  
A operação utiliza `FUNCT3_ADD`, com o primeiro operando vindo de `rs1_data` e o segundo operando vindo do valor imediato estendido com sinal.

**Estímulos:**

- `0 + 0`
- `10 + 5`
- `10 + (-5)`
- `0 + 2047`
- `0 + (-2048)`
- `0x7FFFFFFF + 1`
- `0xFFFFFFFF + 1`

**Resultados esperados:**

- `0 + 0 = 0`
- `10 + 5 = 15`
- `10 + (-5) = 5`
- `0 + 2047 = 0x000007FF`
- `0 + (-2048) = 0xFFFFF800`
- `0x7FFFFFFF + 1 = 0x80000000`
- `0xFFFFFFFF + 1 = 0x00000000`

**Casos de borda:**

- Imediato igual a zero.
- Maior imediato positivo: `2047`.
- Menor imediato negativo: `-2048`.
- Extensão de sinal de valores negativos.
- Overflow com descarte dos bits acima de 32 bits.

**Forma de checagem:**  
Estender o imediato de 12 bits com sinal, calcular o resultado esperado no scoreboard e comparar com o valor produzido pela ALU e gravado em `rd`.

**Cobertura:**

- Execução da instrução `ADDI`.
- Imediatos zero, positivos e negativos.
- Limites do imediato de 12 bits.
- Resultado zero, positivo e negativo.
- Overflow.

### TP-ALU-004 — AND

**Requisito da ISA:**  
A instrução `AND` deve realizar a operação lógica AND bit a bit entre os valores de 32 bits armazenados em `rs1` e `rs2`. O resultado deve ser gravado em `rd`.

**Correspondência no RTL:**  
A operação utiliza `FUNCT3_AND`, com os operandos provenientes de `rs1_data` e `rs2_data`.

**Estímulos:**

- `0x00000000 AND 0x00000000`
- `0x00000000 AND 0xFFFFFFFF`
- `0xFFFFFFFF AND 0xFFFFFFFF`
- `0xAAAAAAAA AND 0x55555555`
- `0xF0F0F0F0 AND 0x0F0F0F0F`
- `0xFF00FF00 AND 0x0F0F0F0F`

**Resultados esperados:**

- `0x00000000`
- `0x00000000`
- `0xFFFFFFFF`
- `0x00000000`
- `0x00000000`
- `0x0F000F00`

**Casos de borda:**

- Ambos os operandos iguais a zero.
- Um operando igual a zero.
- Todos os bits iguais a um.
- Padrões de bits alternados.
- Operandos com bits parcialmente coincidentes.

**Forma de checagem:**  
Calcular no scoreboard o AND bit a bit entre `rs1_data` e `rs2_data` e comparar com o valor produzido pela ALU e gravado em `rd`.

**Cobertura:**

- Execução da instrução `AND`.
- Resultado com todos os bits em zero.
- Resultado com todos os bits em um.
- Resultado com combinação de bits zero e um.
- Padrões alternados e parcialmente coincidentes.



\*\*Forma de checagem:\*\*  

Calcular o resultado esperado no scoreboard e comparar com o valor produzido pela ALU e gravado em `rd`.



\*\*Cobertura:\*\*



\- Execução da instrução `SUB`.

\- Operandos zero, positivos e negativos.

\- Resultado zero, positivo e negativo.

\- Underflow e retorno ao maior valor positivo.

