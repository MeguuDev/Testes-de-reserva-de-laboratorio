# Casos de Teste

## CT01 – Registro de Usuário com E-mail Duplicado
Requisito: RN01

Descrição: Validar que o sistema impede o cadastro de um e-mail que já existe na base de dados.

Pré-condição: Existência de um usuário cadastrado com o e-mail admin@fatec.br.

Entrada: POST /auth/register com JSON {"nome": "João", "email": "admin@fatec.br", "senha": "Senha123", "perfil": "USER"}.

Resultado Esperado: O sistema deve retornar erro informando que o e-mail já está em uso (HTTP 400).

## CT02 – Validação de Senha Curta
Requisito: RN02

Descrição: Verificar se o sistema rejeita senhas com menos de 8 caracteres.

Pré-condição: Nenhuma.

Entrada: POST /auth/register com o campo senha preenchido como Ab123.

Resultado Esperado: O sistema deve retornar erro informando que a senha deve possuir no mínimo 8 caracteres (HTTP 400).

## CT03 – Validação de Senha sem Números
Requisito: RN02

Descrição: Verificar se o sistema rejeita senhas que contenham apenas letras.

Pré-condição: Nenhuma.

Entrada: POST /auth/register com o campo senha preenchido como SenhaApenasLetras.

Resultado Esperado: O sistema deve retornar erro informando a necessidade de ao menos 1 número (HTTP 400).

## CT04 – Acesso Negado a Módulos Restritos
Requisito: RN03 e Item 6

Descrição: Tentar listar salas sem enviar o Token de autenticação.

Pré-condição: Nenhuma.

Entrada: Chamada GET /rooms sem Header Authorization.

Resultado Esperado: O sistema deve retornar erro de não autorizado (HTTP 401).

Módulo de Salas
## CT05 – Cadastro de Sala com Nome Duplicado
Requisito: RN04

Descrição: Impedir que o ADMIN cadastre duas salas com o mesmo nome.

Pré-condição: Já existir uma sala cadastrada com o nome "Laboratório 01".

Entrada: POST /rooms com o campo nome como "Laboratório 01".

Resultado Esperado: O sistema deve retornar erro de nome duplicado (HTTP 400).

## CT06 – Cadastro de Sala com Capacidade Inválida
Requisito: RN05

Descrição: Garantir que a capacidade da sala seja sempre um número positivo.

Pré-condição: Usuário logado como ADMIN.

Entrada: POST /rooms com o campo capacidade preenchido como 0.

Resultado Esperado: O sistema deve retornar erro informando que a capacidade deve ser maior que 0 (HTTP 400).

## CT07 – Restrição de Criação de Sala para Aluno/Professor
Requisito: Item 2 (Permissões)

Descrição: Verificar se um usuário com perfil USER consegue criar salas.

Pré-condição: Usuário logado com perfil USER.

Entrada: POST /rooms com dados válidos de uma nova sala.

Resultado Esperado: O sistema deve retornar acesso negado (HTTP 403).

Módulo de Reservas
## CT08 – Inconsistência Crítica de Horário
Requisito: RN06

Descrição: Impedir reservas onde o horário de início é posterior ao horário de término.

Pré-condição: Usuário autenticado.

Entrada: POST /bookings com hora_inicio: "15:00" e hora_termino: "14:00".

Resultado Esperado: O sistema deve impedir a reserva por erro de lógica de horário (HTTP 400).

## CT09 – Reserva Fora do Limite Noturno
Requisito: RN07

Descrição: Tentar realizar uma reserva que termina após as 22:00.

Pré-condição: Usuário autenticado.

Entrada: POST /bookings com hora_inicio: "21:30" e hora_termino: "22:30".

Resultado Esperado: O sistema deve retornar erro de horário fora do limite permitido (HTTP 400).

## CT10 – Conflito de Horário: Interseção Parcial
Requisito: RN08

Descrição: Tentar reservar uma sala que já possui um compromisso no meio do intervalo solicitado.

Pré-condição: Sala 1 já reservada das 10:00 às 12:00.

Entrada: POST /bookings para a Sala 1 das 11:30 às 13:30.

Resultado Esperado: O sistema deve retornar erro de conflito de horário (HTTP 400).

## CT11 – Conflito de Horário: Reserva Interna
Requisito: RN08

Descrição: Tentar reservar um horário que está totalmente contido dentro de uma reserva já existente.

Pré-condição: Sala 1 já reservada das 08:00 às 12:00.

Entrada: POST /bookings para a Sala 1 das 09:00 às 10:00.

Resultado Esperado: O sistema deve retornar erro de conflito de horário (HTTP 400).

## CT12 – Reserva em Sala Inexistente
Requisito: RN09

Descrição: Validar o comportamento do sistema ao tentar reservar um ID de sala que não consta no banco.

Pré-condição: Usuário autenticado.

Entrada: POST /bookings com id_sala: 9999.

Resultado Esperado: O sistema deve retornar erro de sala não encontrada (HTTP 404).

Módulo de Incidentes
## CT13 – Validação de Descrição de Incidente
Requisito: RN10

Descrição: Tentar abrir um incidente com descrição extremamente curta.

Pré-condição: Usuário autenticado.

Entrada: POST /incidents com descricao: "Quebrou".

Resultado Esperado: O sistema deve retornar erro de descrição insuficiente (mínimo 10 caracteres) (HTTP 400).

## CT14 – Restrição de Fechamento de Incidente
Requisito: RN11

Descrição: Verificar se um usuário comum (USER) consegue fechar um incidente.

Pré-condição: Usuário logado como USER e incidente ID 5 aberto.

Entrada: PATCH /incidents/5/close.

Resultado Esperado: O sistema deve retornar acesso negado (HTTP 403).

## CT15 – Fechamento de Incidente Já Encerrado
Requisito: RN12

Descrição: Tentar finalizar um incidente que já possui o status CLOSED.

Pré-condição: Usuário logado como ADMIN e incidente ID 10 com status CLOSED.

Entrada: PATCH /incidents/10/close.

Resultado Esperado: O sistema deve retornar erro informando que o incidente já está finalizado (HTTP 400).
