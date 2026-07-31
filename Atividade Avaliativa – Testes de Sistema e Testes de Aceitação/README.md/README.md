# Atividade Avaliativa – Testes de Sistema e Testes de Aceitação

## Etapa 1 – Compreensão do Cenário

### Cenário
Um sistema bancário permite que usuários realizem login, acessem sua conta e visualizem seu saldo atual.

### Funcionalidades envolvidas
- Login do usuário.
- Validação de usuário e senha.
- Acesso à conta bancária.
- Visualização do saldo atual.
- Encerramento da sessão (logout).

### Fluxo principal (Caminho Feliz)
1. O usuário acessa a tela de login.
2. Informa usuário e senha válidos.
3. O sistema autentica o usuário.
4. O sistema direciona para a página inicial da conta.
5. O saldo atual é exibido.

### Variações de fluxo
- Usuário informa senha incorreta.
- Usuário informa login inexistente.
- Usuário deixa os campos obrigatórios em branco.
- O sistema apresenta falha ao carregar o saldo.

---

# Etapa 2 – Testes de Sistema

## TS01 – Login com credenciais válidas

| Campo | Descrição |
|--------|-----------|
| **ID** | TS01 |
| **Título** | Login com credenciais válidas |
| **Pré-condições** | Usuário cadastrado e sistema disponível. |
| **Passos** | 1. Acessar a tela de login.<br>2. Informar usuário válido.<br>3. Informar senha válida.<br>4. Clicar em **Entrar**. |
| **Resultado Esperado** | O sistema autentica o usuário e redireciona para a tela principal da conta. |

---

## TS02 – Visualização do saldo após login

| Campo | Descrição |
|--------|-----------|
| **ID** | TS02 |
| **Título** | Visualização do saldo após login |
| **Pré-condições** | Usuário autenticado. |
| **Passos** | 1. Realizar login.<br>2. Acessar a tela **Minha Conta**. |
| **Resultado Esperado** | O sistema exibe corretamente o saldo disponível da conta. |

---

## TS03 – Login com senha incorreta

| Campo | Descrição |
|--------|-----------|
| **ID** | TS03 |
| **Título** | Login com senha incorreta |
| **Pré-condições** | Usuário cadastrado. |
| **Passos** | 1. Informar usuário válido.<br>2. Informar senha incorreta.<br>3. Clicar em **Entrar**. |
| **Resultado Esperado** | O sistema impede o acesso e apresenta mensagem de credenciais inválidas. |

---

## TS04 – Campos obrigatórios em branco

| Campo | Descrição |
|--------|-----------|
| **ID** | TS04 |
| **Título** | Campos obrigatórios em branco |
| **Pré-condições** | Sistema disponível. |
| **Passos** | 1. Acessar a tela de login.<br>2. Deixar usuário e senha em branco.<br>3. Clicar em **Entrar**. |
| **Resultado Esperado** | O sistema informa que os campos são obrigatórios e permanece na tela de login. |

---

# Etapa 3 – Testes de Aceitação

## TA01 – Cliente acessa sua conta

| Campo | Descrição |
|--------|-----------|
| **ID** | TA01 |
| **Título** | Cliente acessa sua conta para consultar saldo |
| **Pré-condições** | Cliente possui cadastro ativo. |
| **Passos** | 1. Informar usuário e senha válidos.<br>2. Clicar em **Entrar**. |
| **Resultado Esperado** | O cliente acessa sua conta com sucesso e consegue visualizar seu saldo, atendendo à necessidade do negócio. |

---

## TA02 – Cliente consulta saldo

| Campo | Descrição |
|--------|-----------|
| **ID** | TA02 |
| **Título** | Consulta de saldo após login |
| **Pré-condições** | Cliente autenticado. |
| **Passos** | 1. Entrar no sistema.<br>2. Acessar a tela inicial da conta. |
| **Resultado Esperado** | O saldo é apresentado de forma clara e correta, permitindo que o cliente acompanhe sua situação financeira. |

---

## TA03 – Cliente informa senha incorreta

