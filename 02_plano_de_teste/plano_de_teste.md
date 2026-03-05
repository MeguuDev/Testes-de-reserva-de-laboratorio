Plano de Teste: Sistema de Reservas de Laboratório (SRL)
1. Escopo
O escopo abrange os testes funcionais e não funcionais da API REST do SRL, focando em:

Módulos: Autenticação, Gestão de Salas, Reservas e Incidentes.

Segurança: Validação de tokens JWT (RBAC - Controle de Acesso Baseado em Perfis).

Regras de Negócio: Validação de restrições de horários, duplicidade de e-mails e conflitos de reserva.

Interface: Exclusivamente os endpoints da API (Backend).

2. Objetivo
Garantir que o sistema cumpra todos os Requisitos Funcionais (RF) e Regras de Negócio (RN) descritos na versão 1.0, assegurando que:

Usuários sem permissão não acessem funções administrativas.

Não ocorram reservas em duplicidade (conflitos de horário).

O fluxo de incidentes respeite os estados definidos (OPEN/CLOSED).

3. Estratégia de Teste
A estratégia será baseada na pirâmide de testes, com foco em automação de API:
Tipo de Teste,Técnica,Ferramenta Sugerida
Testes de Unidade,Verificação de métodos internos e lógica de cálculo de horas.,JUnit / Jest
Testes de Integração,Testar a comunicação entre as rotas da API e o Banco de Dados.,Postman / Insomnia / RestAssured
Testes de Segurança,Tentativa de acesso a rotas ADMIN com token USER.,Postman / Newman
Testes de Caixa Preta,"Validação das respostas (200, 201, 400, 401, 403, 404).",Manual/Automatizado

4. Ambiente
Servidor de Testes: Ambiente de Staging espelhado na produção.

Banco de Dados: Instância isolada (PostgreSQL/MySQL) para evitar corrupção de dados reais.

Ferramentas de Execução: Postman (para testes manuais/coleções) e Newman (para CI/CD).

Formato de Dados: JSON (entrada e saída) seguindo o padrão ISO 8601 para datas.

5. Critérios de Entrada
Ambiente de desenvolvimento estabilizado (Build sem erros críticos).

Documentação da API (Swagger/OpenAPI) atualizada.

Banco de dados populado com massa de dados mínima (um usuário ADMIN e um USER pré-criados).

6. Critérios de Saída
100% dos casos de teste críticos (Reservas e Autenticação) aprovados.

Nenhum erro de "conflito de horário" (RN08) detectado nos testes de estresse.

Relatório de execução gerado com taxa de sucesso superior a 95%.

Todos os bugs de severidade "Alta" ou "Crítica" corrigidos e retestados.

7. Responsáveis
QA/Analista de Testes: Criação dos roteiros, execução dos testes de integração e automação.

Desenvolvedor: Correção de bugs encontrados e execução de testes unitários.

ID,Requisito,Cenário,Resultado Esperado
CT01,RN08,Tentar reservar sala no mesmo horário de uma reserva existente.,Erro 400 (Conflito detectado)
CT02,RN07,Tentar reservar às 07:00 ou 23:00.,Erro 400 (Fora do horário permitido)
CT03,RN11,Usuário USER tenta fechar incidente (PATCH /incidents/{id}/close).,Erro 403 (Forbidden)
CT04,RN02,Registrar usuário com senha de 6 caracteres.,Erro 400 (Senha inválida)
