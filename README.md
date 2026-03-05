# 🏫 SRL - Sistema de Reserva de Laboratórios & Gestão de Incidentes

O **SRL** é uma solução de API REST focada na organização de espaços acadêmicos, controle de reservas e manutenção de infraestrutura técnica para faculdades.

---

## 📌 Sobre o Projeto

O sistema foi concebido para centralizar a gestão de laboratórios, permitindo que administradores controlem o inventário de salas e recursos, enquanto professores e alunos realizam reservas e reportam incidentes técnicos de forma organizada e segura.

### 🛠 Funcionalidades Principais

* **Gestão de Acesso:** Autenticação obrigatória com níveis de permissão (ADMIN e USER).
* **Controle de Salas:** Cadastro de recursos (projetor, ar-condicionado) e capacidade.
* **Reservas Inteligentes:** Validação rigorosa de conflitos de horários e restrição de período (08h às 22h).
* **Gestão de Incidentes:** Registro de falhas técnicas com fluxo de abertura e fechamento exclusivo para administradores.

---

## 🚀 Endpoints da API

### Autenticação
* `POST /auth/register` - Registro de novo usuário.
* `POST /auth/login` - Login e geração de Token Bearer.

### Salas
* `POST /rooms` - Cadastro de salas (Privilegiado: ADMIN).
* `GET /rooms` - Listagem de todos os laboratórios cadastrados.

### Reservas
* `POST /bookings` - Criação de reserva (Validação de disponibilidade).
* `GET /bookings/my` - Consulta de reservas do usuário autenticado.

### Incidentes
* `POST /incidents` - Abertura de chamado técnico vinculado a uma sala.
* `PATCH /incidents/{id}/close` - Finalização de incidente (Privilegiado: ADMIN).

---

## 🛡 Regras de Negócio e Segurança

1.  **Segurança de Senha:** Mínimo de 8 caracteres, contendo obrigatoriamente letras e números.
2.  **Integridade de Reservas:** Bloqueio automático de qualquer interseção de horário para a mesma sala.
3.  **Padronização:** Todas as datas seguem o padrão ISO (YYYY-MM-DD) e horários o formato HH:MM (24h).
4.  **Descritivo de Erros:** Respostas HTTP adequadas (400, 401, 403) com mensagens detalhadas em caso de falha.

---

## 🧪 Qualidade e Documentação de Testes

O projeto foi validado através de um **Plano de Testes** estruturado e uma **Matriz de Rastreabilidade**, garantindo que todos os requisitos funcionais e regras de negócio descritos na documentação oficial fossem atendidos e testados.

---

## 💻 Como Executar

```bash
# Clone o repositório
$ git clone [https://github.com/seu-usuario/srl-api.git](https://github.com/seu-usuario/srl-api.git)

# Acesse a pasta do projeto
$ cd srl-api

# Instale as dependências
$ npm install

# Inicie o serviço
$ npm start