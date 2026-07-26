# Aponti - MOD 01 - Aula 3 - Desafio - Testes Funcionais e Não Funcionais

## Parte 1 — Testes Funcionais

### Exercício 1 — Identificação das Funcionalidades

| Item | Descrição |
|-------|-----------|
| **Funcionalidade** | Cadastro de paciente |
| **Objetivo da funcionalidade** | Cadastrar um novo paciente no sistema para possibilitar o agendamento de consultas e o registro de seus dados. |
| **Usuário que utiliza a funcionalidade** | Recepcionista cadastrada. |
| **Dados necessários** | Nome completo do paciente, CPF, data de nascimento, telefone, endereço, convênio (quando houver) e demais informações obrigatórias para o cadastro. |
| **Resultado esperado** | Paciente cadastrado com sucesso no sistema, com todas as informações obrigatórias registradas e disponível para agendamento de consultas. |
| **Possíveis condições de erro** | Campos obrigatórios não preenchidos, CPF inválido ou já cadastrado, dados em formato incorreto ou falha na gravação das informações no sistema. |

# Tabela de Testes Unitários

| Função / Regra | Entrada | Resultado Esperado | Por que é unitário? |
|----------------|----------|--------------------|---------------------|
| **1. Cálculo do valor total de receitas** | Registros: **R$ 450,00** e **R$ 1.250,00** | **R$ 1.700,00** | Avalia isoladamente a regra de soma do total de entradas (receitas), sem realizar chamadas a banco de dados ou renderização da interface. |
| **2. Formatação de valores monetários** | `1250` ou `1250.0` | `"1.250,00"` ou `"R$ 1.250,00"` | Testa exclusivamente a função utilitária responsável por converter valores numéricos para o formato de moeda local. |
| **3. Validação de campos obrigatórios** | Descrição: `""`<br>Valor: `450.00` | `false` (erro de validação) | Avalia a lógica de validação dos campos obrigatórios, como impedir o cadastro de um registro sem descrição. |
| **4. Validação de formato de data** | `"2026-07-05"` | `true` (data válida) | Testa unicamente a regra de validação do formato da data informada em um registro de receita. |
| **5. Filtragem de receitas por pesquisa** | Termo: `"Vida"` | Retorna apenas as receitas que contêm o termo **"Vida"** | Avalia exclusivamente a lógica de filtragem de registros com base no termo pesquisado, sem depender da interface ou de banco de dados. |

## Justificativa Geral

Todos esses testes são considerados **unitários** porque verificam pequenas funções ou regras de negócio de forma totalmente isolada. Eles avaliam apenas a lógica interna do código (entradas e saídas), sem depender de cliques na interface, chamadas de API, integração com banco de dados ou renderização da aplicação.

# Exercício 3 — Testes de Integração

## Tabela de Testes de Integração

| Componentes Integrados | Ação / Cenário | Resultado Esperado | Risco Evitado |
|-------------------------|----------------|--------------------|----------------|
| **1. Agendamento + Agenda do Profissional** | Agendar uma nova consulta para um psicólogo em um horário específico. | O agendamento é criado e o horário fica marcado como **ocupado** na agenda do profissional. | Evita que dois pacientes sejam agendados no mesmo horário com o mesmo profissional (overbooking). |
| **2. Consulta Realizada + Lançamento Financeiro (Receitas)** | Marcar uma consulta agendada como **"Realizada"**. | O sistema gera automaticamente um lançamento de receita com o valor da consulta na área financeira. | Evita que consultas realizadas fiquem sem faturamento e sem registro financeiro. |
| **3. Receita/Despesa + Relatório Financeiro** | Adicionar uma nova receita ou registrar uma despesa. | O relatório financeiro (Dashboard/Indicadores) atualiza automaticamente os totais de receitas, despesas e saldo do período. | Evita relatórios desatualizados ou divergentes das movimentações registradas. |
| **4. Reagendamento + Liberação de Horário** | Alterar o horário de uma consulta para uma nova data ou horário. | O horário anterior é liberado na agenda e o novo horário é reservado para a consulta. | Evita que horários antigos permaneçam bloqueados, impedindo novos agendamentos. |
| **5. Login + Controle de Perfis e Permissões** | Realizar login com um perfil restrito (ex.: Recepcionista). | O sistema autentica o usuário e restringe o acesso às funcionalidades permitidas para seu perfil. | Evita acesso não autorizado a informações e funcionalidades confidenciais, como prontuários e evoluções. |

