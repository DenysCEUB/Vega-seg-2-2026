# 📑 Especificação dos Casos de Uso Específicos — NutriCount

## 1. Introdução

Este documento detalha cada caso de uso **específico** do Diagrama de Caso de Uso do NutriCount (UC1.1 a UC10.3). Cada um está ligado ao seu caso de uso agrupador (UC1 a UC10) e é descrito com atores, pré e pós-condições, fluxo principal, fluxos alternativos, fluxos de exceção, regras de negócio e dados manipulados.

### 1.1 Convenções

| Sigla | Significado |
|---|---|
| **[UCx.y]** | Referência a outro caso de uso específico do diagrama |
| **FP** | Fluxo Principal |
| **FA** | Fluxo Alternativo: caminho válido diferente do principal |
| **FE** | Fluxo de Exceção: erro ou condição que impede a conclusão |
| **RN** | Regra de Negócio (numeração única para todo o documento) |
| **«include»** | Comportamento sempre executado pelo caso de uso agrupador |
| **«extend»** | Comportamento opcional, executado por escolha do ator ou por condição |

### 1.2 Atores

| Ator | Descrição |
|---|---|
| **Usuário** | Pessoa que usa o NutriCount para controlar alimentação e hidratação |
| **Administrador** | Responsável pela gestão de usuários, do catálogo de alimentos e dos profissionais |
| **Profissional de Saúde** | Nutricionista ou outro profissional que acompanha usuários vinculados |
| **Base Nutricional Externa** | Sistema externo que fornece dados nutricionais de alimentos (ex.: tabela TACO) |
| **Serviço de Notificação** | Sistema externo responsável por entregar notificações push e e-mails |

---

## 2. UC1 — Cadastro de Usuário

### UC1.1 — Incluir Usuário

| Item | Descrição |
|---|---|
| **Agrupador** | UC1 — Cadastro de Usuário («extend») |
| **Atores** | Usuário (primário), Administrador |
| **Descrição** | Permite criar uma nova conta de usuário no sistema, seja por autocadastro ou pelo Administrador. |
| **Pré-condições** | O e-mail informado ainda não está cadastrado. |
| **Pós-condições** | Conta criada com status **Ativo**; usuário apto a se autenticar [UC1.3] e a configurar seu perfil [UC2]. |
| **Dados** | Nome, e-mail, senha, data de nascimento, sexo, data de criação, status. |

**Fluxo Principal**
1. O ator seleciona a opção "Criar conta".
2. O sistema exibe o formulário de cadastro.
3. O ator informa nome, e-mail, senha, confirmação de senha, data de nascimento e sexo, e aceita os Termos de Uso.
4. O ator confirma o cadastro.
5. O sistema valida os dados (RN01, RN02, RN03).
6. O sistema grava o usuário com status **Ativo** e criptografa a senha.
7. O sistema exibe a mensagem "Cadastro realizado com sucesso" e direciona o usuário à configuração do perfil [UC2].

**Fluxos Alternativos**
- **FA01 — Inclusão pelo Administrador:** no passo 1, o Administrador autenticado [UC1.3] acessa "Usuários > Novo". No passo 6, o sistema gera uma senha provisória e a envia ao e-mail informado; no primeiro acesso, o usuário deve trocá-la.

**Fluxos de Exceção**
- **FE01 — E-mail já cadastrado:** no passo 5, o sistema informa "E-mail já cadastrado" e oferece a opção de recuperar senha.
- **FE02 — Dados inválidos:** no passo 5, o sistema destaca os campos inválidos e volta ao passo 3.
- **FE03 — Termos não aceitos:** no passo 5, o sistema impede o cadastro até que os Termos sejam aceitos.

**Regras de Negócio**
- **RN01:** O e-mail é único no sistema e deve ter formato válido.
- **RN02:** A senha deve ter no mínimo 8 caracteres, com ao menos uma letra e um número.
- **RN03:** O usuário deve ter no mínimo 16 anos.

---

### UC1.2 — Alterar Usuário

| Item | Descrição |
|---|---|
| **Agrupador** | UC1 — Cadastro de Usuário («extend») |
| **Atores** | Usuário (primário), Administrador |
| **Descrição** | Permite alterar os dados cadastrais da conta (nome, e-mail, senha). |
| **Pré-condições** | Ator autenticado [UC1.3]; conta com status **Ativo**. |
| **Pós-condições** | Dados cadastrais atualizados e alteração registrada no log de auditoria. |
| **Dados** | Nome, e-mail, senha, data de nascimento, data de atualização. |

**Fluxo Principal**
1. O Usuário acessa "Minha conta".
2. O sistema exibe os dados cadastrais atuais.
3. O Usuário altera os campos desejados.
4. O Usuário confirma a alteração.
5. O sistema valida os dados (RN01, RN03).
6. O sistema grava as alterações e exibe "Dados atualizados com sucesso".

**Fluxos Alternativos**
- **FA01 — Alteração de senha:** no passo 3, o Usuário informa a senha atual, a nova senha e a confirmação. O sistema valida a senha atual e a RN02 antes de gravar.
- **FA02 — Alteração de e-mail:** no passo 6, o sistema envia um link de confirmação ao novo e-mail; a troca só é efetivada após a confirmação (RN04).
- **FA03 — Alteração pelo Administrador:** o Administrador pesquisa o usuário em "Usuários", seleciona-o e segue a partir do passo 2. O Administrador não pode ver nem alterar a senha, apenas forçar a redefinição.

**Fluxos de Exceção**
- **FE01 — Senha atual incorreta:** o sistema informa o erro e mantém a senha anterior.
- **FE02 — Novo e-mail já cadastrado:** o sistema informa "E-mail já cadastrado" e volta ao passo 3.

**Regras de Negócio**
- **RN04:** A troca de e-mail só é efetivada após a confirmação pelo link enviado ao novo endereço, que expira em 24 horas.

---

### UC1.3 — Autenticar Usuário

| Item | Descrição |
|---|---|
| **Agrupador** | UC1 — Cadastro de Usuário («include») |
| **Atores** | Usuário (primário), Administrador, Profissional de Saúde |
| **Descrição** | Valida as credenciais de acesso e inicia uma sessão no sistema. |
| **Pré-condições** | Conta cadastrada e com status **Ativo**. |
| **Pós-condições** | Sessão iniciada com o perfil de acesso do ator (Usuário, Profissional ou Administrador). |
| **Dados** | E-mail, senha, token de sessão, data/hora do acesso, tentativas malsucedidas. |

**Fluxo Principal**
1. O ator acessa a tela de login.
2. O ator informa e-mail e senha.
3. O sistema valida as credenciais.
4. O sistema identifica o perfil de acesso e gera o token de sessão.
5. O sistema direciona o ator à tela inicial do seu perfil.

**Fluxos Alternativos**
- **FA01 — Recuperar senha:** no passo 2, o ator seleciona "Esqueci minha senha" e informa o e-mail. O sistema envia um link de redefinição válido por 1 hora; o ator define nova senha (RN02) e volta ao passo 1.
- **FA02 — Primeiro acesso com senha provisória:** no passo 4, se a senha for provisória, o sistema exige a troca antes de prosseguir.

**Fluxos de Exceção**
- **FE01 — Credenciais inválidas:** o sistema exibe "E-mail ou senha incorretos", sem indicar qual dos dois está errado.
- **FE02 — Conta bloqueada:** após 5 tentativas malsucedidas, o sistema bloqueia o acesso por 15 minutos (RN05).
- **FE03 — Conta inativa:** o sistema informa que a conta está desativada e orienta a contatar o suporte.

**Regras de Negócio**
- **RN05:** Após 5 tentativas de login malsucedidas consecutivas, a conta fica bloqueada por 15 minutos.
- **RN06:** A sessão expira após 30 dias sem uso no aplicativo móvel e após 2 horas de inatividade na versão web.

---

### UC1.4 — Desativar Usuário

| Item | Descrição |
|---|---|
| **Agrupador** | UC1 — Cadastro de Usuário («extend») |
| **Atores** | Usuário (primário), Administrador |
| **Descrição** | Desativa uma conta de usuário, impedindo novos acessos, sem excluir fisicamente o histórico. |
| **Pré-condições** | Ator autenticado [UC1.3]; conta com status **Ativo**. |
| **Pós-condições** | Conta com status **Inativo**; vínculos com profissionais encerrados [UC9.5]; lembretes desativados. |
| **Dados** | Status, data de desativação, motivo, responsável pela desativação. |

**Fluxo Principal**
1. O Usuário acessa "Minha conta > Desativar conta".
2. O sistema informa as consequências da desativação e solicita a senha e, opcionalmente, o motivo.
3. O Usuário informa a senha e confirma.
4. O sistema altera o status da conta para **Inativo**.
5. O sistema encerra os vínculos ativos com profissionais [UC9.5] e desativa os lembretes [UC7].
6. O sistema encerra a sessão e exibe a confirmação.

