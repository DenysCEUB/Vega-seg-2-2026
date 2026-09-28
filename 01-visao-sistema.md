# 📖 Documento de Visão do Sistema

## 1. Introdução
O NutriCount é um sistema web e mobile para controle de calorias, nutrientes e hidratação. O usuário registra suas refeições e o consumo de água ao longo do dia, acompanha o desempenho em relação às metas pessoais e recebe lembretes nos horários habituais das refeições. O sistema também permite que o usuário se vincule a um profissional de saúde (nutricionista, nutrólogo ou médico), que acompanha os resultados dos seus clientes dentro da própria plataforma. A motivação é facilitar o controle alimentar, hoje feito de forma manual, imprecisa e sem histórico, e aproximar o acompanhamento profissional da rotina real do usuário.

## 2. Objetivos
2.1- Problemas que o sistema pretende resolver:
- Dificuldade de saber quantas calorias e nutrientes são consumidos por dia.
- Registro manual, demorado e sem histórico (papel, notas ou planilhas).
- Dificuldade de manter o hábito de beber água e de lembrar de registrar as refeições
- Falta de dados confiáveis para o usuário e para o profissional que o acompanha entre as consultas.
  
2.2- Benefícios esperados: 
- Registro rápido de refeições, com cálculo automático de calorias e macronutrientes.
- Acompanhamento de metas de calorias e de água, com demonstrativo de evolução.
- Lembretes automáticos que ajudam a manter a constância dos registros.
- Acompanhamento remoto e baseado em dados reais pelo profissional de saúde.  

## 3. Stakeholders
- **Usuário:** pessoa que controla a própria alimentação e hidratação.
- **Profissional de saúde (nutricionista, nutrólogo ou médico):** acompanha os resultados dos clientes vinculados.
- **Administrador do sistema:** mantém o catálogo de alimentos, gerencia contas e valida o registro dos profissionais.
- **Provedor da base nutricional:** organização responsável pela base/API de alimentos usada para calcular as calorias; impacta a qualidade dos dados do sistema.
- **Provedor do serviço de notificação:** organização responsável pelo envio dos lembretes (push ou e-mail); impacta a entrega das notificações ao usuário.
- **Equipe de desenvolvimento e professor:** responsáveis pela construção e avaliação do projeto.  

## 4. Escopo
O NutriCount permite que o usuário cadastre seu perfil, registre o que come e bebe, acompanhe sua evolução e compartilhe os resultados com o profissional que o acompanha. Suas principais funcionalidades são:

- **Cadastro de Usuário [UC1]** - Registro, autenticação e manutenção da conta do usuário no sistema.
- **Perfil e Meta Calórica [UC2]** - Manutenção dos dados físicos (peso, altura, idade e objetivo) e cálculo da necessidade calórica diária, base para a meta calórica do usuário.
- **Catálogo de Alimentos [UC3]** - Manutenção e consulta de alimentos com informações nutricionais por porção, alimentado por base externa e por cadastro manual, usado para calcular automaticamente as calorias de cada refeição.
- **Diário Alimentar [UC4]** - Registro, alteração e exclusão das refeições do dia (café da manhã, almoço, jantar e lanches), com porções, cálculo automático de calorias e foto do alimento opcional.
- **Relatórios de Consumo [UC5]** - Resumo diário, histórico de consumo e comparação com a meta calórica.
- **Controle de Hidratação [UC6]** - Definição da meta diária de água, registro do consumo ao longo do dia e demonstrativo de evolução, mostrando os dias em que a meta foi batida e os dias em que não foi.
- **Lembretes de Refeição [UC7]** - Cadastro dos horários padrão das refeições e envio de lembretes automáticos quando a refeição não for registrada no horário.
- **Cadastro de Profissional [UC8]** - Registro do profissional de saúde, com validação do registro profissional pelo administrador.
- **Vínculo com Profissional [UC9]** - O usuário busca um profissional, solicita o vínculo e autoriza o compartilhamento dos seus dados, podendo encerrar o vínculo a qualquer momento.
- **Acompanhamento de Clientes [UC10]** - O profissional consulta a lista de clientes vinculados e seus resultados (diário alimentar, hidratação e evolução) no seu próprio perfil.

## 5. Restrições
Apresente limitações técnicas, prazos, orçamento ou requisitos obrigatórios.  
- Tempo reduzido de desenvolvimento: os integrantes ingressou na disciplina no meio do semestre, o que diminuiu o tempo disponível para o estudo do conteúdo e a elaboração dos artefatos.
- Equipe formada por uma dupla (dois integrantes), o que limita a capacidade de produção e o volume de funcionalidades entregues em cada etapa.
- Sistema acessível via navegador, com interface responsiva para uso no celular.
- Tratamento dos dados pessoais e de saúde conforme a LGPD: o compartilhamento com o profissional só ocorre mediante autorização explícita do usuário, que pode encerrar o vínculo quando quiser. O profissional acessa apenas os dados dos clientes vinculados a ele.
- Uso de base nutricional pública, sem custo.
- Envio de lembretes dependente de permissão do usuário para notificações.

## 6. Critérios de Sucesso
- Registrar uma refeição ou um copo de água em menos de 1 minuto.
- Busca de alimento com resposta em menos de 2 segundos.
- 100% das refeições registradas com calorias calculadas automaticamente.
- Pelo menos 70% dos usuários de teste cumprem a meta calórica semanal.
- Pelo menos 60% dos usuários de teste batem a meta de água em 5 dos 7 dias.
- Primeiro lembrete enviado 10 minutos após o horário configurado sem registro da refeição, com reenvio a cada 30 minutos (até 3 lembretes) até que a refeição seja registrada.
- O profissional consegue consultar os resultados de um cliente vinculado em até 3 cliques.
