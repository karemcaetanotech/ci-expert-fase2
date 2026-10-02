\# CI-EXPERT – Fase 2



Projeto de verificação funcional do núcleo RVX, baseado na arquitetura RISC-V RV32I, utilizando SystemVerilog e UVM.



\## Autora



Karem Caetano



\## Objetivo



Desenvolver uma metodologia de verificação funcional para o RVX Core, inicialmente concentrada nas operações executadas pela Unidade Lógica e Aritmética (ALU).



O ambiente de verificação será estruturado com base nos requisitos da especificação oficial RISC-V RV32I e na implementação real do núcleo em RTL.



\## Escopo atual



\- Estudo da arquitetura do RVX Core e da ALU;

\- Análise do arquivo `rvx\_core.v`;

\- Levantamento das instruções computacionais do RV32I;

\- Elaboração do Test Plan;

\- Definição da arquitetura do ambiente UVM;

\- Desenvolvimento de sequences, driver, monitor, scoreboard e cobertura funcional;

\- Registro dos resultados e das métricas de verificação.



A análise de injeção de falhas e das versões redundantes do RVX será realizada posteriormente, conforme a evolução e a viabilidade do projeto.



\## Estrutura do repositório



\- `docs/`: documentação, metodologia, diagramas e Test Plan;

\- `papers/`: artigos e referências bibliográficas;

\- `verification/`: componentes e testes do ambiente UVM;

\- `scripts/`: scripts de compilação, simulação e automação;

\- `results/`: resultados, logs e métricas;

\- `fault-injection/`: materiais relacionados à futura etapa de injeção de falhas.



\## Etapa atual



O projeto encontra-se na etapa de estudo da especificação RV32I, identificação das operações da ALU e elaboração inicial do Test Plan.