**Fluxos Alternativos**
- **FA01 — Desativação pelo Administrador:** o Administrador seleciona o usuário, informa obrigatoriamente o motivo e confirma. O sistema notifica o usuário por e-mail.
- **FA02 — Reativação:** dentro do prazo da RN07, o Usuário pode reativar a conta ao se autenticar e confirmar a reativação.

**Fluxos de Exceção**
- **FE01 — Senha incorreta:** o sistema não desativa a conta e volta ao passo 2.

**Regras de Negócio**
- **RN07:** A conta desativada pode ser reativada em até 90 dias. Após esse prazo, os dados pessoais são anonimizados, conforme a LGPD.
- **RN08:** O sistema não faz exclusão física de usuários; usa exclusão lógica (status).

---

## 3. UC2 — Perfil e Meta Calórica

### UC2.1 — Alterar Perfil

| Item | Descrição |
|---|---|
| **Agrupador** | UC2 — Perfil e Meta Calórica («extend») |
| **Atores** | Usuário |
| **Descrição** | Permite informar ou atualizar os dados físicos e o objetivo usados no cálculo nutricional. |
| **Pré-condições** | Usuário autenticado [UC1.3]. |
| **Pós-condições** | Perfil atualizado; necessidade calórica recalculada [UC2.3]; peso registrado no histórico. |
| **Dados** | Peso (kg), altura (cm), sexo, data de nascimento, nível de atividade física, objetivo (perder, manter ou ganhar peso). |

**Fluxo Principal**
1. O Usuário acessa "Meu perfil > Editar".
2. O sistema exibe os dados atuais do perfil [UC2.4].
3. O Usuário altera peso, altura, nível de atividade e/ou objetivo.
4. O Usuário confirma.
5. O sistema valida os dados (RN09).
6. O sistema grava o perfil e registra o novo peso no histórico com a data.
7. O sistema recalcula a necessidade calórica [UC2.3] e exibe o novo valor.

**Fluxos Alternativos**
- **FA01 — Primeiro preenchimento:** após o cadastro [UC1.1], o sistema exibe o perfil em formato de assistente (passo a passo) e, ao final, executa o [UC2.2].
- **FA02 — Meta personalizada existente:** no passo 7, se o Usuário tiver meta calórica personalizada, o sistema pergunta se deseja manter a meta atual ou adotar o novo valor calculado.

**Fluxos de Exceção**
- **FE01 — Valores fora da faixa:** o sistema informa a faixa aceita e volta ao passo 3.

**Regras de Negócio**
- **RN09:** Faixas aceitas: peso entre 30 e 300 kg; altura entre 100 e 250 cm.
- **RN10:** Níveis de atividade e fatores: sedentário (1,2), levemente ativo (1,375), moderadamente ativo (1,55), muito ativo (1,725), extremamente ativo (1,9).

---

### UC2.2 — Definir Meta Calórica

| Item | Descrição |
|---|---|
| **Agrupador** | UC2 — Perfil e Meta Calórica («extend») |
| **Atores** | Usuário |
| **Descrição** | Define a meta calórica diária, aceitando o valor sugerido pelo sistema ou informando um valor personalizado. |
| **Pré-condições** | Usuário autenticado [UC1.3]; perfil preenchido. |
| **Pós-condições** | Meta calórica diária vigente gravada, com data de início; relatórios passam a compará-la [UC5.3]. |
| **Dados** | Meta calórica (kcal/dia), origem (calculada ou personalizada), distribuição de macronutrientes (%), data de vigência. |

**Fluxo Principal**
1. O Usuário acessa "Meu perfil > Meta calórica".
2. O sistema calcula a necessidade calórica [UC2.3] e exibe o valor sugerido.
3. O Usuário aceita o valor sugerido.
4. O sistema grava a meta com origem **Calculada** e data de início igual à data atual.
5. O sistema exibe a meta e a distribuição padrão de macronutrientes (RN12).

**Fluxos Alternativos**
- **FA01 — Meta personalizada:** no passo 3, o Usuário informa outro valor (por exemplo, orientado por um nutricionista). O sistema valida a RN11 e grava com origem **Personalizada**.
- **FA02 — Ajuste de macronutrientes:** no passo 5, o Usuário altera os percentuais de proteínas, carboidratos e gorduras; o sistema valida que somam 100%.

**Fluxos de Exceção**
- **FE01 — Meta fora do limite seguro:** o sistema informa o limite (RN11) e não grava a meta.
- **FE02 — Perfil incompleto:** no passo 2, o sistema direciona o Usuário ao [UC2.1].

**Regras de Negócio**
- **RN11:** A meta calórica deve estar entre 1.200 e 5.000 kcal/dia.
- **RN12:** Distribuição padrão de macronutrientes: 50% carboidratos, 20% proteínas e 30% gorduras.
- **RN13:** Ao definir uma nova meta, a anterior é mantida no histórico com data de fim, para que relatórios de períodos passados usem a meta vigente na época.

---

### UC2.3 — Calcular Necessidade Calórica

| Item | Descrição |
|---|---|
| **Agrupador** | UC2 — Perfil e Meta Calórica («include») |
| **Atores** | Usuário (indireto, executado pelo sistema) |
| **Descrição** | Calcula a Taxa Metabólica Basal (TMB), o Gasto Energético Total (GET) e a necessidade calórica de acordo com o objetivo do Usuário. |
| **Pré-condições** | Perfil com peso, altura, sexo, idade e nível de atividade preenchidos. |
| **Pós-condições** | Valores de TMB, GET e necessidade calórica disponíveis para o [UC2.2] e o [UC2.4]. |
| **Dados** | Peso, altura, idade, sexo, fator de atividade, objetivo, TMB, GET, necessidade calórica. |

**Fluxo Principal**
1. O sistema obtém os dados do perfil do Usuário.
2. O sistema calcula a idade a partir da data de nascimento.
3. O sistema calcula a TMB pela fórmula de Mifflin-St Jeor (RN14).
4. O sistema calcula o GET multiplicando a TMB pelo fator de atividade (RN10).
5. O sistema aplica o ajuste do objetivo (RN15) e obtém a necessidade calórica.
6. O sistema retorna os valores ao caso de uso que o acionou.

**Fluxos Alternativos**
- **FA01 — Resultado abaixo do mínimo:** no passo 5, se o valor ficar abaixo de 1.200 kcal, o sistema adota 1.200 kcal e exibe um aviso recomendando acompanhamento profissional.

**Fluxos de Exceção**
- **FE01 — Dados insuficientes:** se algum dado obrigatório faltar, o sistema interrompe o cálculo e solicita o preenchimento do perfil [UC2.1].

**Regras de Negócio**
- **RN14:** TMB (Mifflin-St Jeor): homens = 10 × peso + 6,25 × altura − 5 × idade + 5; mulheres = 10 × peso + 6,25 × altura − 5 × idade − 161.
- **RN15:** Ajuste por objetivo: perder peso = GET − 500 kcal; manter = GET; ganhar peso = GET + 300 kcal.

---

### UC2.4 — Consultar Perfil

| Item | Descrição |
|---|---|
| **Agrupador** | UC2 — Perfil e Meta Calórica («include») |
| **Atores** | Usuário |
| **Descrição** | Exibe os dados do perfil, os indicadores calculados e a meta calórica vigente. |
| **Pré-condições** | Usuário autenticado [UC1.3]. |
| **Pós-condições** | Nenhuma alteração de dados. |
| **Dados** | Dados do perfil, IMC, TMB, GET, meta calórica vigente, histórico de peso. |

**Fluxo Principal**
1. O Usuário acessa "Meu perfil".
2. O sistema obtém os dados do perfil e a meta vigente.
3. O sistema calcula o IMC (RN16).
4. O sistema exibe dados físicos, objetivo, nível de atividade, IMC com classificação, TMB, GET e meta calórica.

**Fluxos Alternativos**
- **FA01 — Histórico de peso:** no passo 4, o Usuário seleciona "Ver evolução" e o sistema exibe o gráfico de peso ao longo do tempo.

**Fluxos de Exceção**
- **FE01 — Perfil não preenchido:** o sistema informa que o perfil está incompleto e oferece o [UC2.1].

**Regras de Negócio**
- **RN16:** IMC = peso ÷ altura² (altura em metros), classificado conforme a tabela da OMS.

---

## 4. UC3 — Catálogo de Alimentos

### UC3.1 — Incluir Alimento

