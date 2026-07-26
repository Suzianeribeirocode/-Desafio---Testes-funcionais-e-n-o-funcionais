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


# Exercício 5 — Testes de Aceitação

## Critérios de Aceitação

### 1. Impedir duplo agendamento no mesmo horário

**Dado que** o Dr. Rafael Lima já possui uma consulta agendada para **28/07/2026 às 10:00**,

**Quando** a recepcionista tentar agendar outro paciente no mesmo horário,

**Então** o sistema deve bloquear o agendamento, informar que o horário está ocupado e manter a agenda sem alterações.

---

### 2. Controle de acesso a prontuários por perfil

**Dado que** uma usuária esteja autenticada com o perfil **Recepcionista**,

**Quando** tentar acessar a tela de prontuários,

**Então** o sistema deve negar o acesso, exibir uma mensagem de permissão insuficiente e ocultar os dados do paciente.

---

### 3. Atualização do saldo financeiro

**Dado que** o relatório financeiro apresenta um saldo de **R$ 1.700,00**,

**Quando** o gestor registrar uma nova receita de **R$ 300,00**,

**Então** o sistema deve atualizar automaticamente o saldo para **R$ 2.000,00**.

---

### 4. Alerta de estoque mínimo

**Dado que** o produto **Bloco de Anotações** possui estoque mínimo de **10 unidades** e estoque atual de **11 unidades**,

**Quando** forem registradas **2 unidades** de saída,

**Então** o sistema deve atualizar o saldo para **9 unidades** e exibir um alerta de estoque baixo.

---

### 5. Busca de pacientes

**Dado que** o paciente **Carlos Eduardo Silva** está cadastrado com o CPF **123.456.789-00**,

**Quando** o usuário pesquisar por **"Carlos"** ou **"123.456"**,

**Então** o sistema deve localizar e exibir imediatamente o cadastro do paciente.

---

### 6. Persistência de dados

**Dado que** a recepcionista cadastrou um novo paciente e encerrou a sessão,

**Quando** realizar um novo login,

**Então** o cadastro do paciente deve permanecer salvo e disponível no sistema.

## Justificativa dos Testes de Aceitação

Os **testes de aceitação** verificam se o sistema atende aos requisitos de negócio e às necessidades do usuário final. Eles utilizam uma linguagem simples (Dado, Quando, Então) para confirmar que as funcionalidades estão prontas para uso e homologação.

# Exercício 6 — Classificação dos Testes

| Cenário | Classificação | Justificativa |
|----------|---------------|---------------|
| Verificar se **receitas − despesas** retorna o saldo correto. | **Unitário** | Avalia uma função ou regra de cálculo matemática de forma isolada. |
| Verificar se uma **receita salva** aparece no relatório financeiro. | **Integração** | Valida a comunicação entre o módulo financeiro e o relatório. |
| Executar todo o fluxo entre **cadastro, atendimento e pagamento**. | **Sistema** | Testa a jornada completa do usuário de ponta a ponta pela interface. |
| Confirmar com a direção da clínica se o relatório atende às necessidades administrativas. | **Aceitação** | Verifica se a solução atende aos requisitos e regras do negócio. |
| Verificar isoladamente a **validação de CPF**. | **Unitário** | Avalia apenas o algoritmo de validação, sem dependências externas. |
| Verificar se um **reagendamento** atualiza corretamente a agenda. | **Integração** | Valida a troca de informações entre o módulo de reagendamento e a agenda do profissional. |
| Avaliar se apenas **psicólogos** podem visualizar prontuários. | **Aceitação** | Garante o cumprimento das regras de acesso e privacidade dos dados. |
| Confirmar com a recepcionista se o processo de agendamento atende à rotina da clínica. | **Aceitação** | Verifica se a funcionalidade atende às necessidades do usuário final. |

---

# Parte 2 — Checklist de Testes Não Funcionais

