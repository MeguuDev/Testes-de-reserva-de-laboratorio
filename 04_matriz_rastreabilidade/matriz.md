# Matriz de Rastreabilidade

| Requisito | Casos de Teste |
|-----------|----------------|
ID Requisito,Descrição do Requisito / Regra,Endpoint Relacionado,ID Caso de Teste,Status Esperado
RF01/RN01,Registro de usuário (E-mail único),POST /auth/register,CT-001,201 Created / 400 Bad Request
RN02,"Validação de senha (8 chars, let/num)",POST /auth/register,CT-002,400 Bad Request (se inválida)
RF02/RF03,Login e geração de Token JWT,POST /auth/login,CT-003,200 OK + Token
RN03/6.0,Bloqueio de acesso sem Token,Todos exceto /auth,CT-004,401 Unauthorized
RF04/RN05,Cadastro de sala (ADMIN),POST /rooms,CT-005,201 Created
RN04,Nome de sala duplicado,POST /rooms,CT-006,400 Bad Request
RF05,Listagem de salas (ADMIN/USER),GET /rooms,CT-007,200 OK
RF06/RN09,Criação de reserva (Sala existente),POST /bookings,CT-008,201 Created
RN06,Início < Término,POST /bookings,CT-009,400 Bad Request
RN07,Horário permitido (08:00 - 22:00),POST /bookings,CT-010,400 Bad Request
RN08,Conflito de horários (Interseção),POST /bookings,CT-011,400 Bad Request
RF07,Visualizar próprias reservas,GET /bookings/my,CT-012,200 OK
RF09/RF11,Abertura de incidente (Status OPEN),POST /incidents,CT-013,201 Created
RN10,Descrição incidente min. 10 chars,POST /incidents,CT-014,400 Bad Request
RF10/RN12,Fechamento de incidente (Status CLOSED),PATCH /incidents/{id}/close,CT-015,200 OK
RN11,Apenas ADMIN fecha incidente,PATCH /incidents/{id}/close,CT-016,403 Forbidden (se USER)