| Item | Descrição |
|---|---|
| **Agrupador** | UC3 — Catálogo de Alimentos («extend») |
| **Atores** | Administrador (primário), Usuário |
| **Descrição** | Cadastra manualmente um novo alimento com suas informações nutricionais. |
| **Pré-condições** | Ator autenticado [UC1.3]. |
| **Pós-condições** | Alimento gravado com status **Ativo** e disponível para consulta [UC3.3]. |
| **Dados** | Nome, marca (opcional), categoria, porção de referência e unidade (g ou ml), calorias, proteínas, carboidratos, gorduras, fibras, sódio, origem, visibilidade, status. |

**Fluxo Principal**
1. O Administrador acessa "Catálogo > Novo alimento".
2. O sistema exibe o formulário.
3. O Administrador informa nome, categoria, porção de referência e valores nutricionais.
4. O Administrador confirma.
5. O sistema valida os dados (RN17, RN18).
6. O sistema grava o alimento com origem **Manual**, visibilidade **Pública** e status **Ativo**.
7. O sistema exibe a confirmação.

**Fluxos Alternativos**
- **FA01 — Alimento personalizado pelo Usuário:** o Usuário que não encontra um alimento [UC3.3] pode cadastrá-lo. No passo 6, o sistema grava com visibilidade **Privada**, visível apenas para esse Usuário (RN19).

**Fluxos de Exceção**
- **FE01 — Alimento duplicado:** no passo 5, se já existir alimento ativo com mesmo nome e marca, o sistema informa e sugere usar o existente.
- **FE02 — Valores inconsistentes:** no passo 5, o sistema informa a inconsistência (RN18) e volta ao passo 3.

**Regras de Negócio**
- **RN17:** Nome, porção de referência e calorias são obrigatórios; os demais nutrientes são opcionais.
- **RN18:** Nenhum valor nutricional pode ser negativo, e a soma de proteínas, carboidratos e gorduras não pode exceder a porção de referência em gramas.
- **RN19:** Alimentos criados por Usuários são privados; somente o Administrador pode publicar alimentos no catálogo geral.

---

### UC3.2 — Alterar Alimento

| Item | Descrição |
|---|---|
| **Agrupador** | UC3 — Catálogo de Alimentos («extend») |
| **Atores** | Administrador (primário), Usuário |
| **Descrição** | Altera os dados de um alimento do catálogo. |
| **Pré-condições** | Ator autenticado [UC1.3]; alimento cadastrado e com status **Ativo**. |
| **Pós-condições** | Alimento atualizado; refeições já registradas mantêm os valores da época (RN20). |
| **Dados** | Mesmos dados do [UC3.1], data de atualização, responsável. |

**Fluxo Principal**
1. O Administrador pesquisa o alimento [UC3.3] e seleciona "Editar".
2. O sistema exibe os dados atuais.
3. O Administrador altera os campos desejados.
4. O Administrador confirma.
5. O sistema valida os dados (RN17, RN18).
6. O sistema grava as alterações e exibe a confirmação.

**Fluxos Alternativos**
- **FA01 — Alteração pelo Usuário:** o Usuário só pode alterar alimentos privados criados por ele (RN19).
- **FA02 — Alimento importado:** se o alimento tiver origem **Base Externa**, o sistema avisa que a alteração deixa de acompanhar as atualizações da base e muda a origem para **Manual**.

**Fluxos de Exceção**
- **FE01 — Sem permissão:** se o Usuário tentar alterar um alimento público, o sistema nega a operação.
- **FE02 — Dados inválidos:** o sistema informa o erro e volta ao passo 3.

**Regras de Negócio**
- **RN20:** Cada refeição registrada guarda uma cópia dos valores nutricionais do alimento no momento do registro; alterações posteriores no catálogo não mudam o histórico.

---

### UC3.3 — Consultar Alimento

| Item | Descrição |
|---|---|
| **Agrupador** | UC3 — Catálogo de Alimentos («include»); incluído também por [UC4.1] |
| **Atores** | Usuário (primário), Administrador |
| **Descrição** | Pesquisa alimentos no catálogo e exibe suas informações nutricionais. |
| **Pré-condições** | Ator autenticado [UC1.3]. |
| **Pós-condições** | Nenhuma alteração de dados; alimento selecionado pode ser usado em outro caso de uso. |
| **Dados** | Termo de busca, filtros (categoria, origem), lista de alimentos, detalhes nutricionais. |

**Fluxo Principal**
1. O ator acessa a busca de alimentos.
2. O ator digita ao menos 3 caracteres do nome.
3. O sistema pesquisa alimentos ativos públicos e os privados do próprio Usuário (RN21).
4. O sistema exibe a lista com nome, marca, porção de referência e calorias.
5. O ator seleciona um alimento.
6. O sistema exibe os detalhes nutricionais completos.

**Fluxos Alternativos**
- **FA01 — Filtros:** no passo 2, o ator filtra por categoria ou origem.
- **FA02 — Alimentos recentes e favoritos:** no passo 1, o sistema exibe os alimentos usados recentemente pelo Usuário antes da digitação.
- **FA03 — Busca na base externa:** no passo 4, se não houver resultados, o sistema oferece buscar na Base Nutricional Externa [UC3.5].
- **FA04 — Visão do Administrador:** o Administrador vê também os alimentos inativos.

**Fluxos de Exceção**
- **FE01 — Nenhum resultado:** o sistema informa que não encontrou alimentos e oferece incluir um alimento [UC3.1] ou importar [UC3.5].

**Regras de Negócio**
- **RN21:** A busca ignora acentos e maiúsculas/minúsculas e ordena os resultados por relevância e frequência de uso.

---

### UC3.4 — Desativar Alimento

| Item | Descrição |
|---|---|
| **Agrupador** | UC3 — Catálogo de Alimentos («extend») |
| **Atores** | Administrador (primário), Usuário |
| **Descrição** | Retira um alimento das buscas sem apagar o histórico de refeições que o utilizam. |
| **Pré-condições** | Ator autenticado [UC1.3]; alimento com status **Ativo**. |
| **Pós-condições** | Alimento com status **Inativo**, fora das buscas; registros antigos preservados. |
| **Dados** | Status, motivo, data de desativação, responsável. |

**Fluxo Principal**
1. O Administrador pesquisa o alimento [UC3.3] e seleciona "Desativar".
2. O sistema solicita o motivo (ex.: duplicado, dados incorretos).
3. O Administrador informa o motivo e confirma.
4. O sistema altera o status para **Inativo** e exibe a confirmação.

**Fluxos Alternativos**
- **FA01 — Reativação:** o Administrador filtra alimentos inativos, seleciona um e escolhe "Reativar".
- **FA02 — Desativação pelo Usuário:** o Usuário pode desativar apenas seus alimentos privados, sem informar motivo.

**Fluxos de Exceção**
- **FE01 — Sem permissão:** o sistema nega a operação se o Usuário tentar desativar um alimento público.

**Regras de Negócio**
- **RN22:** Alimentos não são excluídos fisicamente, pois podem estar referenciados em refeições registradas.

---

### UC3.5 — Importar Alimento da Base Externa

| Item | Descrição |
|---|---|
| **Agrupador** | UC3 — Catálogo de Alimentos («extend») |
| **Atores** | Administrador (primário), Usuário, Base Nutricional Externa (secundário) |
| **Descrição** | Busca alimentos em uma base nutricional externa e os importa para o catálogo. |
| **Pré-condições** | Ator autenticado [UC1.3]; integração com a Base Nutricional Externa configurada. |
| **Pós-condições** | Alimento gravado no catálogo com origem **Base Externa** e código de referência. |
| **Dados** | Termo de busca, código do alimento na base externa, dados nutricionais, data da importação. |

**Fluxo Principal**
1. O Administrador acessa "Catálogo > Importar da base externa".
2. O Administrador informa o termo de busca.
3. O sistema consulta a Base Nutricional Externa.
4. A Base Nutricional Externa retorna os alimentos encontrados.
5. O sistema exibe os resultados, indicando os que já existem no catálogo.
6. O Administrador seleciona um ou mais alimentos e confirma.
7. O sistema converte os dados para o padrão do catálogo (RN23) e grava com origem **Base Externa**, visibilidade **Pública** e status **Ativo**.
8. O sistema exibe o resumo da importação.

**Fluxos Alternativos**
- **FA01 — Importação pelo Usuário:** a partir do [UC3.3] (FA03), o Usuário importa um alimento; ele fica disponível de imediato para o Usuário e é enviado para aprovação do Administrador antes de ficar público.
- **FA02 — Atualização de alimento já importado:** no passo 6, se o alimento já existir, o sistema oferece atualizar os valores em vez de duplicar.

