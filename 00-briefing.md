# 📋 Briefing

## 1. Informações gerais    
1.1 - Nome do sistema: NutriCount  
1.2 - Nome da equipe:  Pedro Henrique Dos Santos Soares e 

## 2. Problema e/ou necessidade    
2.1 - Pessoas que querem controlar a alimentação (emagrecer, ganhar massa ou manter o peso) têm dificuldade de saber quantas calorias e nutrientes consomem por dia e de manter o hábito de beber água. O sistema deve permitir registrar refeições (com foto opcional) e consumo de água de forma rápida, calcular automaticamente calorias e macronutrientes, comparar o consumo com metas pessoais, lembrar o usuário de registrar as refeições nos horários habituais e permitir que o usuário se vincule ao profissional que o acompanha (nutricionista, nutrólogo ou médico), que passa a ver os resultados do cliente diretamente no sistema. 
2.2 - Registro manual e demorado em anotações ou planilhas; dificuldade de estimar calorias das porções; falta de histórico e de acompanhamento da evolução; não saber se está dentro da meta diária de calorias e de água; tabelas nutricionais espalhadas e pouco confiáveis; esquecer de registrar as refeições e de beber água ao longo do dia; para o profissional, dificuldade de acompanhar a alimentação real do cliente entre as consultas, dependendo de relatos e anotações soltas. 
2.3 - O controle é feito de forma manual (papel, bloco de notas ou planilhas) ou por estimativa "de cabeça". Isso gera erros de cálculo, esquecimento dos registros, abandono do controle após poucos dias e falta de dados confiáveis para o usuário e para o profissional que o acompanha avaliarem os resultados.

## 3. Escopo funcional do sistema   
- Permitir que o usuário registre suas refeições e o consumo de água diários e acompanhe sua evolução em relação às metas de calorias e de hidratação, com a possibilidade de vincular-se a um profissional de saúde que acompanha os resultados dentro do sistema. 
- Cadastro e autenticação de usuário; perfil com dados físicos (peso, altura, idade, objetivo) e cálculo da necessidade calórica diária; catálogo de alimentos com informações nutricionais por porção, alimentado por base externa e por cadastro manual, usado para calcular automaticamente as calorias de cada refeição; diário alimentar com registro, alteração e exclusão de refeições, com foto do alimento opcional; relatórios de resumo diário, histórico e comparação com a meta; controle de hidratação, com meta diária de água, registro do consumo e demonstrativo de evolução (dias em que a meta foi batida e dias em que não foi); horários padrão das refeições com lembretes automáticos quando a refeição não for registrada; cadastro de profissional de saúde, com validação do registro profissional pelo administrador; vínculo entre usuário e profissional, com autorização do usuário para o compartilhamento dos dados; perfil do profissional com painel dos clientes vinculados e seus resultados (diário alimentar, hidratação e evolução); administração de usuários e do catálogo de alimentos.  

## 4. Escopo não funcional    
Desempenho: busca de alimento em menos de 2 segundos.
- Usabilidade: registrar uma refeição ou um copo de água em menos de 1 minuto, com interface simples e responsiva (web e celular). A foto do alimento é sempre opcional.
- Segurança e privacidade: autenticação com senha protegida e tratamento dos dados pessoais conforme a LGPD. Dados de alimentação e saúde são compartilhados com o profissional somente mediante autorização explícita do usuário, que pode encerrar o vínculo a qualquer momento. O profissional acessa apenas os dados dos clientes vinculados a ele.
- Armazenamento: fotos com limite de tamanho e formato definidos, armazenadas de forma segura.
- Disponibilidade: sistema acessível pelo navegador, com meta de 99% de disponibilidade mensal.
- Notificações: envio de lembretes (push no navegador/celular ou e-mail) com permissão do usuário, respeitando os horários configurados.
- Restrições: prazo definido pelo professor e uso de base nutricional pública, sem custo.

## 5. Usuários    
- Usuário: pessoa que registra refeições e consumo de água, define metas, configura horários de refeição, consulta relatórios e evolução e se vincula a um profissional.
- Profissional de saúde (nutricionista, nutrólogo ou médico): possui perfil próprio, aceita vínculos de clientes e acompanha os resultados dos clientes vinculados.
- Administrador: mantém o catálogo de alimentos, gerencia contas de usuários e valida o registro dos profissionais.

## 6. Entregáveis   
        • Documento de Visão do Sistema  
        • Diagrama de Caso de Uso  
        • Especificação de Caso de Uso  
        • Diagrama de Classe   
        • Documento de Arquitetura do Sistema   
        • Protótipo do Sistema   