| Campo | Descrição |
|--------|-----------|
| **ID** | TA03 |
| **Título** | Tentativa de login com senha incorreta |
| **Pré-condições** | Cliente cadastrado. |
| **Passos** | 1. Informar usuário válido.<br>2. Informar senha incorreta.<br>3. Clicar em **Entrar**. |
| **Resultado Esperado** | O cliente recebe uma mensagem clara informando que não foi possível realizar o login e pode tentar novamente. |

---

## TA04 – Campos obrigatórios não preenchidos

| Campo | Descrição |
|--------|-----------|
| **ID** | TA04 |
| **Título** | Validação de campos obrigatórios |
| **Pré-condições** | Sistema disponível. |
| **Passos** | 1. Deixar usuário e senha em branco.<br>2. Clicar em **Entrar**. |
| **Resultado Esperado** | O sistema orienta o cliente a preencher os campos obrigatórios antes de continuar, proporcionando uma experiência de uso adequada. |

---

# Etapa 4 – Justificativa e Classificação

## TS01 – Login com credenciais válidas

### Por que é um teste de sistema?
- **Objetivo do teste:** Validar o funcionamento técnico da autenticação.
- **Ponto de vista:** Sistema.
- **Tipo de validação:** Integração entre a tela de login e a tela principal.

---

## TS02 – Visualização do saldo

### Por que é um teste de sistema?
- **Objetivo do teste:** Verificar se o sistema recupera e apresenta corretamente o saldo.
- **Ponto de vista:** Sistema.
- **Tipo de validação:** Integração entre autenticação e consulta de saldo.

---

## TS03 – Login com senha incorreta

### Por que é um teste de sistema?
- **Objetivo do teste:** Validar o tratamento técnico para credenciais inválidas.
- **Ponto de vista:** Sistema.
- **Tipo de validação:** Validação do fluxo alternativo de autenticação.

---

## TS04 – Campos obrigatórios em branco

### Por que é um teste de sistema?
- **Objetivo do teste:** Verificar a validação dos campos obrigatórios.
- **Ponto de vista:** Sistema.
- **Tipo de validação:** Validação da interface e do fluxo de login.

---

## TA01 – Cliente acessa sua conta

### Por que é um teste de aceitação?
- **Objetivo do teste:** Confirmar que o cliente consegue acessar sua conta.
- **Ponto de vista:** Usuário e negócio.
- **Tipo de validação:** Atendimento ao critério de aceitação da funcionalidade.

---

## TA02 – Cliente consulta saldo

### Por que é um teste de aceitação?
- **Objetivo do teste:** Garantir que o cliente visualize seu saldo corretamente.
- **Ponto de vista:** Usuário.
- **Tipo de validação:** Entrega do valor esperado pela funcionalidade.

---

## TA03 – Cliente informa senha incorreta

### Por que é um teste de aceitação?
- **Objetivo do teste:** Verificar se o usuário recebe uma mensagem clara ao errar a senha.
- **Ponto de vista:** Usuário.
- **Tipo de validação:** Critério de aceitação relacionado à usabilidade.

---

## TA04 – Campos obrigatórios não preenchidos

### Por que é um teste de aceitação?
- **Objetivo do teste:** Garantir que o sistema oriente corretamente o usuário.
- **Ponto de vista:** Usuário.
- **Tipo de validação:** Critério de aceitação relacionado à experiência do usuário.

---

# Etapa 5 – Revisão por Pares

## Critérios de Revisão

### Clareza
Os casos de teste apresentam linguagem objetiva, passos bem definidos e resultados esperados de fácil compreensão.

### Estrutura
Todos os casos seguem a estrutura exigida:
- ID
- Título
- Pré-condições
- Passos
- Resultado Esperado

### Coerência com o tipo de teste

#### Testes de Sistema
- Validam o funcionamento técnico da aplicação.
- Verificam a integração entre telas.
- Baseados em requisitos funcionais.

#### Testes de Aceitação
- Validam as expectativas do usuário e do negócio.
- Confirmam que a funcionalidade entrega valor.
- Baseados em critérios de aceitação.

---

## Conclusão

Os **Testes de Sistema** garantem que o sistema funcione corretamente do ponto de vista técnico, enquanto os **Testes de Aceitação** confirmam que as funcionalidades atendem às expectativas do usuário e do negócio.

Dessa forma, antes da entrega do sistema, ambos os tipos de teste são essenciais para assegurar qualidade técnica e satisfação do cliente.