**Fluxos de Exceção**
- **FE01 — Base externa indisponível:** no passo 3, o sistema informa a indisponibilidade e sugere o cadastro manual [UC3.1].
- **FE02 — Dados incompletos:** no passo 7, alimentos sem calorias ou porção de referência não são importados e aparecem no resumo como rejeitados.

**Regras de Negócio**
- **RN23:** Os valores importados são convertidos para a porção de referência de 100 g ou 100 ml.
- **RN24:** O código de origem é guardado para impedir importações duplicadas do mesmo alimento.

---

## 5. UC4 — Diário Alimentar

### UC4.1 — Registrar Refeição

| Item | Descrição |
|---|---|
| **Agrupador** | UC4 — Diário Alimentar («extend») |
| **Atores** | Usuário |
| **Descrição** | Registra uma refeição no diário, com os alimentos e as quantidades consumidas. |
| **Pré-condições** | Usuário autenticado [UC1.3]. |
| **Pós-condições** | Refeição gravada no diário do dia; totais diários e saldo da meta atualizados. |
| **Dados** | Data, horário, tipo de refeição, itens (alimento, quantidade, unidade), valores nutricionais calculados, observação, foto (opcional). |

**Fluxo Principal**
1. O Usuário acessa o diário e seleciona "Adicionar refeição".
2. O sistema exibe o formulário com a data atual e sugere o tipo de refeição pelo horário (RN25).
3. O Usuário confirma ou altera data, horário e tipo de refeição.
4. O Usuário busca e seleciona um alimento [UC3.3].
5. O Usuário informa a quantidade consumida.
6. O sistema calcula os valores nutricionais do item proporcionalmente à porção de referência (RN26).
7. O Usuário repete os passos 4 a 6 para cada alimento.
8. O Usuário confirma o registro.
9. O sistema grava a refeição com uma cópia dos valores nutricionais (RN20).
10. O sistema atualiza os totais do dia e exibe o saldo em relação à meta calórica.

**Fluxos Alternativos**
- **FA01 — Anexar foto:** no passo 8, o Usuário pode anexar uma foto da refeição [UC4.5].
- **FA02 — Medidas caseiras:** no passo 5, o Usuário escolhe uma medida caseira (colher, xícara, unidade) e o sistema converte para gramas.
- **FA03 — Repetir refeição anterior:** no passo 4, o Usuário seleciona uma refeição de outro dia e o sistema copia seus itens.
- **FA04 — Registro retroativo:** no passo 3, o Usuário escolhe uma data passada (RN27).

**Fluxos de Exceção**
- **FE01 — Refeição sem itens:** no passo 8, o sistema exige ao menos um alimento.
- **FE02 — Quantidade inválida:** no passo 5, o sistema rejeita quantidades menores ou iguais a zero.
- **FE03 — Data futura:** no passo 3, o sistema não aceita datas futuras.

**Regras de Negócio**
- **RN25:** Tipos de refeição: café da manhã, lanche da manhã, almoço, lanche da tarde, jantar e ceia. A sugestão é feita pelos horários definidos em [UC7.1] ou, na falta deles, por faixas padrão.
- **RN26:** Valor do item = (quantidade consumida ÷ porção de referência) × valor nutricional da porção.
- **RN27:** Refeições podem ser registradas ou alteradas com até 7 dias de retroatividade.

---

### UC4.2 — Alterar Refeição

| Item | Descrição |
|---|---|
| **Agrupador** | UC4 — Diário Alimentar («extend») |
| **Atores** | Usuário |
| **Descrição** | Altera uma refeição já registrada (itens, quantidades, horário ou tipo). |
| **Pré-condições** | Usuário autenticado [UC1.3]; refeição registrada pelo próprio Usuário, dentro do prazo da RN27. |
| **Pós-condições** | Refeição atualizada; totais do dia recalculados. |
| **Dados** | Mesmos dados do [UC4.1], data de atualização. |

**Fluxo Principal**
1. O Usuário consulta o diário [UC4.4] e seleciona a refeição.
2. O Usuário seleciona "Editar".
3. O sistema exibe os itens e dados da refeição.
4. O Usuário altera quantidades, horário, tipo ou observação.
5. O Usuário confirma.
6. O sistema recalcula os valores (RN26), grava e atualiza os totais do dia.

**Fluxos Alternativos**
- **FA01 — Incluir item:** no passo 4, o Usuário adiciona outro alimento [UC3.3].
- **FA02 — Remover item:** no passo 4, o Usuário remove um alimento da refeição.
- **FA03 — Trocar foto:** no passo 4, o Usuário substitui ou remove a foto [UC4.5].

**Fluxos de Exceção**
- **FE01 — Prazo expirado:** o sistema informa que a refeição não pode mais ser alterada (RN27).
- **FE02 — Refeição sem itens:** se todos os itens forem removidos, o sistema sugere excluir a refeição [UC4.3].

---

### UC4.3 — Excluir Refeição

| Item | Descrição |
|---|---|
| **Agrupador** | UC4 — Diário Alimentar («extend») |
| **Atores** | Usuário |
| **Descrição** | Remove uma refeição registrada no diário. |
| **Pré-condições** | Usuário autenticado [UC1.3]; refeição do próprio Usuário, dentro do prazo da RN27. |
| **Pós-condições** | Refeição removida do diário; totais do dia recalculados. |
| **Dados** | Identificador da refeição, data da exclusão. |

**Fluxo Principal**
1. O Usuário consulta o diário [UC4.4] e seleciona a refeição.
2. O Usuário seleciona "Excluir".
3. O sistema pede confirmação.
4. O Usuário confirma.
5. O sistema remove a refeição e a foto associada, recalcula os totais e exibe a confirmação.

**Fluxos Alternativos**
- **FA01 — Desfazer:** por 10 segundos após o passo 5, o sistema exibe a opção "Desfazer", que restaura a refeição.

**Fluxos de Exceção**
- **FE01 — Prazo expirado:** o sistema informa que a refeição não pode mais ser excluída (RN27).

**Regras de Negócio**
- **RN28:** A exclusão de refeições é física, pois são dados do próprio Usuário; a foto anexada também é removida do armazenamento.

---

### UC4.4 — Consultar Diário

| Item | Descrição |
|---|---|
| **Agrupador** | UC4 — Diário Alimentar («include») |
| **Atores** | Usuário |
| **Descrição** | Exibe as refeições de um dia, agrupadas por tipo, com os totais nutricionais. |
| **Pré-condições** | Usuário autenticado [UC1.3]. |
| **Pós-condições** | Nenhuma alteração de dados. |
| **Dados** | Data, refeições e itens, totais de calorias e macronutrientes, meta do dia. |

**Fluxo Principal**
1. O Usuário acessa "Diário".
2. O sistema exibe o dia atual.
3. O sistema lista as refeições agrupadas por tipo, com seus itens e calorias.
4. O sistema exibe o total do dia, a meta calórica e o saldo (meta − consumido).

**Fluxos Alternativos**
- **FA01 — Navegar entre dias:** o Usuário avança ou retrocede a data, ou escolhe uma data no calendário.
- **FA02 — Detalhar refeição:** o Usuário seleciona uma refeição e vê os macronutrientes de cada item e a foto, se houver.

**Fluxos de Exceção**
- **FE01 — Dia sem registros:** o sistema informa que não há refeições e oferece registrar uma [UC4.1].

---

### UC4.5 — Anexar Foto da Refeição

| Item | Descrição |
|---|---|
| **Agrupador** | UC4 — Diário Alimentar; estende [UC4.1] («extend») |
| **Atores** | Usuário |
| **Descrição** | Anexa uma foto à refeição, como registro visual do que foi consumido. |
| **Pré-condições** | Usuário registrando [UC4.1] ou alterando [UC4.2] uma refeição; permissão de câmera ou galeria concedida. |
| **Pós-condições** | Foto armazenada e associada à refeição. |
| **Dados** | Arquivo de imagem, tamanho, formato, data de envio. |

**Fluxo Principal**
1. O Usuário seleciona "Adicionar foto".
2. O sistema oferece as opções "Câmera" e "Galeria".
3. O Usuário tira ou escolhe a foto.
4. O sistema valida o arquivo (RN29).
5. O sistema comprime a imagem e exibe a pré-visualização.
6. O Usuário confirma e o sistema associa a foto à refeição.

**Fluxos Alternativos**
- **FA01 — Remover foto:** na pré-visualização, o Usuário remove a foto antes de salvar.

**Fluxos de Exceção**
- **FE01 — Arquivo inválido:** o sistema informa o formato ou tamanho aceito.
- **FE02 — Permissão negada:** o sistema orienta o Usuário a liberar o acesso à câmera ou à galeria nas configurações do aparelho.
- **FE03 — Falha no envio:** o sistema salva a refeição sem a foto e permite reenviá-la depois.