| Categoria | O que verificar | Como verificar | Critério esperado | Risco associado | Prioridade |
|------------|-----------------|----------------|-------------------|-----------------|------------|
| **Performance** | Tempo de carregamento do Dashboard | Medir o tempo de renderização com o DevTools em rede normal. | Carregar em até **2 segundos**. | Lentidão e insatisfação do usuário. | **Alta** |
| **Performance** | Abertura de tabelas com grande volume | Inserir mais de **1.000 registros** e abrir a lista de pacientes. | Exibir a listagem em até **1,5 segundo** sem travamentos. | Perda de desempenho e produtividade. | **Alta** |
| **Performance** | Velocidade da pesquisa de pacientes | Pesquisar com mais de **1.000 registros** cadastrados. | Filtrar resultados em até **500 ms**. | Dificuldade para localizar pacientes. | **Média** |
| **Performance** | Consumo de memória | Utilizar o sistema continuamente por **30 minutos**. | Manter consumo estável, sem vazamento de memória. | Lentidão progressiva do sistema. | **Média** |
| **Performance** | Tamanho dos arquivos JS/CSS | Inspecionar a aba **Network** no primeiro carregamento. | Pacote inferior a **2 MB**. | Carregamento lento e maior consumo de dados. | **Baixa** |
| **Segurança** | Acesso a prontuários sem autenticação | Tentar acessar a URL diretamente em janela anônima. | Redirecionar para a tela de login. | Exposição de dados confidenciais. | **Alta** |
| **Segurança** | Restrição por perfil | Tentar excluir um paciente com perfil de recepcionista. | Bloquear a ação e exibir mensagem de permissão negada. | Exclusão indevida de informações. | **Alta** |
| **Segurança** | Proteção contra XSS | Inserir `<script>alert(1)</script>` em um formulário. | Sanitizar o conteúdo informado. | Execução de código malicioso. | **Alta** |
| **Segurança** | Dados sensíveis no navegador | Verificar o **localStorage** na aba **Application**. | Não armazenar senhas ou tokens sem proteção. | Vazamento de informações. | **Alta** |
| **Segurança** | Expiração de sessão | Permanecer inativo por **30 minutos**. | Encerrar a sessão e solicitar novo login. | Acesso indevido por terceiros. | **Média** |
| **Usabilidade** | Clareza dos menus | Navegar pelas telas principais. | Menus intuitivos e fáceis de localizar. | Dificuldade de uso e retrabalho. | **Média** |
| **Usabilidade** | Confirmação antes da exclusão | Excluir um paciente ou agendamento. | Exibir confirmação antes da exclusão. | Exclusão acidental de dados. | **Alta** |
| **Usabilidade** | Mensagens de sucesso e erro | Enviar formulários válidos e inválidos. | Exibir mensagens claras ao usuário. | Incerteza sobre o resultado da ação. | **Média** |
| **Usabilidade** | Campos obrigatórios | Verificar formulários de cadastro. | Destacar todos os campos obrigatórios. | Erros frequentes de preenchimento. | **Média** |
| **Usabilidade** | Contraste e legibilidade | Avaliar conforme a norma **WCAG**. | Contraste mínimo de **4,5:1**. | Dificuldade de leitura para usuários. | **Baixa** |
| **Compatibilidade** | Navegadores | Testar no **Chrome**, **Firefox** e **Edge**. | Funcionamento idêntico em todos. | Incompatibilidade entre navegadores. | **Alta** |
| **Compatibilidade** | Responsividade | Testar em **360 px**, **768 px** e **1366 px**. | Layout adaptado sem quebras. | Componentes inacessíveis em telas menores. | **Alta** |
| **Compatibilidade** | Orientação da tela | Alternar entre modo retrato e paisagem. | Interface ajustada automaticamente. | Problemas de exibição em dispositivos móveis. | **Média** |
| **Compatibilidade** | Acentos e caracteres especiais | Salvar nomes com acentuação. | Exibir corretamente todos os caracteres. | Erros de codificação e relatórios inconsistentes. | **Média** |
| **Compatibilidade** | Sistemas Operacionais | Executar no **Windows**, **macOS** e **Linux**. | Funcionamento equivalente em todos os sistemas. | Diferenças de comportamento entre plataformas. | **Baixa** |