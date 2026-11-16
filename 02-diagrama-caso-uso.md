# 🎭 Diagrama de Caso de Uso

## 1. Introdução
O Diagrama de Caso de Uso é importante para o NutriCount porque mostra, de forma visual e do ponto de vista do usuário, quem interage com o sistema e quais funcionalidades ele oferece, sem detalhar como serão implementadas. Ele ajuda a delimitar o escopo, facilita a comunicação entre analistas, clientes e desenvolvedores e serve de base para as próximas etapas do projeto, como a especificação dos casos de uso, o diagrama de classes e o protótipo. Os diagramas foram elaborados a partir do Documento de Visão, e cada caso de uso tem um identificador exclusivo (UC1 a UC10), citado no item 4 do documento.  

## 2. Diagrama de Caso de Uso - Visão Macro
<img width="907" height="1027" alt="Diagrama de caso de uso - primeiro nível" src="https://github.com/user-attachments/assets/93bebb37-84bf-408e-a222-320873100123" />
  

## 3. Diagrama de Caso de Uso - Visão Detalhada
<img width="2851" height="2629" alt="Diagrama de caso de uso - segundo nível" src="https://github.com/user-attachments/assets/d5cecac9-dcdb-4467-a517-ada19424e587" />

## 4. Atores e coerência com os demais artefatos

| Ator | Casos de uso (primeiro nível) | Observação |
|---|---|---|
| Usuário | UC1, UC2, UC3, UC4, UC5, UC6, UC7, UC9 | Controla alimentação e hidratação |
| Profissional de Saúde | UC1, UC8, UC9, UC10 | Autentica-se (UC1.3), mantém o próprio cadastro e acompanha clientes |
| Administrador | UC1, UC3, UC8 | Gerencia contas, catálogo e validação de profissionais |
| Base Nutricional Externa (sistema) | UC3 | Fornece os dados nutricionais na importação (UC3.5) |
| Serviço de Notificação (sistema) | UC7 | Entrega os lembretes (UC7.3) |

No segundo nível, os três atores humanos são especializações do ator abstrato **Pessoa** (herança), que se associa ao UC1, pois todos precisam se autenticar (UC1.3). No Diagrama de Classes, a classe abstrata `Pessoa` é a generalização de `CadastroUsuario` e `CadastroProfissional`. Os atores dos casos de uso específicos descritos na Especificação de Caso de Uso são os mesmos deste diagrama.

Relacionamentos entre casos de uso: 16 «include» e 24 «extend», os mesmos listados na rastreabilidade do Diagrama de Classes.