**Regras de Negócio**
- **RN29:** Formatos aceitos: JPG, PNG e HEIC; tamanho máximo de 10 MB; uma foto por refeição.

---

## 6. UC5 — Relatórios de Consumo

### UC5.1 — Consultar Resumo Diário

| Item | Descrição |
|---|---|
| **Agrupador** | UC5 — Relatórios de Consumo («include») |
| **Atores** | Usuário |
| **Descrição** | Exibe o resumo nutricional de um dia: calorias, macronutrientes e distribuição por refeição. |
| **Pré-condições** | Usuário autenticado [UC1.3]. |
| **Pós-condições** | Nenhuma alteração de dados. |
| **Dados** | Data, total de calorias, proteínas, carboidratos, gorduras, fibras, calorias por refeição, consumo de água. |

**Fluxo Principal**
1. O Usuário acessa "Relatórios > Resumo do dia".
2. O sistema seleciona a data atual.
3. O sistema soma os valores de todas as refeições do dia.
4. O sistema exibe o total de calorias, o gráfico de macronutrientes (em gramas e percentual) e as calorias por refeição.
5. O sistema exibe a comparação com a meta [UC5.3] e o consumo de água do dia.

**Fluxos Alternativos**
- **FA01 — Outra data:** no passo 2, o Usuário escolhe outra data.

**Fluxos de Exceção**
- **FE01 — Sem registros:** o sistema informa que não há dados para o dia selecionado.

**Regras de Negócio**
- **RN30:** Percentual de calorias de cada macronutriente: proteínas e carboidratos = 4 kcal/g; gorduras = 9 kcal/g.

---

### UC5.2 — Consultar Histórico

| Item | Descrição |
|---|---|
| **Agrupador** | UC5 — Relatórios de Consumo («include») |
| **Atores** | Usuário |
| **Descrição** | Exibe a evolução do consumo em um período, com médias e gráficos. |
| **Pré-condições** | Usuário autenticado [UC1.3]. |
| **Pós-condições** | Nenhuma alteração de dados. |
| **Dados** | Período, calorias e macronutrientes por dia, médias, dias registrados, evolução do peso. |

**Fluxo Principal**
1. O Usuário acessa "Relatórios > Histórico".
2. O sistema seleciona por padrão os últimos 7 dias.
3. O sistema calcula as médias diárias considerando apenas os dias com registro (RN31).
4. O sistema exibe o gráfico de calorias por dia com a linha da meta, as médias de macronutrientes e a quantidade de dias registrados.

**Fluxos Alternativos**
- **FA01 — Alterar período:** no passo 2, o Usuário escolhe 30 dias, 90 dias ou um período personalizado (RN32).
- **FA02 — Exportar:** no passo 4, o Usuário exporta o relatório em PDF ou CSV.

**Fluxos de Exceção**
- **FE01 — Período sem registros:** o sistema informa que não há dados no período.
- **FE02 — Período inválido:** o sistema rejeita datas finais anteriores às iniciais ou períodos acima do limite.

**Regras de Negócio**
- **RN31:** Dias sem nenhum registro não entram no cálculo das médias.
- **RN32:** O período personalizado pode ter no máximo 365 dias.

---

### UC5.3 — Comparar Consumo com Meta

| Item | Descrição |
|---|---|
| **Agrupador** | UC5 — Relatórios de Consumo («include») |
| **Atores** | Usuário |
| **Descrição** | Compara o consumo real com a meta calórica e de macronutrientes, indicando a adesão. |
| **Pré-condições** | Usuário autenticado [UC1.3]; meta calórica definida [UC2.2]. |
| **Pós-condições** | Nenhuma alteração de dados. |
| **Dados** | Consumo do dia ou período, meta vigente em cada data, diferença absoluta e percentual, situação. |

**Fluxo Principal**
1. O sistema obtém o consumo do dia ou período consultado.
2. O sistema obtém a meta vigente em cada data (RN13).
3. O sistema calcula a diferença entre consumo e meta, em kcal e em percentual.
4. O sistema classifica cada dia (RN33).
5. O sistema exibe a comparação com indicadores visuais e, no caso de períodos, o percentual de dias dentro da meta.

**Fluxos Alternativos**
- **FA01 — Comparação de macronutrientes:** o Usuário escolhe ver a comparação de proteínas, carboidratos e gorduras com a distribuição definida.

**Fluxos de Exceção**
- **FE01 — Meta não definida:** o sistema exibe apenas o consumo e oferece definir a meta [UC2.2].

**Regras de Negócio**
- **RN33:** Classificação: **Dentro da meta** (até 10% acima ou abaixo), **Abaixo da meta** (mais de 10% abaixo) e **Acima da meta** (mais de 10% acima).

---

## 7. UC6 — Controle de Hidratação

### UC6.1 — Definir Meta de Água

| Item | Descrição |
|---|---|
| **Agrupador** | UC6 — Controle de Hidratação («extend») |
| **Atores** | Usuário |
| **Descrição** | Define a meta diária de consumo de água, sugerida pelo peso ou personalizada. |
| **Pré-condições** | Usuário autenticado [UC1.3]. |
| **Pós-condições** | Meta de água vigente gravada. |
| **Dados** | Meta diária (ml), origem (calculada ou personalizada), data de vigência. |

**Fluxo Principal**
1. O Usuário acessa "Hidratação > Meta".
2. O sistema calcula e exibe a meta sugerida (RN34).
3. O Usuário aceita a sugestão.
4. O sistema grava a meta e exibe a confirmação.

**Fluxos Alternativos**
- **FA01 — Meta personalizada:** no passo 3, o Usuário informa outro valor, validado pela RN35.

**Fluxos de Exceção**
- **FE01 — Peso não informado:** o sistema não sugere meta e pede que o Usuário informe o valor manualmente ou complete o perfil [UC2.1].
- **FE02 — Valor fora da faixa:** o sistema informa a faixa aceita.

**Regras de Negócio**
- **RN34:** Meta sugerida = 35 ml × peso (kg), arredondada para múltiplos de 50 ml.
- **RN35:** A meta de água deve estar entre 1.000 e 6.000 ml/dia.

---

### UC6.2 — Registrar Consumo de Água

| Item | Descrição |
|---|---|
| **Agrupador** | UC6 — Controle de Hidratação («extend») |
| **Atores** | Usuário |
| **Descrição** | Registra uma quantidade de água ingerida. |
| **Pré-condições** | Usuário autenticado [UC1.3]. |
| **Pós-condições** | Registro gravado; total do dia e percentual da meta atualizados. |
| **Dados** | Data, horário, quantidade (ml). |

**Fluxo Principal**
1. O Usuário acessa "Hidratação".
2. O sistema exibe o total do dia, a meta e atalhos de quantidade (copo de 200 ml, garrafa de 500 ml).
3. O Usuário seleciona um atalho.
4. O sistema grava o registro com data e horário atuais.
5. O sistema atualiza o total do dia e o percentual da meta.

**Fluxos Alternativos**
- **FA01 — Quantidade personalizada:** no passo 3, o Usuário digita a quantidade.
- **FA02 — Meta atingida:** no passo 5, ao atingir 100% da meta, o sistema exibe uma mensagem de parabéns.

**Fluxos de Exceção**
- **FE01 — Quantidade inválida:** o sistema rejeita valores menores ou iguais a zero ou acima de 2.000 ml por registro (RN36).

**Regras de Negócio**
- **RN36:** Cada registro de água deve ter entre 1 e 2.000 ml.

---

### UC6.3 — Alterar Consumo de Água

| Item | Descrição |
|---|---|
| **Agrupador** | UC6 — Controle de Hidratação («extend») |
| **Atores** | Usuário |
| **Descrição** | Corrige ou remove um registro de consumo de água. |
| **Pré-condições** | Usuário autenticado [UC1.3]; registro existente, dentro do prazo da RN27. |
| **Pós-condições** | Registro alterado ou removido; total do dia recalculado. |
| **Dados** | Identificador do registro, quantidade, horário. |

**Fluxo Principal**
1. O Usuário acessa "Hidratação" e abre a lista de registros do dia.
2. O Usuário seleciona um registro e escolhe "Editar".
3. O Usuário altera a quantidade ou o horário.
4. O Usuário confirma.
5. O sistema valida (RN36), grava e recalcula o total do dia.

**Fluxos Alternativos**
- **FA01 — Remover registro:** no passo 2, o Usuário escolhe "Remover" e confirma; o sistema exclui o registro e recalcula o total.

**Fluxos de Exceção**
- **FE01 — Prazo expirado:** o sistema informa que o registro não pode mais ser alterado (RN27).
- **FE02 — Quantidade inválida:** o sistema informa o erro e volta ao passo 3.

