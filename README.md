# 📘 Projeto de Sistema Computacional - "NutriCount"
<img width="192" height="89" alt="image" src="https://github.com/user-attachments/assets/9f00c432-def4-4de7-8b12-439b34ca8b9c" />

Curso: Ciência de Dados e Machine Learning

Disciplina: Análise e Projeto de Sistemas

Orientador: Prof. Me. Denys Alves da Silva

Nome do grupo: [NOME DO GRUPO]

Alunos: Pedro Henrique Dos Santos Soares ([E-MAIL]@sempreceub.com), Mateus Itibal ([E-MAIL]@sempreceub.com)

---

## 🥗 Sobre o sistema

O **NutriCount** é um sistema web responsivo (computador e celular) para controle de calorias, nutrientes e hidratação. O usuário registra as refeições, com foto opcional, e o consumo de água. O sistema calcula automaticamente calorias e macronutrientes, compara o consumo com as metas pessoais e envia lembretes nos horários das refeições. O usuário também pode se vincular a um profissional de saúde (nutricionista, nutrólogo ou médico), que acompanha os resultados dele dentro da plataforma.

### Problema que resolve

Hoje o controle alimentar costuma ser feito em papel, bloco de notas ou planilha, ou "de cabeça". Isso gera erros de cálculo, esquecimento dos registros, abandono do controle após poucos dias e falta de dados confiáveis. O profissional que acompanha a pessoa também não vê a alimentação real entre as consultas e depende de relatos soltos.

### Atores

| Ator | Papel |
|---|---|
| **Usuário** | Registra refeições e água, define metas, configura lembretes, consulta relatórios e se vincula a um profissional |
| **Profissional de saúde** | Tem perfil próprio, aceita vínculos e acompanha os resultados dos clientes vinculados |
| **Administrador** | Mantém o catálogo de alimentos, gerencia contas e valida o registro dos profissionais |
| **Base Nutricional Externa** (sistema) | Fornece os dados nutricionais dos alimentos (tabela TACO) |
| **Serviço de Notificação** (sistema) | Entrega os lembretes por push ou e-mail |

### Funcionalidades (casos de uso)

| Caso de uso | Funcionalidade | Casos de uso específicos |
|---|---|---|
| **UC1** | Cadastro de Usuário | Incluir, alterar, autenticar e desativar usuário |
| **UC2** | Perfil e Meta Calórica | Alterar e consultar perfil, calcular necessidade calórica, definir meta |
| **UC3** | Catálogo de Alimentos | Incluir, alterar, consultar, desativar e importar alimento da base externa |
| **UC4** | Diário Alimentar | Registrar, alterar, excluir e consultar refeições; anexar foto |
| **UC5** | Relatórios de Consumo | Resumo diário, histórico e comparação com a meta |
| **UC6** | Controle de Hidratação | Meta de água, registro e correção do consumo, evolução |
| **UC7** | Lembretes de Refeição | Definir e alterar horários, envio automático do lembrete |
| **UC8** | Cadastro de Profissional | Incluir, alterar, validar registro e desativar profissional |
| **UC9** | Vínculo com Profissional | Buscar profissional, solicitar, autorizar compartilhamento, aceitar e encerrar vínculo |
| **UC10** | Acompanhamento de Clientes | Listar clientes, consultar diário e evolução do cliente |

O detalhamento de cada caso de uso (fluxos e regras de negócio RN01 a RN53) está na [Especificação de Caso de Uso](03-especificacao-caso-uso.md).

### Critérios de sucesso

- Registrar uma refeição ou um copo de água em menos de 1 minuto.
- Busca de alimento com resposta em menos de 2 segundos.
- 100% das refeições com calorias calculadas automaticamente.
- O profissional consulta os resultados de um cliente vinculado em até 3 cliques.
- Dados de saúde compartilhados somente com autorização do usuário, conforme a LGPD.

---

## 🛠️ Tecnologias previstas

| Camada | Tecnologias |
|---|---|
| Front End | React 18, TypeScript, Vite, Tailwind CSS, PWA (notificações push) |
| Back End | Node.js 20, Express, Prisma, Zod, JWT + bcrypt, node-cron |
| Banco de dados | PostgreSQL 16 (Neon) |
| Serviços externos | Tabela TACO (base nutricional), Web Push e Brevo (notificações), Cloudinary (fotos) |
| Hospedagem | Vercel (Front End) e Render (API), em planos gratuitos |

Detalhes no [Documento de Arquitetura do Sistema](05-arquitetura-sistema.md).

---

## 🎨 Protótipo

Protótipo navegável com 39 telas, cobrindo todos os casos de uso específicos (usuário no celular; profissional e administrador no computador):

**https://claude.ai/artifact/6LHKsHMJNXdr1oEQj9DBRr**

Para navegar, abra o link e clique em **Play** em qualquer tela. A lista de telas e o roteiro de teste estão em [Protótipo do Sistema](06-prototipo.md).

---

## 📑 Sumário
1. [Briefing](00-briefing.md)
2. [Documento de Visão do Sistema](01-visao-sistema.md)  
3. [Diagrama de Caso de Uso](02-diagrama-caso-uso.md)  
4. [Especificação de Caso de Uso](03-especificacao-caso-uso.md)  
5. [Diagrama de Classe](04-diagrama-classes.md)  
6. [Documento de Arquitetura do Sistema](05-arquitetura-sistema.md)  
7. [Protótipo do Sistema](06-prototipo.md)  

---