## Justificativa dos Testes de Integração

Diferentemente dos **testes unitários**, que verificam funções isoladas, os **testes de integração** têm como objetivo validar a comunicação entre dois ou mais componentes do sistema. Esses testes garantem que módulos, serviços, telas e banco de dados troquem informações corretamente, mantendo a consistência dos dados e o funcionamento adequado dos fluxos completos da aplicação.

## Exercício 4 — Testes de sistema

# Exercício 4 — Testes de Sistema

## Cenário A — Atendimento Completo

**Pré-condições:** Usuário autenticado e psicólogo cadastrado.

**Dados:** Paciente Carlos Eduardo Silva, consulta em **25/07/2026 às 14:00**, valor **R$ 150,00**.

**Passos:**
1. Cadastrar o paciente.
2. Agendar a consulta.
3. Registrar o check-in.
4. Registrar a evolução da sessão.
5. Lançar a receita da consulta.
6. Verificar o relatório financeiro.

**Resultado esperado:** Todo o fluxo é concluído corretamente e a receita de **R$ 150,00** aparece no relatório financeiro.

**Resultado obtido:** Fluxo executado com sucesso e saldo atualizado.

**Situação:** **Aprovado**

**Evidência:** Relatório financeiro exibindo a nova receita.

**Justificativa:** Teste de sistema que valida o fluxo completo de atendimento (End-to-End).

---

## Cenário B — Reagendamento

**Pré-condições:** Consulta já agendada.

**Dados:** Paciente Mariana Costa. Alteração de **27/07/2026 às 09:00** para **28/07/2026 às 11:00**.

**Passos:**
1. Localizar a consulta.
2. Reagendar para o novo horário.
3. Verificar a agenda do profissional.

**Resultado esperado:** O horário antigo é liberado e o novo horário fica reservado.

**Resultado obtido:** Reagendamento realizado corretamente.

**Situação:** **Aprovado**

**Evidência:** Agenda mostrando o horário antigo livre e o novo ocupado.

**Justificativa:** Valida a consistência do processo de reagendamento na interface.

---

## Cenário C — Controle de Estoque

**Pré-condições:** Módulo de estoque ativo.

**Dados:** Produto **Bloco de Anotações**, estoque mínimo **10**, entrada **15** e saída **8**.

**Passos:**
1. Cadastrar o produto.
2. Registrar entrada e saída.
3. Verificar o saldo e o alerta de estoque.

**Resultado esperado:** Saldo final de **7 unidades** e alerta de estoque baixo.

**Resultado obtido:** Saldo atualizado e alerta exibido.

**Situação:** **Aprovado**

**Evidência:** Dashboard e tabela de estoque mostrando o produto em alerta.

**Justificativa:** Testa o cálculo do estoque e a atualização da interface.

---

## Cenário D — Controle de Acesso e Permissões

**Pré-condições:** Perfis **Recepcionista** e **Psicólogo** cadastrados.

**Dados:** Usuários com permissões diferentes.

**Passos:**
1. Fazer login como recepcionista e tentar acessar prontuários.
2. Fazer login como psicólogo e acessar a mesma área.

**Resultado esperado:** Recepcionista recebe **Acesso Negado**; psicólogo acessa normalmente.

**Resultado obtido:** Permissões aplicadas corretamente.

**Situação:** **Aprovado**

**Evidência:** Tela de acesso negado para recepcionista e acesso liberado para psicólogo.

**Justificativa:** Teste de sistema que valida autenticação e controle de permissões.