---

### UC6.4 — Consultar Evolução da Hidratação

| Item | Descrição |
|---|---|
| **Agrupador** | UC6 — Controle de Hidratação («include») |
| **Atores** | Usuário |
| **Descrição** | Exibe a evolução do consumo de água em um período, comparada à meta. |
| **Pré-condições** | Usuário autenticado [UC1.3]. |
| **Pós-condições** | Nenhuma alteração de dados. |
| **Dados** | Período, total por dia, meta por dia, média, dias com meta atingida, sequência de dias. |

**Fluxo Principal**
1. O Usuário acessa "Hidratação > Evolução".
2. O sistema seleciona por padrão os últimos 7 dias.
3. O sistema calcula o total por dia, a média do período e os dias com meta atingida.
4. O sistema exibe o gráfico diário com a linha da meta, a média e a sequência atual de dias com meta atingida.

**Fluxos Alternativos**
- **FA01 — Alterar período:** o Usuário escolhe 30 dias, 90 dias ou um período personalizado (RN32).

**Fluxos de Exceção**
- **FE01 — Sem registros:** o sistema informa que não há registros no período.

---

## 8. UC7 — Lembretes de Refeição

### UC7.1 — Definir Horários de Refeições

| Item | Descrição |
|---|---|
| **Agrupador** | UC7 — Lembretes de Refeição («extend») |
| **Atores** | Usuário |
| **Descrição** | Define os horários das refeições para os quais o Usuário quer receber lembretes. |
| **Pré-condições** | Usuário autenticado [UC1.3]. |
| **Pós-condições** | Horários gravados e lembretes agendados [UC7.3]. |
| **Dados** | Tipo de refeição, horário, dias da semana, ativo/inativo, antecedência do lembrete, fuso horário. |

**Fluxo Principal**
1. O Usuário acessa "Lembretes".
2. O sistema exibe os tipos de refeição com horários sugeridos.
3. O Usuário ativa os tipos desejados e define o horário de cada um.
4. O Usuário escolhe os dias da semana.
5. O Usuário confirma.
6. O sistema valida os horários (RN37), grava e agenda os lembretes.

**Fluxos Alternativos**
- **FA01 — Lembrete de água:** no passo 3, o Usuário ativa também lembretes de hidratação em intervalos fixos (ex.: a cada 2 horas).
- **FA02 — Permissão de notificação:** no passo 6, se o aparelho ainda não permitiu notificações, o sistema solicita a permissão.

**Fluxos de Exceção**
- **FE01 — Horários conflitantes:** o sistema informa quando dois lembretes ficam a menos de 30 minutos um do outro.
- **FE02 — Notificações negadas:** o sistema grava os horários, mas avisa que os lembretes não serão exibidos até a permissão ser concedida.

**Regras de Negócio**
- **RN37:** O intervalo mínimo entre lembretes de refeição é de 30 minutos.

---

### UC7.2 — Alterar Horários de Refeições

| Item | Descrição |
|---|---|
| **Agrupador** | UC7 — Lembretes de Refeição («extend») |
| **Atores** | Usuário |
| **Descrição** | Altera, pausa ou desativa lembretes já definidos. |
| **Pré-condições** | Usuário autenticado [UC1.3]; ao menos um horário definido [UC7.1]. |
| **Pós-condições** | Horários atualizados e lembretes reagendados. |
| **Dados** | Mesmos dados do [UC7.1]. |

**Fluxo Principal**
1. O Usuário acessa "Lembretes".
2. O sistema exibe os lembretes cadastrados.
3. O Usuário altera horário, dias da semana ou antecedência de um lembrete.
4. O Usuário confirma.
5. O sistema valida (RN37), grava, cancela o agendamento antigo e agenda o novo.

**Fluxos Alternativos**
- **FA01 — Desativar lembrete:** no passo 3, o Usuário desativa um tipo de refeição; o sistema cancela os agendamentos dele.
- **FA02 — Pausar todos:** o Usuário pausa todos os lembretes por um período (ex.: férias); o sistema os reativa automaticamente ao fim do período.

**Fluxos de Exceção**
- **FE01 — Horários conflitantes:** o sistema informa o conflito e volta ao passo 3.

---

### UC7.3 — Enviar Lembrete de Refeição

| Item | Descrição |
|---|---|
| **Agrupador** | UC7 — Lembretes de Refeição («include») |
| **Atores** | Serviço de Notificação (secundário), Usuário (receptor) |
| **Descrição** | Envia automaticamente o lembrete no horário definido. |
| **Pré-condições** | Lembrete ativo [UC7.1]; conta do Usuário ativa. |
| **Pós-condições** | Lembrete enviado e registrado no log de envios. |
| **Dados** | Lembrete, mensagem, data/hora do envio, situação do envio. |

**Fluxo Principal**
1. O sistema identifica, a cada minuto, os lembretes cujo horário foi atingido, considerando o fuso horário do Usuário.
2. O sistema verifica se a refeição daquele tipo já foi registrada no dia (RN38).
3. O sistema monta a mensagem (ex.: "Hora do almoço! Não esqueça de registrar sua refeição").
4. O sistema envia a mensagem ao Serviço de Notificação.
5. O Serviço de Notificação entrega a notificação no aparelho do Usuário.
6. O sistema registra o envio.

**Fluxos Alternativos**
- **FA01 — Refeição já registrada:** no passo 2, se a refeição já foi registrada, o sistema não envia o lembrete.
- **FA02 — Abrir pela notificação:** após o passo 5, o Usuário toca na notificação e o sistema abre o registro de refeição [UC4.1] com o tipo já preenchido.

**Fluxos de Exceção**
- **FE01 — Falha no envio:** se o Serviço de Notificação não confirmar a entrega, o sistema tenta novamente até 3 vezes, com intervalo de 1 minuto, e registra a falha.
- **FE02 — Token do aparelho inválido:** o sistema desativa o token e aguarda o próximo login do Usuário para registrar um novo.

**Regras de Negócio**
- **RN38:** O lembrete não é enviado se a refeição daquele tipo já tiver sido registrada no dia.
- **RN39:** Lembretes respeitam o fuso horário configurado no aparelho do Usuário.

---

## 9. UC8 — Cadastro de Profissional

### UC8.1 — Incluir Profissional

| Item | Descrição |
|---|---|
| **Agrupador** | UC8 — Cadastro de Profissional («extend») |
| **Atores** | Profissional de Saúde (primário), Administrador |
| **Descrição** | Cadastra um profissional de saúde que poderá acompanhar usuários. |
| **Pré-condições** | O e-mail e o registro profissional ainda não estão cadastrados. |
| **Pós-condições** | Profissional gravado com status **Pendente de validação**; registro enviado para validação [UC8.3]. |
| **Dados** | Nome, e-mail, senha, CPF, profissão, conselho profissional (CRN, CRM, CREF etc.), número de registro, UF, especialidades, foto, descrição, status. |

**Fluxo Principal**
1. O Profissional acessa "Sou profissional de saúde > Cadastrar".
2. O sistema exibe o formulário.
3. O Profissional informa dados pessoais, de acesso e profissionais, e aceita os Termos de Uso.
4. O Profissional confirma.
5. O sistema valida os dados (RN01, RN02, RN40).
6. O sistema grava o profissional com status **Pendente de validação**.
7. O sistema aciona a validação do registro profissional [UC8.3].
8. O sistema informa que o perfil ficará visível aos usuários após a validação.

**Fluxos Alternativos**
- **FA01 — Inclusão pelo Administrador:** o Administrador cadastra o profissional; o sistema envia uma senha provisória ao e-mail informado e segue para o passo 7.

**Fluxos de Exceção**
- **FE01 — E-mail ou registro já cadastrado:** o sistema informa a duplicidade e não grava.
- **FE02 — Dados inválidos:** o sistema destaca os campos e volta ao passo 3.

**Regras de Negócio**
- **RN40:** A combinação conselho + número de registro + UF é única no sistema; o CPF também é único.
- **RN41:** Somente profissionais com status **Validado** aparecem na busca [UC9.1] e podem ter vínculos.

---

### UC8.2 — Alterar Profissional

| Item | Descrição |
|---|---|
| **Agrupador** | UC8 — Cadastro de Profissional («extend») |
| **Atores** | Profissional de Saúde (primário), Administrador |
| **Descrição** | Altera os dados do cadastro profissional. |
| **Pré-condições** | Profissional autenticado [UC1.3]; cadastro com status diferente de **Inativo**. |
| **Pós-condições** | Dados atualizados; se o registro profissional mudou, nova validação pendente. |
| **Dados** | Mesmos dados do [UC8.1], data de atualização. |

