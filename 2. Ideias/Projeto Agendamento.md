LEVANTAMENTO DE REQUISITOS

- **RF01 - Cadastro e Gestão de Usuários:** Permitir cadastro, edição, consulta e exclusão de clientes, profissionais e administradores

- **RF02 - Gestão de Serviços/Horários:** Cadastrar, editar e excluir os serviços prestados, seus respectivos valores e duração estimada. 

- **RF03 - Agendamento Online:** Permitir que o cliente escolha o serviço, o profissional, a data e a hora disponível. 

- **RF04 - Confirmação e Lembretes:** Envio automático de notificações (e-mail, SMS ou _WhatsApp_) para confirmar o agendamento ou lembrar o cliente com antecedência.

- **RF05 - Relatórios e Dashboard:** Gerar relatórios de atendimentos 

- **RF06 - Gestão de Disponibilidade:** O profissional deve poder bloquear sua agenda (férias, feriados ou indisponibilidade).

### 2. Requisitos Não Funcionais (RNF)

- RNF01 -Desempenho - O tempo de resposta do sistema para salvar ou carregar páginas não deve ultrapassar 3 segundos.

- **RNF02 - Disponibilidade:** O sistema deve estar disponível 99% do tempo no ar (alta disponibilidade).

- **RNF03 - Segurança:** Os dados do usuário devem ser armazenados de forma criptografada.

- **RNF04 - Responsividade:** O sistema deve se adaptar perfeitamente a telas de computadores, tablets e smartphones. 

### 3. Principais Regras de Negócio

- **Conflito de horários:** O sistema não pode permitir que dois clientes marquem o mesmo profissional no mesmo horário.

- **Política de cancelamento:** Definir um tempo mínimo (ex: 24 horas) para que o cliente consiga cancelar um horário sem multa.

- **Permissões:** O que o cliente vê (apenas o seu agendamento) é diferente do que o administrador vê (gestão total).