**Fluxo Principal**
1. O Profissional acessa "Meu perfil profissional > Editar".
2. O sistema exibe os dados atuais.
3. O Profissional altera foto, descrição, especialidades ou contato.
4. O Profissional confirma.
5. O sistema valida e grava as alterações.

**Fluxos Alternativos**
- **FA01 — Alteração do registro profissional:** se o Profissional alterar conselho, número ou UF, o sistema muda o status para **Pendente de validação** e aciona o [UC8.3]; os vínculos ativos são mantidos.
- **FA02 — Alteração pelo Administrador:** o Administrador pesquisa o profissional e segue a partir do passo 2.

**Fluxos de Exceção**
- **FE01 — Registro duplicado:** o sistema informa que o registro já pertence a outro cadastro (RN40).

---

### UC8.3 — Validar Registro Profissional

| Item | Descrição |
|---|---|
| **Agrupador** | UC8 — Cadastro de Profissional («include») |
| **Atores** | Administrador (primário), Profissional de Saúde (receptor) |
| **Descrição** | Verifica se o registro no conselho profissional é válido e ativo, aprovando ou rejeitando o cadastro. |
| **Pré-condições** | Profissional com status **Pendente de validação**; Administrador autenticado [UC1.3]. |
| **Pós-condições** | Status alterado para **Validado** ou **Rejeitado**; Profissional notificado. |
| **Dados** | Conselho, número, UF, situação no conselho, documento comprobatório, decisão, motivo, data e responsável. |

**Fluxo Principal**
1. O Administrador acessa "Profissionais > Pendentes de validação".
2. O sistema lista os cadastros pendentes por ordem de chegada.
3. O Administrador seleciona um cadastro.
4. O sistema exibe os dados profissionais e o documento enviado.
5. O Administrador consulta o registro no site do conselho correspondente.
6. O Administrador aprova o cadastro.
7. O sistema altera o status para **Validado**, registra a decisão e notifica o Profissional por e-mail.

**Fluxos Alternativos**
- **FA01 — Rejeição:** no passo 6, o Administrador rejeita e informa o motivo; o sistema altera o status para **Rejeitado** e notifica o Profissional, que pode corrigir os dados [UC8.2] e reenviar.
- **FA02 — Solicitar documento:** no passo 6, o Administrador pede um documento complementar; o cadastro continua pendente.

**Fluxos de Exceção**
- **FE01 — Registro inativo ou inexistente no conselho:** o Administrador rejeita o cadastro conforme o FA01.

**Regras de Negócio**
- **RN42:** A validação deve ser concluída em até 3 dias úteis após o cadastro.
- **RN43:** A validação é revista anualmente; se o registro deixar de estar ativo, o profissional é desativado [UC8.4].

---

### UC8.4 — Desativar Profissional

| Item | Descrição |
|---|---|
| **Agrupador** | UC8 — Cadastro de Profissional («extend») |
| **Atores** | Administrador (primário), Profissional de Saúde |
| **Descrição** | Desativa o cadastro de um profissional, retirando-o da busca e encerrando seus vínculos. |
| **Pré-condições** | Ator autenticado [UC1.3]; profissional com status diferente de **Inativo**. |
| **Pós-condições** | Status **Inativo**; vínculos encerrados [UC9.5]; usuários vinculados notificados. |
| **Dados** | Status, motivo, data de desativação, responsável. |

**Fluxo Principal**
1. O Administrador pesquisa o profissional e seleciona "Desativar".
2. O sistema exibe a quantidade de vínculos ativos e solicita o motivo.
3. O Administrador informa o motivo e confirma.
4. O sistema altera o status para **Inativo**.
5. O sistema encerra os vínculos ativos [UC9.5] e notifica os usuários afetados.
6. O sistema exibe a confirmação.

**Fluxos Alternativos**
- **FA01 — Desativação pelo próprio Profissional:** o Profissional solicita a desativação em "Meu perfil profissional", confirma com a senha e o sistema segue a partir do passo 4.

**Fluxos de Exceção**
- **FE01 — Senha incorreta (FA01):** o sistema não desativa o cadastro.

---

## 10. UC9 — Vínculo com Profissional

### UC9.1 — Buscar Profissional

| Item | Descrição |
|---|---|
| **Agrupador** | UC9 — Vínculo com Profissional («include») |
| **Atores** | Usuário |
| **Descrição** | Pesquisa profissionais de saúde validados para solicitar vínculo. |
| **Pré-condições** | Usuário autenticado [UC1.3]. |
| **Pós-condições** | Nenhuma alteração de dados; profissional selecionado pode receber solicitação [UC9.2]. |
| **Dados** | Filtros (nome, profissão, especialidade, UF), lista de profissionais, perfil público. |

**Fluxo Principal**
1. O Usuário acessa "Profissionais > Buscar".
2. O Usuário informa nome ou aplica filtros.
3. O sistema pesquisa apenas profissionais com status **Validado** (RN41).
4. O sistema exibe a lista com nome, foto, profissão, registro e UF.
5. O Usuário seleciona um profissional e o sistema exibe o perfil público.

**Fluxos Alternativos**
- **FA01 — Busca por código de convite:** no passo 2, o Usuário informa o código fornecido pelo profissional e o sistema exibe diretamente o perfil correspondente.

**Fluxos de Exceção**
- **FE01 — Nenhum resultado:** o sistema informa que não encontrou profissionais com os filtros aplicados.

**Regras de Negócio**
- **RN44:** O perfil público exibe apenas nome, foto, profissão, registro, UF, especialidades e descrição; contato pessoal não é exibido.

---

### UC9.2 — Solicitar Vínculo

| Item | Descrição |
|---|---|
| **Agrupador** | UC9 — Vínculo com Profissional («extend») |
| **Atores** | Usuário (primário), Profissional de Saúde (receptor) |
| **Descrição** | O Usuário solicita acompanhamento a um profissional, autorizando o compartilhamento de seus dados. |
| **Pré-condições** | Usuário autenticado [UC1.3]; profissional selecionado [UC9.1]. |
| **Pós-condições** | Vínculo com status **Solicitado**; autorização registrada [UC9.3]; Profissional notificado. |
| **Dados** | Usuário, profissional, data da solicitação, mensagem opcional, status do vínculo. |

**Fluxo Principal**
1. No perfil do profissional, o Usuário seleciona "Solicitar acompanhamento".
2. O sistema verifica se o Usuário pode solicitar o vínculo (RN45).
3. O sistema executa a autorização de compartilhamento de dados [UC9.3].
4. O Usuário escreve uma mensagem opcional e confirma.
5. O sistema grava o vínculo com status **Solicitado**.
6. O sistema notifica o Profissional.
7. O sistema informa ao Usuário que a solicitação foi enviada.

**Fluxos Alternativos**
- **FA01 — Cancelar solicitação:** enquanto o status for **Solicitado**, o Usuário pode cancelar a solicitação; o vínculo passa a **Cancelado**.

**Fluxos de Exceção**
- **FE01 — Vínculo existente:** no passo 2, se já houver vínculo ativo ou solicitação pendente com o mesmo profissional, o sistema informa e não cria nova solicitação.
- **FE02 — Autorização negada:** no passo 3, se o Usuário não autorizar o compartilhamento, a solicitação não é criada.

**Regras de Negócio**
- **RN45:** O Usuário pode ter no máximo 3 vínculos ativos ou pendentes ao mesmo tempo, e apenas um por profissional.
- **RN46:** Solicitações sem resposta em 15 dias expiram automaticamente.

---

### UC9.3 — Autorizar Compartilhamento de Dados

| Item | Descrição |
|---|---|
| **Agrupador** | UC9 — Vínculo com Profissional; incluído por [UC9.2] («include») |
| **Atores** | Usuário |
| **Descrição** | Registra o consentimento do Usuário para que o profissional acesse seus dados, conforme a LGPD. |
| **Pré-condições** | Usuário em processo de solicitação de vínculo [UC9.2]. |
| **Pós-condições** | Consentimento registrado com escopo, data e versão do termo. |
| **Dados** | Escopo autorizado (diário alimentar, fotos, hidratação, perfil e peso), versão do termo, data/hora, IP. |

**Fluxo Principal**
1. O sistema exibe o termo de compartilhamento, com os dados que o profissional poderá ver.
2. O sistema apresenta o escopo, com todos os itens marcados por padrão.
3. O Usuário revisa o escopo e aceita o termo.
4. O sistema registra o consentimento e retorna ao [UC9.2].

**Fluxos Alternativos**
- **FA01 — Escopo parcial:** no passo 3, o Usuário desmarca itens (ex.: fotos das refeições); o profissional não terá acesso a eles.
- **FA02 — Alterar escopo depois:** com o vínculo ativo, o Usuário pode alterar o escopo em "Meus profissionais"; o sistema registra um novo consentimento.

**Fluxos de Exceção**
- **FE01 — Termo recusado:** o sistema encerra a solicitação de vínculo.

**Regras de Negócio**
- **RN47:** O diário alimentar é o escopo mínimo obrigatório para que o vínculo exista.
- **RN48:** O consentimento pode ser revogado a qualquer momento, o que encerra o vínculo [UC9.5].

---

### UC9.4 — Aceitar Vínculo

| Item | Descrição |
|---|---|
| **Agrupador** | UC9 — Vínculo com Profissional («extend») |
| **Atores** | Profissional de Saúde (primário), Usuário (receptor) |
| **Descrição** | O Profissional aceita ou recusa uma solicitação de acompanhamento. |
| **Pré-condições** | Profissional autenticado [UC1.3] e com status **Validado**; solicitação com status **Solicitado**. |
| **Pós-condições** | Vínculo **Ativo** (ou **Recusado**); Usuário notificado; cliente disponível no [UC10.1]. |
| **Dados** | Vínculo, decisão, data de início, motivo da recusa. |

**Fluxo Principal**
1. O Profissional acessa "Solicitações".
2. O sistema lista as solicitações pendentes com nome do Usuário, mensagem e escopo autorizado.
3. O Profissional seleciona uma solicitação e escolhe "Aceitar".
4. O sistema altera o status para **Ativo** e registra a data de início.
5. O sistema notifica o Usuário.

**Fluxos Alternativos**
- **FA01 — Recusar:** no passo 3, o Profissional escolhe "Recusar" e pode informar o motivo; o sistema altera o status para **Recusado** e notifica o Usuário.

**Fluxos de Exceção**
- **FE01 — Solicitação expirada ou cancelada:** o sistema informa que a solicitação não está mais disponível (RN46).
- **FE02 — Limite de clientes atingido:** se o Profissional tiver atingido o limite da RN49, o sistema impede o aceite.

**Regras de Negócio**
- **RN49:** Cada profissional pode ter no máximo 100 clientes com vínculo ativo.

---

### UC9.5 — Encerrar Vínculo

| Item | Descrição |
|---|---|
| **Agrupador** | UC9 — Vínculo com Profissional («extend») |
| **Atores** | Usuário, Profissional de Saúde |
| **Descrição** | Encerra um vínculo ativo, revogando o acesso do profissional aos dados do Usuário. |
| **Pré-condições** | Ator autenticado [UC1.3]; vínculo com status **Ativo**. |
| **Pós-condições** | Vínculo **Encerrado**; acesso do profissional revogado imediatamente; outra parte notificada. |
| **Dados** | Vínculo, data de encerramento, quem encerrou, motivo. |

**Fluxo Principal**
1. O Usuário acessa "Meus profissionais" e seleciona o vínculo.
2. O Usuário escolhe "Encerrar acompanhamento".
3. O sistema informa que o profissional perderá acesso aos dados e pede confirmação.
4. O Usuário confirma e, opcionalmente, informa o motivo.
5. O sistema altera o status para **Encerrado**, revoga o acesso e notifica o Profissional.

**Fluxos Alternativos**
- **FA01 — Encerramento pelo Profissional:** o Profissional encerra o vínculo em "Meus clientes" [UC10.1]; o sistema notifica o Usuário.
- **FA02 — Encerramento automático:** o sistema encerra os vínculos quando o Usuário [UC1.4] ou o Profissional [UC8.4] é desativado, ou quando o consentimento é revogado (RN48).

**Fluxos de Exceção**
- **FE01 — Vínculo já encerrado:** o sistema informa que o vínculo não está mais ativo.

**Regras de Negócio**
- **RN50:** Após o encerramento, o profissional não acessa nenhum dado do Usuário, inclusive do período em que o vínculo esteve ativo.

---

## 11. UC10 — Acompanhamento de Clientes

### UC10.1 — Listar Clientes Vinculados

| Item | Descrição |
|---|---|
| **Agrupador** | UC10 — Acompanhamento de Clientes («include») |
| **Atores** | Profissional de Saúde |
| **Descrição** | Exibe a lista de usuários com vínculo ativo e um indicador de acompanhamento de cada um. |
| **Pré-condições** | Profissional autenticado [UC1.3] e com status **Validado**. |
| **Pós-condições** | Nenhuma alteração de dados. |
| **Dados** | Nome do cliente, data de início do vínculo, último registro, adesão à meta nos últimos 7 dias. |

**Fluxo Principal**
1. O Profissional acessa "Meus clientes".
2. O sistema obtém os vínculos com status **Ativo** do Profissional.
3. O sistema exibe a lista com nome, data de início do vínculo, data do último registro e adesão à meta nos últimos 7 dias [UC5.3].
4. O Profissional seleciona um cliente para ver o diário [UC10.2] ou a evolução [UC10.3].

**Fluxos Alternativos**
- **FA01 — Filtrar e ordenar:** o Profissional busca por nome ou ordena por último registro ou adesão.
- **FA02 — Clientes sem registro:** o sistema destaca os clientes sem registros há mais de 3 dias.

**Fluxos de Exceção**
- **FE01 — Nenhum cliente:** o sistema informa que não há clientes vinculados e exibe o código de convite do Profissional.

---

### UC10.2 — Consultar Diário do Cliente

| Item | Descrição |
|---|---|
| **Agrupador** | UC10 — Acompanhamento de Clientes («include») |
| **Atores** | Profissional de Saúde |
| **Descrição** | Permite ao Profissional ver o diário alimentar de um cliente vinculado, em modo somente leitura. |
| **Pré-condições** | Profissional autenticado [UC1.3]; vínculo **Ativo** com o cliente; diário incluído no escopo autorizado [UC9.3]. |
| **Pós-condições** | Nenhuma alteração nos dados do cliente; acesso registrado no log (RN51). |
| **Dados** | Data, refeições, itens, totais nutricionais, meta do cliente, fotos (se autorizadas). |

**Fluxo Principal**
1. O Profissional seleciona um cliente na lista [UC10.1] e escolhe "Diário".
2. O sistema verifica o vínculo e o escopo autorizado.
3. O sistema exibe o diário do dia atual, no mesmo formato do [UC4.4], em modo somente leitura.
4. O sistema registra o acesso no log.

**Fluxos Alternativos**
- **FA01 — Navegar entre dias:** o Profissional altera a data consultada.
- **FA02 — Fotos não autorizadas:** se o cliente não autorizou fotos, o sistema não as exibe e indica que não fazem parte do escopo.

**Fluxos de Exceção**
- **FE01 — Vínculo inativo ou acesso revogado:** o sistema nega o acesso e remove o cliente da lista.

**Regras de Negócio**
- **RN51:** Todo acesso de um profissional aos dados de um cliente é registrado (profissional, cliente, dado acessado, data/hora), e o Usuário pode consultar esse histórico.
- **RN52:** O profissional não pode criar, alterar ou excluir dados do cliente.

---

### UC10.3 — Consultar Evolução do Cliente

| Item | Descrição |
|---|---|
| **Agrupador** | UC10 — Acompanhamento de Clientes («include») |
| **Atores** | Profissional de Saúde |
| **Descrição** | Exibe a evolução do cliente em um período: consumo calórico, macronutrientes, adesão à meta, hidratação e peso. |
| **Pré-condições** | Profissional autenticado [UC1.3]; vínculo **Ativo** com o cliente. |
| **Pós-condições** | Nenhuma alteração nos dados do cliente; acesso registrado no log (RN51). |
| **Dados** | Período, médias de calorias e macronutrientes, adesão à meta, consumo de água, evolução do peso. |

**Fluxo Principal**
1. O Profissional seleciona um cliente na lista [UC10.1] e escolhe "Evolução".
2. O sistema verifica o vínculo e o escopo autorizado.
3. O sistema seleciona por padrão os últimos 30 dias, limitados à data de início do vínculo (RN53).
4. O sistema exibe o histórico de consumo [UC5.2], a comparação com a meta [UC5.3], a evolução da hidratação [UC6.4] e do peso, conforme o escopo autorizado.
5. O sistema registra o acesso no log.

**Fluxos Alternativos**
- **FA01 — Alterar período:** o Profissional escolhe outro período, respeitando a RN53.
- **FA02 — Exportar:** o Profissional exporta o relatório em PDF para uso na consulta.

**Fluxos de Exceção**
- **FE01 — Vínculo inativo ou acesso revogado:** o sistema nega o acesso.
- **FE02 — Período sem registros:** o sistema informa que não há dados no período.

**Regras de Negócio**
- **RN53:** O profissional pode ver dados a partir de 30 dias antes da data de início do vínculo, para ter um histórico de referência.
