# Especificação de Caso de Uso – NutriCount

## UC1 – Cadastro de Usuário

| Item | Descrição |
|---|---|
| Caso de Uso | UC1.1 – Incluir Usuário |
| Finalidade | Permitir a criação de uma nova conta de usuário no NutriCount, por autocadastro ou por inclusão feita pelo Administrador. |
| Atores | Usuário e Administrador. O Usuário se cadastra sozinho; o Administrador também pode incluir usuários. |
| Pré-condições | O e-mail informado ainda não está cadastrado no sistema. |
| Fluxo Principal | 1 - **Ator** seleciona a opção “Criar conta”.<br>2 - **Sistema** exibe o formulário de cadastro.<br>3 - **Ator** informa nome, e-mail, senha, confirmação de senha, data de nascimento e sexo, e aceita os Termos de Uso.<br>4 - **Ator** confirma o cadastro.<br>5 - **Sistema** valida os dados [RN01, RN02, RN03].<br>6 - **Sistema** grava o usuário com status Ativo e senha criptografada.<br>7 - **Sistema** exibe a mensagem “Cadastro realizado com sucesso” e direciona o usuário para o preenchimento do perfil [UC2.1]. |
| Fluxos Alternativos | FA1 – Inclusão pelo Administrador [passo 1]<br>&emsp;1 - **Ator** (Administrador autenticado [UC1.3]) acessa “Usuários > Novo” e informa os dados do usuário.<br>&emsp;2 - **Sistema** segue a partir do passo 5 e, no passo 6, gera uma senha provisória e a envia ao e-mail informado.<br>&emsp;3 - No primeiro acesso, o usuário deve trocar a senha [UC1.3].<br>FA2 – E-mail já cadastrado [passo 5]<br>&emsp;1 - **Sistema** informa “E-mail já cadastrado” e oferece a opção de recuperar a senha [UC1.3].<br>&emsp;2 - Retorna ao passo 3.<br>FA3 – Dados inválidos [passo 5]<br>&emsp;1 - **Sistema** destaca os campos inválidos.<br>&emsp;2 - Retorna ao passo 3.<br>FA4 – Termos de Uso não aceitos [passo 5]<br>&emsp;1 - **Sistema** impede o cadastro até que os Termos de Uso sejam aceitos.<br>&emsp;2 - Retorna ao passo 3.<br>FA5 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Conta criada com status Ativo; usuário apto a se autenticar [UC1.3] e a configurar seu perfil [UC2.1]. |
| Regras de negócio | RN01: O e-mail é único no sistema e deve ter formato válido.<br>RN02: A senha deve ter no mínimo 8 caracteres, com ao menos uma letra e um número.<br>RN03: O usuário deve ter no mínimo 16 anos. |
| Dados Manipulados | **Nome** do usuário com no máximo 60 caracteres, alfabético e obrigatório. **E-mail** com no máximo 100 caracteres, formato nome@dominio.com, único e obrigatório. **Senha** com 8 a 20 caracteres, ao menos uma letra e um número, obrigatória e gravada criptografada. **Confirmação de senha** igual à senha, obrigatória. **Data de nascimento** no formato DD/MM/AAAA, obrigatória, idade mínima de 16 anos. **Sexo** escolhido em lista (Feminino ou Masculino), obrigatório. **Aceite dos Termos de Uso** caixa de seleção obrigatória. **Status** gerado pelo sistema (Ativo ou Inativo). |

| Item | Descrição |
|---|---|
| Caso de Uso | UC1.2 – Alterar Usuário |
| Finalidade | Permitir a alteração dos dados cadastrais da conta (nome, e-mail e senha). |
| Atores | Usuário e Administrador. Ambos podem manter os dados da conta atualizados. |
| Pré-condições | Ator autenticado [UC1.3]; conta com status Ativo. |
| Fluxo Principal | 1 - **Ator** acessa a opção “Minha conta”.<br>2 - **Sistema** exibe os dados cadastrais atuais.<br>3 - **Ator** altera os campos desejados.<br>4 - **Ator** confirma a alteração.<br>5 - **Sistema** valida os dados [RN01, RN03].<br>6 - **Sistema** grava as alterações, registra no log de auditoria e exibe “Dados atualizados com sucesso”. |
| Fluxos Alternativos | FA1 – Alteração de senha [passo 3]<br>&emsp;1 - **Ator** informa a senha atual, a nova senha e a confirmação.<br>&emsp;2 - **Sistema** valida a senha atual e a regra de senha [RN02] antes de gravar.<br>&emsp;3 - Segue para o passo 4.<br>FA2 – Alteração de e-mail [passo 6]<br>&emsp;1 - **Sistema** envia um link de confirmação ao novo e-mail.<br>&emsp;2 - **Ator** confirma pelo link.<br>&emsp;3 - **Sistema** efetiva a troca do e-mail [RN04].<br>FA3 – Alteração pelo Administrador [passo 1]<br>&emsp;1 - **Ator** (Administrador) pesquisa o usuário em “Usuários” e o seleciona.<br>&emsp;2 - Segue a partir do passo 2. O Administrador não vê nem altera a senha, apenas força a redefinição.<br>FA4 – Senha atual incorreta [passo 2 do FA1]<br>&emsp;1 - **Sistema** informa o erro e mantém a senha anterior.<br>&emsp;2 - Retorna ao passo 3.<br>FA5 – Novo e-mail já cadastrado [passo 5]<br>&emsp;1 - **Sistema** informa “E-mail já cadastrado”.<br>&emsp;2 - Retorna ao passo 3.<br>FA6 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Dados cadastrais atualizados e alteração registrada no log de auditoria. |
| Regras de negócio | RN01: O e-mail é único no sistema e deve ter formato válido.<br>RN02: A senha deve ter no mínimo 8 caracteres, com ao menos uma letra e um número.<br>RN03: O usuário deve ter no mínimo 16 anos.<br>RN04: A troca de e-mail só é efetivada após a confirmação pelo link enviado ao novo endereço, que expira em 24 horas. |
| Dados Manipulados | **Nome** com no máximo 60 caracteres, alfabético e obrigatório. **E-mail** com no máximo 100 caracteres, formato nome@dominio.com, único e obrigatório. **Senha atual** obrigatória apenas na troca de senha. **Nova senha** com 8 a 20 caracteres, ao menos uma letra e um número. **Confirmação da nova senha** igual à nova senha. **Data de nascimento** DD/MM/AAAA, obrigatória, idade mínima de 16 anos. **Data de atualização** gerada pelo sistema (DD/MM/AAAA HH:MM). |

| Item | Descrição |
|---|---|
| Caso de Uso | UC1.3 – Autenticar Usuário |
| Finalidade | Validar as credenciais de acesso e iniciar a sessão do ator no sistema. |
| Atores | Usuário, Profissional de Saúde e Administrador. Todos precisam se autenticar para usar o sistema. |
| Pré-condições | Conta cadastrada com status Ativo. |
| Fluxo Principal | 1 - **Ator** acessa a tela de login.<br>2 - **Ator** informa e-mail e senha.<br>3 - **Sistema** valida as credenciais.<br>4 - **Sistema** identifica o perfil de acesso (Usuário, Profissional de Saúde ou Administrador) e gera o token de sessão [RN06].<br>5 - **Sistema** direciona o ator para a tela inicial do seu perfil. |
| Fluxos Alternativos | FA1 – Recuperar senha [passo 2]<br>&emsp;1 - **Ator** seleciona “Esqueci minha senha” e informa o e-mail.<br>&emsp;2 - **Sistema** envia um link de redefinição válido por 1 hora.<br>&emsp;3 - **Ator** define a nova senha [RN02] e retorna ao passo 1.<br>FA2 – Primeiro acesso com senha provisória [passo 4]<br>&emsp;1 - **Sistema** identifica que a senha é provisória e exige a troca.<br>&emsp;2 - **Ator** informa a nova senha [RN02].<br>&emsp;3 - Segue para o passo 5.<br>FA3 – Credenciais inválidas [passo 3]<br>&emsp;1 - **Sistema** exibe “E-mail ou senha incorretos”, sem indicar qual dos dois está errado.<br>&emsp;2 - Retorna ao passo 2.<br>FA4 – Conta bloqueada [passo 3]<br>&emsp;1 - **Sistema** bloqueia o acesso por 15 minutos após 5 tentativas malsucedidas [RN05] e informa o tempo restante.<br>&emsp;2 - Retorna ao passo 1 após o prazo.<br>FA5 – Conta inativa [passo 3]<br>&emsp;1 - **Sistema** informa que a conta está desativada e orienta o contato com o suporte.<br>&emsp;2 - Se estiver dentro do prazo da RN07, **Sistema** oferece a reativação [UC1.4].<br>FA6 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Sessão iniciada com o perfil de acesso do ator (Usuário, Profissional de Saúde ou Administrador). |
| Regras de negócio | RN02: A senha deve ter no mínimo 8 caracteres, com ao menos uma letra e um número.<br>RN05: Após 5 tentativas de login malsucedidas consecutivas, a conta fica bloqueada por 15 minutos.<br>RN06: A sessão expira após 30 dias sem uso no aplicativo móvel e após 2 horas de inatividade na versão web.<br>RN07: Uma conta desativada pode ser reativada em até 90 dias. Após esse prazo, os dados pessoais são anonimizados, em conformidade com a LGPD. |
| Dados Manipulados | **E-mail** com no máximo 100 caracteres, formato nome@dominio.com e obrigatório. **Senha** com 8 a 20 caracteres e obrigatória. **Token de sessão** gerado pelo sistema. **Data/hora do acesso** gerada (DD/MM/AAAA HH:MM). **Tentativas malsucedidas** número inteiro de 0 a 5, controlado pelo sistema. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC1.4 – Desativar Usuário |
| Finalidade | Desativar uma conta de usuário, impedindo novos acessos, sem excluir o histórico. |
| Atores | Usuário e Administrador. O Usuário pode desativar a própria conta e o Administrador pode desativar qualquer conta. |
| Pré-condições | Ator autenticado [UC1.3]; conta com status Ativo. |
| Fluxo Principal | 1 - **Ator** acessa “Minha conta > Desativar conta”.<br>2 - **Sistema** informa as consequências da desativação e solicita a senha e, opcionalmente, o motivo.<br>3 - **Ator** informa a senha e confirma.<br>4 - **Sistema** altera o status da conta para Inativo [RN08].<br>5 - **Sistema** encerra os vínculos ativos com profissionais [UC9.5] e desativa os lembretes [UC7.2].<br>6 - **Sistema** encerra a sessão e exibe a confirmação. |
| Fluxos Alternativos | FA1 – Senha incorreta [passo 3]<br>&emsp;1 - **Sistema** informa o erro e não desativa a conta.<br>&emsp;2 - Retorna ao passo 2.<br>FA2 – Desativação pelo Administrador [passo 1]<br>&emsp;1 - **Ator** (Administrador) seleciona o usuário em “Usuários” e escolhe “Desativar”.<br>&emsp;2 - **Ator** informa obrigatoriamente o motivo e confirma.<br>&emsp;3 - **Sistema** segue a partir do passo 4 e notifica o usuário por e-mail.<br>FA3 – Reativação da conta [após o passo 6]<br>&emsp;1 - **Ator** (Usuário) se autentica [UC1.3] dentro do prazo da RN07.<br>&emsp;2 - **Sistema** oferece a reativação da conta.<br>&emsp;3 - **Ator** confirma e o **Sistema** altera o status para Ativo.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Conta com status Inativo; vínculos com profissionais encerrados e lembretes desativados. |
| Regras de negócio | RN07: Uma conta desativada pode ser reativada em até 90 dias. Após esse prazo, os dados pessoais são anonimizados, em conformidade com a LGPD.<br>RN08: O sistema não faz exclusão física de usuários; usa exclusão lógica (status). |
| Dados Manipulados | **Senha** obrigatória, para confirmar a desativação. **Motivo** texto de até 200 caracteres, opcional para o Usuário e obrigatório para o Administrador. **Status** gerado pelo sistema (Ativo ou Inativo). **Data da desativação** gerada (DD/MM/AAAA HH:MM). **Responsável pela desativação** gerado pelo sistema (Usuário ou Administrador). |

## UC2 – Perfil e Meta Calórica

| Item | Descrição |
|---|---|
| Caso de Uso | UC2.1 – Alterar Perfil |
| Finalidade | Permitir informar ou atualizar os dados físicos e o objetivo do usuário, usados no cálculo nutricional. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]. |
| Fluxo Principal | 1 - **Ator** acessa “Meu perfil > Editar”.<br>2 - **Sistema** exibe os dados atuais do perfil [UC2.4].<br>3 - **Ator** altera peso, altura, nível de atividade e/ou objetivo.<br>4 - **Ator** confirma.<br>5 - **Sistema** valida os dados [RN09].<br>6 - **Sistema** grava o perfil e registra o novo peso no histórico com a data.<br>7 - **Sistema** recalcula a necessidade calórica [UC2.3] e exibe o novo valor. |
| Fluxos Alternativos | FA1 – Primeiro preenchimento [após o UC1.1]<br>&emsp;1 - **Sistema** exibe o perfil em formato de assistente (passo a passo).<br>&emsp;2 - **Ator** preenche os dados.<br>&emsp;3 - Ao final, o **Sistema** executa o [UC2.2].<br>FA2 – Meta personalizada existente [passo 7]<br>&emsp;1 - **Sistema** pergunta se o Usuário deseja manter a meta atual ou adotar o novo valor calculado.<br>&emsp;2 - **Ator** escolhe a opção e o **Sistema** grava.<br>FA3 – Valores fora da faixa [passo 5]<br>&emsp;1 - **Sistema** informa a faixa aceita [RN09].<br>&emsp;2 - Retorna ao passo 3.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Perfil atualizado; necessidade calórica recalculada [UC2.3]; peso registrado no histórico. |
| Regras de negócio | RN09: Faixas aceitas: peso entre 30 e 300 kg; altura entre 100 e 250 cm.<br>RN10: Níveis de atividade e fatores: sedentário (1,2), levemente ativo (1,375), moderadamente ativo (1,55), muito ativo (1,725), extremamente ativo (1,9). |
| Dados Manipulados | **Peso** em kg, numérico com 1 casa decimal, entre 30 e 300 e obrigatório. **Altura** em cm, número inteiro entre 100 e 250 e obrigatório. **Sexo** escolhido em lista (Feminino ou Masculino), obrigatório. **Data de nascimento** DD/MM/AAAA, obrigatória. **Nível de atividade física** escolhido em lista (sedentário, levemente ativo, moderadamente ativo, muito ativo, extremamente ativo), obrigatório. **Objetivo** escolhido em lista (perder, manter ou ganhar peso), obrigatório. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC2.2 – Definir Meta Calórica |
| Finalidade | Permitir definir a meta calórica diária, aceitando o valor sugerido pelo sistema ou informando um valor personalizado. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]; perfil preenchido [UC2.1]. |
| Fluxo Principal | 1 - **Ator** acessa “Meu perfil > Meta calórica”.<br>2 - **Sistema** calcula a necessidade calórica [UC2.3] e exibe o valor sugerido.<br>3 - **Ator** aceita o valor sugerido.<br>4 - **Sistema** grava a meta com origem Calculada e data de início igual à data atual [RN13].<br>5 - **Sistema** exibe a meta e a distribuição padrão de macronutrientes [RN12]. |
| Fluxos Alternativos | FA1 – Meta personalizada [passo 3]<br>&emsp;1 - **Ator** informa outro valor (por exemplo, orientado por um nutricionista).<br>&emsp;2 - **Sistema** valida o valor [RN11] e grava com origem Personalizada.<br>&emsp;3 - Segue para o passo 5.<br>FA2 – Ajuste de macronutrientes [passo 5]<br>&emsp;1 - **Ator** altera os percentuais de proteínas, carboidratos e gorduras.<br>&emsp;2 - **Sistema** valida que a soma é 100% e grava.<br>FA3 – Meta fora do limite seguro [passo 3 do FA1]<br>&emsp;1 - **Sistema** informa o limite [RN11] e não grava a meta.<br>&emsp;2 - Retorna ao passo 3.<br>FA4 – Perfil incompleto [passo 2]<br>&emsp;1 - **Sistema** direciona o Usuário para o [UC2.1].<br>FA5 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Meta calórica diária vigente registrada, com data de início; os relatórios passam a compará-la com o consumo [UC5.3]. |
| Regras de negócio | RN11: A meta calórica deve estar entre 1.200 e 5.000 kcal/dia.<br>RN12: Distribuição padrão de macronutrientes: 50% carboidratos, 20% proteínas e 30% gorduras.<br>RN13: Ao definir uma nova meta, a anterior é mantida no histórico com data de fim, para que relatórios de períodos passados usem a meta vigente na época. |
| Dados Manipulados | **Meta calórica** em kcal/dia, número inteiro entre 1.200 e 5.000 e obrigatório. **Origem** gerada pelo sistema (Calculada ou Personalizada). **Percentual de carboidratos, proteínas e gorduras** números inteiros de 0 a 100, cuja soma deve ser 100, obrigatórios (padrão 50, 20 e 30). **Data de início e data de fim da vigência** DD/MM/AAAA, geradas pelo sistema. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC2.3 – Calcular Necessidade Calórica |
| Finalidade | Calcular a Taxa Metabólica Basal (TMB), o Gasto Energético Total (GET) e a necessidade calórica de acordo com o objetivo do usuário. |
| Atores | Usuário. O cálculo é executado pelo Sistema quando o Usuário salva o perfil ou define a meta. |
| Pré-condições | Perfil com peso, altura, sexo, data de nascimento, nível de atividade e objetivo preenchidos. |
| Fluxo Principal | 1 - **Ator** aciona o cálculo ao salvar o perfil [UC2.1] ou ao definir a meta [UC2.2].<br>2 - **Sistema** obtém os dados do perfil do Usuário.<br>3 - **Sistema** calcula a idade a partir da data de nascimento.<br>4 - **Sistema** calcula a TMB pela fórmula de Mifflin-St Jeor [RN14].<br>5 - **Sistema** calcula o GET multiplicando a TMB pelo fator de atividade [RN10].<br>6 - **Sistema** aplica o ajuste do objetivo [RN15] e obtém a necessidade calórica.<br>7 - **Sistema** retorna os valores ao caso de uso que o acionou. |
| Fluxos Alternativos | FA1 – Resultado abaixo do mínimo [passo 6]<br>&emsp;1 - **Sistema** adota 1.200 kcal [RN11] e exibe um aviso recomendando acompanhamento profissional.<br>&emsp;2 - Segue para o passo 7.<br>FA2 – Dados insuficientes [passo 2]<br>&emsp;1 - **Sistema** interrompe o cálculo e solicita o preenchimento do perfil [UC2.1].<br>FA3 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** registra o erro no log de auditoria.<br>&emsp;2 - **Sistema** interrompe a execução e a repete na próxima oportunidade, sem perda de dados. |
| Pós-condições | Valores de TMB, GET e necessidade calórica disponíveis para o [UC2.2] e o [UC2.4]. |
| Regras de negócio | RN10: Níveis de atividade e fatores: sedentário (1,2), levemente ativo (1,375), moderadamente ativo (1,55), muito ativo (1,725), extremamente ativo (1,9).<br>RN11: A meta calórica deve estar entre 1.200 e 5.000 kcal/dia.<br>RN14: TMB (Mifflin-St Jeor): homens = 10 × peso + 6,25 × altura − 5 × idade + 5; mulheres = 10 × peso + 6,25 × altura − 5 × idade − 161.<br>RN15: Ajuste por objetivo: perder peso = GET − 500 kcal; manter = GET; ganhar peso = GET + 300 kcal. |
| Dados Manipulados | **Peso** em kg e **Altura** em cm, conforme o perfil. **Idade** em anos, número inteiro calculado pelo sistema. **Sexo** (Feminino ou Masculino). **Fator de atividade** numérico com 3 casas decimais, entre 1,2 e 1,9. **Objetivo** (perder, manter ou ganhar peso). **TMB, GET e necessidade calórica** em kcal, números inteiros calculados pelo sistema. Todos os dados são obrigatórios para o cálculo. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC2.4 – Consultar Perfil |
| Finalidade | Exibir os dados do perfil, os indicadores calculados e a meta calórica vigente. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]. |
| Fluxo Principal | 1 - **Ator** acessa “Meu perfil”.<br>2 - **Sistema** obtém os dados do perfil e a meta vigente.<br>3 - **Sistema** calcula o IMC [RN16].<br>4 - **Sistema** exibe dados físicos, objetivo, nível de atividade, IMC com classificação, TMB, GET e meta calórica. |
| Fluxos Alternativos | FA1 – Histórico de peso [passo 4]<br>&emsp;1 - **Ator** seleciona “Ver evolução”.<br>&emsp;2 - **Sistema** exibe o gráfico do peso ao longo do tempo.<br>FA2 – Perfil não preenchido [passo 2]<br>&emsp;1 - **Sistema** informa que o perfil está incompleto e oferece o [UC2.1].<br>FA3 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Nenhuma alteração de dados. |
| Regras de negócio | RN16: IMC = peso ÷ altura² (altura em metros), classificado conforme a tabela da OMS. |
| Dados Manipulados | **Peso** (kg), **altura** (cm), **sexo**, **data de nascimento** (DD/MM/AAAA), **nível de atividade** e **objetivo**, exibidos como cadastrados no [UC2.1]. **IMC** com 1 casa decimal, calculado e classificado pelo sistema. **TMB, GET e meta calórica** em kcal, números inteiros. **Histórico de peso** com data (DD/MM/AAAA) e peso (kg). Todos os campos são somente leitura. |

## UC3 – Catálogo de Alimentos

| Item | Descrição |
|---|---|
| Caso de Uso | UC3.1 – Incluir Alimento |
| Finalidade | Cadastrar manualmente um novo alimento com suas informações nutricionais. |
| Atores | Administrador e Usuário. O Administrador cadastra alimentos no catálogo geral e o Usuário cadastra alimentos privados. |
| Pré-condições | Ator autenticado [UC1.3]. |
| Fluxo Principal | 1 - **Ator** (Administrador) acessa “Catálogo > Novo alimento”.<br>2 - **Sistema** exibe o formulário.<br>3 - **Ator** informa nome, categoria, porção de referência e valores nutricionais.<br>4 - **Ator** confirma.<br>5 - **Sistema** valida os dados [RN17, RN18].<br>6 - **Sistema** grava o alimento com origem Manual, visibilidade Pública e status Ativo.<br>7 - **Sistema** exibe a confirmação. |
| Fluxos Alternativos | FA1 – Alimento personalizado pelo Usuário [passo 1]<br>&emsp;1 - **Ator** (Usuário) que não encontrou um alimento [UC3.3] seleciona “Cadastrar alimento”.<br>&emsp;2 - **Sistema** segue pelos passos 2 a 5 e, no passo 6, grava o alimento com visibilidade Privada [RN19].<br>FA2 – Alimento duplicado [passo 5]<br>&emsp;1 - **Sistema** informa que já existe alimento ativo com o mesmo nome e marca e sugere usar o existente [UC3.3].<br>FA3 – Valores inconsistentes [passo 5]<br>&emsp;1 - **Sistema** informa a inconsistência [RN18].<br>&emsp;2 - Retorna ao passo 3.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Alimento gravado com status Ativo e disponível para consulta [UC3.3]. |
| Regras de negócio | RN17: Nome, porção de referência e calorias são obrigatórios; os demais nutrientes são opcionais.<br>RN18: Nenhum valor nutricional pode ser negativo, e a soma de proteínas, carboidratos e gorduras não pode exceder a porção de referência em gramas.<br>RN19: Alimentos criados por Usuários são privados; somente o Administrador pode publicar alimentos no catálogo geral. |
| Dados Manipulados | **Nome** com no máximo 80 caracteres e obrigatório. **Marca** com no máximo 50 caracteres, opcional. **Categoria** escolhida em lista, obrigatória. **Porção de referência** numérica maior que zero, até 4 dígitos, obrigatória. **Unidade** escolhida em lista (g ou ml), obrigatória. **Calorias** em kcal, numérico com 1 casa decimal, maior ou igual a zero, obrigatório. **Proteínas, carboidratos, gorduras e fibras** em g, numéricos com 1 casa decimal, maiores ou iguais a zero, opcionais. **Sódio** em mg, numérico, opcional. **Origem** gerada pelo sistema (Manual ou Base Externa). **Visibilidade** gerada pelo sistema (Pública ou Privada). **Status** gerado pelo sistema (Ativo ou Inativo). |

| Item | Descrição |
|---|---|
| Caso de Uso | UC3.2 – Alterar Alimento |
| Finalidade | Alterar os dados de um alimento do catálogo. |
| Atores | Administrador e Usuário. |
| Pré-condições | Ator autenticado [UC1.3]; alimento cadastrado e com status Ativo. |
| Fluxo Principal | 1 - **Ator** (Administrador) pesquisa o alimento [UC3.3] e seleciona “Editar”.<br>2 - **Sistema** exibe os dados atuais.<br>3 - **Ator** altera os campos desejados.<br>4 - **Ator** confirma.<br>5 - **Sistema** valida os dados [RN17, RN18].<br>6 - **Sistema** grava as alterações e exibe a confirmação. |
| Fluxos Alternativos | FA1 – Alteração pelo Usuário [passo 1]<br>&emsp;1 - **Ator** (Usuário) seleciona “Editar” em um alimento privado criado por ele [RN19].<br>&emsp;2 - Segue para o passo 2.<br>FA2 – Alimento importado [passo 6]<br>&emsp;1 - **Sistema** avisa que a alteração deixa de acompanhar as atualizações da base externa.<br>&emsp;2 - **Ator** confirma.<br>&emsp;3 - **Sistema** muda a origem para Manual e grava.<br>FA3 – Sem permissão [passo 1]<br>&emsp;1 - **Sistema** nega a operação quando o Usuário tenta alterar um alimento público.<br>FA4 – Dados inválidos [passo 5]<br>&emsp;1 - **Sistema** informa o erro.<br>&emsp;2 - Retorna ao passo 3.<br>FA5 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Alimento atualizado; as refeições já registradas mantêm os valores da época [RN20]. |
| Regras de negócio | RN17: Nome, porção de referência e calorias são obrigatórios; os demais nutrientes são opcionais.<br>RN18: Nenhum valor nutricional pode ser negativo, e a soma de proteínas, carboidratos e gorduras não pode exceder a porção de referência em gramas.<br>RN19: Alimentos criados por Usuários são privados; somente o Administrador pode publicar alimentos no catálogo geral.<br>RN20: Cada refeição registrada guarda uma cópia dos valores nutricionais do alimento no momento do registro; alterações posteriores no catálogo não mudam o histórico. |
| Dados Manipulados | Mesmos dados do [UC3.1]: nome, marca, categoria, porção de referência, unidade, calorias, proteínas, carboidratos, gorduras, fibras e sódio, com os mesmos tipos, tamanhos e obrigatoriedade. **Data de atualização** gerada pelo sistema (DD/MM/AAAA HH:MM). **Responsável** gerado pelo sistema. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC3.3 – Consultar Alimento |
| Finalidade | Pesquisar alimentos no catálogo e exibir suas informações nutricionais. |
| Atores | Usuário e Administrador. |
| Pré-condições | Ator autenticado [UC1.3]. |
| Fluxo Principal | 1 - **Ator** acessa a busca de alimentos.<br>2 - **Ator** digita ao menos 3 caracteres do nome.<br>3 - **Sistema** pesquisa os alimentos ativos públicos e os privados do próprio Usuário [RN21].<br>4 - **Sistema** exibe a lista com nome, marca, porção de referência e calorias.<br>5 - **Ator** seleciona um alimento.<br>6 - **Sistema** exibe os detalhes nutricionais completos. |
| Fluxos Alternativos | FA1 – Filtros [passo 2]<br>&emsp;1 - **Ator** filtra por categoria ou origem.<br>&emsp;2 - Segue para o passo 3.<br>FA2 – Alimentos recentes e favoritos [passo 1]<br>&emsp;1 - **Sistema** exibe os alimentos usados recentemente pelo Usuário antes da digitação.<br>FA3 – Busca na base externa [passo 4]<br>&emsp;1 - **Sistema** oferece buscar na Base Nutricional Externa quando não há resultados.<br>&emsp;2 - **Ator** aceita e o **Sistema** segue para o [UC3.5].<br>FA4 – Visão do Administrador [passo 3]<br>&emsp;1 - **Sistema** inclui também os alimentos inativos na pesquisa.<br>FA5 – Nenhum resultado [passo 4]<br>&emsp;1 - **Sistema** informa que não encontrou alimentos e oferece incluir um alimento [UC3.1] ou importar [UC3.5].<br>FA6 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Nenhuma alteração de dados; o alimento selecionado pode ser usado em outro caso de uso. |
| Regras de negócio | RN21: A busca ignora acentos e maiúsculas/minúsculas e ordena os resultados por relevância e frequência de uso. |
| Dados Manipulados | **Termo de busca** com 3 a 80 caracteres e obrigatório. **Filtro de categoria** escolhido em lista, opcional. **Filtro de origem** escolhido em lista (Manual ou Base Externa), opcional. **Lista de alimentos** com nome, marca, porção de referência e calorias, somente leitura. **Detalhes nutricionais** (calorias, proteínas, carboidratos, gorduras, fibras e sódio), somente leitura. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC3.4 – Desativar Alimento |
| Finalidade | Retirar um alimento das buscas sem apagar o histórico de refeições que o utilizam. |
| Atores | Administrador e Usuário. |
| Pré-condições | Ator autenticado [UC1.3]; alimento com status Ativo. |
| Fluxo Principal | 1 - **Ator** (Administrador) pesquisa o alimento [UC3.3] e seleciona “Desativar”.<br>2 - **Sistema** solicita o motivo (por exemplo, duplicado ou dados incorretos).<br>3 - **Ator** informa o motivo e confirma.<br>4 - **Sistema** altera o status para Inativo e exibe a confirmação. |
| Fluxos Alternativos | FA1 – Reativação [passo 1]<br>&emsp;1 - **Ator** (Administrador) filtra os alimentos inativos e seleciona um.<br>&emsp;2 - **Ator** escolhe “Reativar” e o **Sistema** altera o status para Ativo.<br>FA2 – Desativação pelo Usuário [passo 1]<br>&emsp;1 - **Ator** (Usuário) desativa apenas os seus alimentos privados [RN19].<br>&emsp;2 - **Sistema** não solicita o motivo e segue para o passo 4.<br>FA3 – Sem permissão [passo 1]<br>&emsp;1 - **Sistema** nega a operação quando o Usuário tenta desativar um alimento público.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Alimento com status Inativo, fora das buscas; registros antigos preservados. |
| Regras de negócio | RN19: Alimentos criados por Usuários são privados; somente o Administrador pode publicar alimentos no catálogo geral.<br>RN22: Alimentos não são excluídos fisicamente, pois podem estar referenciados em refeições registradas. |
| Dados Manipulados | **Status** gerado pelo sistema (Ativo ou Inativo). **Motivo** escolhido em lista (duplicado, dados incorretos ou outro) com complemento de até 200 caracteres, obrigatório para o Administrador. **Data da desativação** gerada (DD/MM/AAAA HH:MM). **Responsável** gerado pelo sistema. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC3.5 – Importar Alimento da Base Externa |
| Finalidade | Buscar alimentos em uma base nutricional externa e importá-los para o catálogo. |
| Atores | Administrador, Usuário e Base Nutricional Externa. O Administrador e o Usuário solicitam a importação e a Base Nutricional Externa fornece os dados. |
| Pré-condições | Ator autenticado [UC1.3]; integração com a Base Nutricional Externa configurada. |
| Fluxo Principal | 1 - **Ator** (Administrador) acessa “Catálogo > Importar da base externa”.<br>2 - **Ator** informa o termo de busca.<br>3 - **Sistema** consulta a Base Nutricional Externa.<br>4 - **Base Nutricional Externa** retorna os alimentos encontrados.<br>5 - **Sistema** exibe os resultados, indicando os que já existem no catálogo.<br>6 - **Ator** seleciona um ou mais alimentos e confirma.<br>7 - **Sistema** converte os dados para o padrão do catálogo [RN23] e grava com origem Base Externa, visibilidade Pública e status Ativo [RN24].<br>8 - **Sistema** exibe o resumo da importação. |
| Fluxos Alternativos | FA1 – Importação pelo Usuário [passo 1]<br>&emsp;1 - **Ator** (Usuário) importa um alimento a partir do [UC3.3].<br>&emsp;2 - **Sistema** disponibiliza o alimento de imediato apenas para o Usuário e o envia para aprovação do Administrador antes de torná-lo público [RN19].<br>FA2 – Atualização de alimento já importado [passo 6]<br>&emsp;1 - **Sistema** oferece atualizar os valores do alimento existente em vez de duplicar [RN24].<br>&emsp;2 - **Ator** confirma a atualização.<br>FA3 – Base externa indisponível [passo 3]<br>&emsp;1 - **Sistema** informa a indisponibilidade e sugere o cadastro manual [UC3.1].<br>FA4 – Dados incompletos [passo 7]<br>&emsp;1 - **Sistema** não importa alimentos sem calorias ou sem porção de referência.<br>&emsp;2 - **Sistema** lista esses alimentos como rejeitados no resumo.<br>FA5 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Alimento gravado no catálogo com origem Base Externa e código de referência. |
| Regras de negócio | RN19: Alimentos criados por Usuários são privados; somente o Administrador pode publicar alimentos no catálogo geral.<br>RN23: Os valores importados são convertidos para a porção de referência de 100 g ou 100 ml.<br>RN24: O código de origem é guardado para impedir importações duplicadas do mesmo alimento. |
| Dados Manipulados | **Termo de busca** com 3 a 80 caracteres e obrigatório. **Código do alimento na base externa** texto de até 30 caracteres, gerado pela base, único. **Dados nutricionais** (calorias, proteínas, carboidratos, gorduras, fibras e sódio) numéricos, convertidos para 100 g ou 100 ml. **Data da importação** gerada pelo sistema (DD/MM/AAAA HH:MM). |

## UC4 – Diário Alimentar

| Item | Descrição |
|---|---|
| Caso de Uso | UC4.1 – Registrar Refeição |
| Finalidade | Registrar uma refeição no diário, com os alimentos e as quantidades consumidas. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]. |
| Fluxo Principal | 1 - **Ator** acessa o diário e seleciona “Adicionar refeição”.<br>2 - **Sistema** exibe o formulário com a data atual e sugere o tipo de refeição pelo horário [RN25].<br>3 - **Ator** confirma ou altera data, horário e tipo de refeição.<br>4 - **Ator** busca e seleciona um alimento [UC3.3].<br>5 - **Ator** informa a quantidade consumida.<br>6 - **Sistema** calcula os valores nutricionais do item [RN26].<br>7 - **Ator** repete os passos 4 a 6 para cada alimento.<br>8 - **Ator** confirma o registro.<br>9 - **Sistema** grava a refeição com uma cópia dos valores nutricionais [RN20].<br>10 - **Sistema** atualiza os totais do dia e exibe o saldo em relação à meta calórica. |
| Fluxos Alternativos | FA1 – Anexar foto [passo 8]<br>&emsp;1 - **Ator** seleciona “Adicionar foto”.<br>&emsp;2 - **Sistema** executa o [UC4.5] e retorna ao passo 8.<br>FA2 – Medidas caseiras [passo 5]<br>&emsp;1 - **Ator** escolhe uma medida caseira (colher, xícara ou unidade).<br>&emsp;2 - **Sistema** converte a medida para gramas e segue para o passo 6.<br>FA3 – Repetir refeição anterior [passo 4]<br>&emsp;1 - **Ator** seleciona uma refeição de outro dia.<br>&emsp;2 - **Sistema** copia os itens da refeição selecionada e segue para o passo 8.<br>FA4 – Registro retroativo [passo 3]<br>&emsp;1 - **Ator** escolhe uma data passada, dentro do limite da RN27.<br>&emsp;2 - Segue para o passo 4.<br>FA5 – Refeição sem itens [passo 8]<br>&emsp;1 - **Sistema** exige ao menos um alimento.<br>&emsp;2 - Retorna ao passo 4.<br>FA6 – Quantidade inválida [passo 5]<br>&emsp;1 - **Sistema** rejeita quantidades menores ou iguais a zero.<br>&emsp;2 - Retorna ao passo 5.<br>FA7 – Data futura [passo 3]<br>&emsp;1 - **Sistema** não aceita datas futuras.<br>&emsp;2 - Retorna ao passo 3.<br>FA8 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Refeição gravada no diário do dia; totais diários e saldo da meta atualizados. |
| Regras de negócio | RN20: Cada refeição registrada guarda uma cópia dos valores nutricionais do alimento no momento do registro; alterações posteriores no catálogo não mudam o histórico.<br>RN25: Tipos de refeição: café da manhã, lanche da manhã, almoço, lanche da tarde, jantar e ceia. A sugestão é feita pelos horários definidos em [UC7.1] ou, na falta deles, por faixas padrão.<br>RN26: Valor do item = (quantidade consumida ÷ porção de referência) × valor nutricional da porção.<br>RN27: Refeições e registros de água podem ser registrados, alterados ou excluídos com até 7 dias de retroatividade. |
| Dados Manipulados | **Data** DD/MM/AAAA, obrigatória, não futura e com no máximo 7 dias de retroatividade. **Horário** HH:MM, obrigatório. **Tipo de refeição** escolhido em lista (café da manhã, lanche da manhã, almoço, lanche da tarde, jantar ou ceia), obrigatório. **Itens** com ao menos um alimento, escolhido pelo [UC3.3], obrigatório. **Quantidade** numérica maior que zero, com até 2 casas decimais, obrigatória. **Unidade** escolhida em lista (g, ml ou medida caseira), obrigatória. **Valores nutricionais** calculados pelo sistema. **Observação** com até 200 caracteres, opcional. **Foto** opcional [UC4.5]. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC4.2 – Alterar Refeição |
| Finalidade | Alterar uma refeição já registrada (itens, quantidades, horário ou tipo). |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]; refeição registrada pelo próprio Usuário, dentro do prazo da RN27. |
| Fluxo Principal | 1 - **Ator** consulta o diário [UC4.4] e seleciona a refeição.<br>2 - **Ator** seleciona “Editar”.<br>3 - **Sistema** exibe os itens e os dados da refeição.<br>4 - **Ator** altera quantidades, horário, tipo ou observação.<br>5 - **Ator** confirma.<br>6 - **Sistema** recalcula os valores [RN26], grava e atualiza os totais do dia. |
| Fluxos Alternativos | FA1 – Incluir item [passo 4]<br>&emsp;1 - **Ator** adiciona outro alimento [UC3.3] e informa a quantidade.<br>FA2 – Remover item [passo 4]<br>&emsp;1 - **Ator** remove um alimento da refeição.<br>FA3 – Trocar foto [passo 4]<br>&emsp;1 - **Ator** substitui ou remove a foto, conforme o [UC4.5].<br>FA4 – Prazo expirado [passo 2]<br>&emsp;1 - **Sistema** informa que a refeição não pode mais ser alterada [RN27].<br>FA5 – Refeição sem itens [passo 5]<br>&emsp;1 - **Sistema** informa que todos os itens foram removidos e sugere excluir a refeição [UC4.3].<br>FA6 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Refeição atualizada; totais do dia recalculados. |
| Regras de negócio | RN26: Valor do item = (quantidade consumida ÷ porção de referência) × valor nutricional da porção.<br>RN27: Refeições e registros de água podem ser registrados, alterados ou excluídos com até 7 dias de retroatividade. |
| Dados Manipulados | Mesmos dados do [UC4.1]: data, horário, tipo de refeição, itens, quantidade, unidade, observação e foto, com os mesmos tipos, tamanhos e obrigatoriedade. **Data de atualização** gerada pelo sistema (DD/MM/AAAA HH:MM). |

| Item | Descrição |
|---|---|
| Caso de Uso | UC4.3 – Excluir Refeição |
| Finalidade | Remover uma refeição registrada no diário. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]; refeição do próprio Usuário, dentro do prazo da RN27. |
| Fluxo Principal | 1 - **Ator** consulta o diário [UC4.4] e seleciona a refeição.<br>2 - **Ator** seleciona “Excluir”.<br>3 - **Sistema** pede a confirmação.<br>4 - **Ator** confirma.<br>5 - **Sistema** remove a refeição e a foto associada [RN28], recalcula os totais e exibe a confirmação. |
| Fluxos Alternativos | FA1 – Desfazer [após o passo 5]<br>&emsp;1 - **Sistema** exibe a opção “Desfazer” por 10 segundos.<br>&emsp;2 - **Ator** seleciona “Desfazer” e o **Sistema** restaura a refeição.<br>FA2 – Prazo expirado [passo 2]<br>&emsp;1 - **Sistema** informa que a refeição não pode mais ser excluída [RN27].<br>FA3 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Refeição removida do diário; totais do dia recalculados. |
| Regras de negócio | RN27: Refeições e registros de água podem ser registrados, alterados ou excluídos com até 7 dias de retroatividade.<br>RN28: A exclusão de refeições é física, pois são dados do próprio Usuário; a foto anexada também é removida do armazenamento. |
| Dados Manipulados | **Identificador da refeição** gerado pelo sistema, somente leitura. **Confirmação da exclusão** obrigatória. **Data da exclusão** gerada pelo sistema (DD/MM/AAAA HH:MM). |

| Item | Descrição |
|---|---|
| Caso de Uso | UC4.4 – Consultar Diário |
| Finalidade | Exibir as refeições de um dia, agrupadas por tipo, com os totais nutricionais. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]. |
| Fluxo Principal | 1 - **Ator** acessa “Diário”.<br>2 - **Sistema** exibe o dia atual.<br>3 - **Sistema** lista as refeições agrupadas por tipo, com seus itens e calorias.<br>4 - **Sistema** exibe o total do dia, a meta calórica e o saldo (meta − consumido). |
| Fluxos Alternativos | FA1 – Navegar entre dias [passo 4]<br>&emsp;1 - **Ator** avança ou retrocede a data, ou escolhe uma data no calendário.<br>&emsp;2 - **Sistema** retorna ao passo 3 com a nova data.<br>FA2 – Detalhar refeição [passo 4]<br>&emsp;1 - **Ator** seleciona uma refeição.<br>&emsp;2 - **Sistema** exibe os macronutrientes de cada item e a foto, se houver.<br>FA3 – Dia sem registros [passo 3]<br>&emsp;1 - **Sistema** informa que não há refeições e oferece registrar uma [UC4.1].<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Nenhuma alteração de dados. |
| Regras de negócio | Não se aplica. |
| Dados Manipulados | **Data** DD/MM/AAAA, obrigatória e não futura. **Refeições e itens** (tipo, horário, alimento, quantidade e unidade), somente leitura. **Totais** de calorias (kcal) e macronutrientes (g) calculados pelo sistema. **Meta do dia** e **saldo** em kcal, calculados pelo sistema. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC4.5 – Anexar Foto da Refeição |
| Finalidade | Anexar uma foto à refeição, como registro visual do que foi consumido. |
| Atores | Usuário. |
| Pré-condições | Usuário registrando [UC4.1] ou alterando [UC4.2] uma refeição; permissão de câmera ou galeria concedida. |
| Fluxo Principal | 1 - **Ator** seleciona “Adicionar foto”.<br>2 - **Sistema** oferece as opções “Câmera” e “Galeria”.<br>3 - **Ator** tira ou escolhe a foto.<br>4 - **Sistema** valida o arquivo [RN29].<br>5 - **Sistema** comprime a imagem e exibe a pré-visualização.<br>6 - **Ator** confirma e o **Sistema** associa a foto à refeição. |
| Fluxos Alternativos | FA1 – Remover foto [passo 6]<br>&emsp;1 - **Ator** remove a foto na pré-visualização antes de salvar.<br>FA2 – Arquivo inválido [passo 4]<br>&emsp;1 - **Sistema** informa o formato ou o tamanho aceito [RN29].<br>&emsp;2 - Retorna ao passo 2.<br>FA3 – Permissão negada [passo 2]<br>&emsp;1 - **Sistema** orienta o Usuário a liberar o acesso à câmera ou à galeria nas configurações do aparelho.<br>FA4 – Falha no envio [passo 6]<br>&emsp;1 - **Sistema** salva a refeição sem a foto e permite reenviá-la depois.<br>FA5 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Foto armazenada e associada à refeição. |
| Regras de negócio | RN29: Formatos de foto aceitos: JPG, PNG e HEIC; tamanho máximo de 10 MB; uma foto por refeição. |
| Dados Manipulados | **Arquivo de imagem** nos formatos JPG, PNG ou HEIC, obrigatório. **Tamanho** de até 10 MB. **Quantidade** de uma foto por refeição. **Data de envio** gerada pelo sistema (DD/MM/AAAA HH:MM). |

## UC5 – Relatórios de Consumo

| Item | Descrição |
|---|---|
| Caso de Uso | UC5.1 – Consultar Resumo Diário |
| Finalidade | Exibir o resumo nutricional de um dia: calorias, macronutrientes e distribuição por refeição. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]. |
| Fluxo Principal | 1 - **Ator** acessa “Relatórios > Resumo do dia”.<br>2 - **Sistema** seleciona a data atual.<br>3 - **Sistema** soma os valores de todas as refeições do dia.<br>4 - **Sistema** exibe o total de calorias, o gráfico de macronutrientes (em gramas e percentual) e as calorias por refeição [RN30].<br>5 - **Sistema** exibe a comparação com a meta [UC5.3] e o consumo de água do dia. |
| Fluxos Alternativos | FA1 – Outra data [passo 2]<br>&emsp;1 - **Ator** escolhe outra data.<br>&emsp;2 - **Sistema** retorna ao passo 3.<br>FA2 – Sem registros [passo 3]<br>&emsp;1 - **Sistema** informa que não há dados para o dia selecionado.<br>FA3 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Nenhuma alteração de dados. |
| Regras de negócio | RN30: Calorias por grama: proteínas e carboidratos = 4 kcal/g; gorduras = 9 kcal/g. |
| Dados Manipulados | **Data** DD/MM/AAAA, obrigatória e não futura. **Total de calorias** em kcal. **Proteínas, carboidratos, gorduras e fibras** em g, com 1 casa decimal. **Calorias por refeição** em kcal. **Consumo de água** em ml. Todos os valores são calculados pelo sistema e somente leitura. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC5.2 – Consultar Histórico |
| Finalidade | Exibir a evolução do consumo em um período, com médias e gráficos. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]. |
| Fluxo Principal | 1 - **Ator** acessa “Relatórios > Histórico”.<br>2 - **Sistema** seleciona por padrão os últimos 7 dias.<br>3 - **Sistema** calcula as médias diárias considerando apenas os dias com registro [RN31].<br>4 - **Sistema** exibe o gráfico de calorias por dia com a linha da meta, as médias de macronutrientes e a quantidade de dias registrados. |
| Fluxos Alternativos | FA1 – Alterar período [passo 2]<br>&emsp;1 - **Ator** escolhe 30 dias, 90 dias ou um período personalizado [RN32].<br>&emsp;2 - **Sistema** retorna ao passo 3.<br>FA2 – Exportar [passo 4]<br>&emsp;1 - **Ator** exporta o relatório em PDF ou CSV.<br>FA3 – Período sem registros [passo 3]<br>&emsp;1 - **Sistema** informa que não há dados no período.<br>FA4 – Período inválido [passo 2 do FA1]<br>&emsp;1 - **Sistema** rejeita data final anterior à inicial ou período acima do limite [RN32].<br>&emsp;2 - Retorna ao passo 2 do FA1.<br>FA5 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Nenhuma alteração de dados. |
| Regras de negócio | RN31: Dias sem nenhum registro não entram no cálculo das médias.<br>RN32: O período personalizado pode ter no máximo 365 dias. |
| Dados Manipulados | **Período** com data inicial e data final no formato DD/MM/AAAA, obrigatórias, final maior ou igual à inicial e intervalo de no máximo 365 dias. **Calorias e macronutrientes por dia** em kcal e g. **Médias** calculadas pelo sistema. **Dias registrados** número inteiro. **Formato de exportação** escolhido em lista (PDF ou CSV), opcional. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC5.3 – Comparar Consumo com Meta |
| Finalidade | Comparar o consumo real com a meta calórica e de macronutrientes, indicando a adesão. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]; meta calórica definida [UC2.2]. |
| Fluxo Principal | 1 - **Sistema** obtém o consumo do dia ou do período consultado.<br>2 - **Sistema** obtém a meta vigente em cada data [RN13].<br>3 - **Sistema** calcula a diferença entre consumo e meta, em kcal e em percentual.<br>4 - **Sistema** classifica cada dia [RN33].<br>5 - **Sistema** exibe a comparação com indicadores visuais e, no caso de períodos, o percentual de dias dentro da meta. |
| Fluxos Alternativos | FA1 – Comparação de macronutrientes [passo 5]<br>&emsp;1 - **Ator** escolhe ver a comparação de proteínas, carboidratos e gorduras com a distribuição definida.<br>&emsp;2 - **Sistema** exibe a comparação.<br>FA2 – Meta não definida [passo 2]<br>&emsp;1 - **Sistema** exibe apenas o consumo e oferece definir a meta [UC2.2].<br>FA3 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Nenhuma alteração de dados. |
| Regras de negócio | RN13: Ao definir uma nova meta, a anterior é mantida no histórico com data de fim, para que relatórios de períodos passados usem a meta vigente na época.<br>RN33: Classificação: Dentro da meta (até 10% acima ou abaixo), Abaixo da meta (mais de 10% abaixo) e Acima da meta (mais de 10% acima). |
| Dados Manipulados | **Consumo** do dia ou período em kcal e g. **Meta vigente** em kcal/dia e percentuais de macronutrientes. **Diferença** em kcal (inteiro) e em percentual (1 casa decimal). **Situação** gerada pelo sistema (Dentro da meta, Abaixo da meta ou Acima da meta). Todos os campos são calculados e somente leitura. |

## UC6 – Controle de Hidratação

| Item | Descrição |
|---|---|
| Caso de Uso | UC6.1 – Definir Meta de Água |
| Finalidade | Definir a meta diária de consumo de água, sugerida pelo peso ou personalizada. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]. |
| Fluxo Principal | 1 - **Ator** acessa “Hidratação > Meta”.<br>2 - **Sistema** calcula e exibe a meta sugerida [RN34].<br>3 - **Ator** aceita a sugestão.<br>4 - **Sistema** grava a meta e exibe a confirmação. |
| Fluxos Alternativos | FA1 – Meta personalizada [passo 3]<br>&emsp;1 - **Ator** informa outro valor.<br>&emsp;2 - **Sistema** valida o valor [RN35] e segue para o passo 4.<br>FA2 – Peso não informado [passo 2]<br>&emsp;1 - **Sistema** não sugere meta e pede que o Usuário informe o valor manualmente ou complete o perfil [UC2.1].<br>FA3 – Valor fora da faixa [passo 3 do FA1]<br>&emsp;1 - **Sistema** informa a faixa aceita [RN35].<br>&emsp;2 - Retorna ao passo 3 do FA1.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Meta de água vigente gravada. |
| Regras de negócio | RN34: Meta de água sugerida = 35 ml × peso (kg), arredondada para múltiplos de 50 ml.<br>RN35: A meta de água deve estar entre 1.000 e 6.000 ml/dia. |
| Dados Manipulados | **Meta diária** em ml, número inteiro entre 1.000 e 6.000 e obrigatório. **Origem** gerada pelo sistema (Calculada ou Personalizada). **Data de vigência** DD/MM/AAAA, gerada pelo sistema. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC6.2 – Registrar Consumo de Água |
| Finalidade | Registrar uma quantidade de água ingerida. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]. |
| Fluxo Principal | 1 - **Ator** acessa “Hidratação”.<br>2 - **Sistema** exibe o total do dia, a meta e atalhos de quantidade (copo de 200 ml e garrafa de 500 ml).<br>3 - **Ator** seleciona um atalho.<br>4 - **Sistema** grava o registro com data e horário atuais.<br>5 - **Sistema** atualiza o total do dia e o percentual da meta. |
| Fluxos Alternativos | FA1 – Quantidade personalizada [passo 3]<br>&emsp;1 - **Ator** digita a quantidade em ml.<br>&emsp;2 - **Sistema** valida o valor [RN36] e segue para o passo 4.<br>FA2 – Meta atingida [passo 5]<br>&emsp;1 - **Sistema** exibe uma mensagem de parabéns ao atingir 100% da meta.<br>FA3 – Quantidade inválida [passo 3 do FA1]<br>&emsp;1 - **Sistema** rejeita valores menores ou iguais a zero ou acima de 2.000 ml por registro [RN36].<br>&emsp;2 - Retorna ao passo 3 do FA1.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Registro gravado; total do dia e percentual da meta atualizados. |
| Regras de negócio | RN36: Cada registro de água deve ter entre 1 e 2.000 ml. |
| Dados Manipulados | **Data** DD/MM/AAAA, obrigatória e não futura. **Horário** HH:MM, obrigatório. **Quantidade** em ml, número inteiro entre 1 e 2.000 e obrigatória. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC6.3 – Alterar Consumo de Água |
| Finalidade | Corrigir ou remover um registro de consumo de água. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]; registro existente, dentro do prazo da RN27. |
| Fluxo Principal | 1 - **Ator** acessa “Hidratação” e abre a lista de registros do dia.<br>2 - **Ator** seleciona um registro e escolhe “Editar”.<br>3 - **Ator** altera a quantidade ou o horário.<br>4 - **Ator** confirma.<br>5 - **Sistema** valida os dados [RN36], grava e recalcula o total do dia. |
| Fluxos Alternativos | FA1 – Remover registro [passo 2]<br>&emsp;1 - **Ator** escolhe “Remover” e confirma.<br>&emsp;2 - **Sistema** exclui o registro e recalcula o total do dia.<br>FA2 – Prazo expirado [passo 2]<br>&emsp;1 - **Sistema** informa que o registro não pode mais ser alterado [RN27].<br>FA3 – Quantidade inválida [passo 5]<br>&emsp;1 - **Sistema** informa o erro [RN36].<br>&emsp;2 - Retorna ao passo 3.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Registro alterado ou removido; total do dia recalculado. |
| Regras de negócio | RN27: Refeições e registros de água podem ser registrados, alterados ou excluídos com até 7 dias de retroatividade.<br>RN36: Cada registro de água deve ter entre 1 e 2.000 ml. |
| Dados Manipulados | **Identificador do registro** gerado pelo sistema, somente leitura. **Quantidade** em ml, número inteiro entre 1 e 2.000 e obrigatória. **Horário** HH:MM, obrigatório. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC6.4 – Consultar Evolução da Hidratação |
| Finalidade | Exibir a evolução do consumo de água em um período, comparada à meta. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]. |
| Fluxo Principal | 1 - **Ator** acessa “Hidratação > Evolução”.<br>2 - **Sistema** seleciona por padrão os últimos 7 dias.<br>3 - **Sistema** calcula o total por dia, a média do período e os dias com meta atingida.<br>4 - **Sistema** exibe o gráfico diário com a linha da meta, a média e a sequência atual de dias com meta atingida. |
| Fluxos Alternativos | FA1 – Alterar período [passo 2]<br>&emsp;1 - **Ator** escolhe 30 dias, 90 dias ou um período personalizado [RN32].<br>&emsp;2 - **Sistema** retorna ao passo 3.<br>FA2 – Sem registros [passo 3]<br>&emsp;1 - **Sistema** informa que não há registros no período.<br>FA3 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Nenhuma alteração de dados. |
| Regras de negócio | RN32: O período personalizado pode ter no máximo 365 dias. |
| Dados Manipulados | **Período** com data inicial e final no formato DD/MM/AAAA, obrigatórias e com no máximo 365 dias. **Total por dia** e **meta por dia** em ml. **Média** em ml, calculada pelo sistema. **Dias com meta atingida** e **sequência de dias** números inteiros calculados pelo sistema. |

## UC7 – Lembretes de Refeição

| Item | Descrição |
|---|---|
| Caso de Uso | UC7.1 – Definir Horários de Refeições |
| Finalidade | Definir os horários das refeições para os quais o Usuário quer receber lembretes. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]. |
| Fluxo Principal | 1 - **Ator** acessa “Lembretes”.<br>2 - **Sistema** exibe os tipos de refeição com horários sugeridos [RN25].<br>3 - **Ator** ativa os tipos desejados e define o horário de cada um.<br>4 - **Ator** escolhe os dias da semana.<br>5 - **Ator** confirma.<br>6 - **Sistema** valida os horários [RN37], grava e agenda os lembretes [UC7.3]. |
| Fluxos Alternativos | FA1 – Lembrete de água [passo 3]<br>&emsp;1 - **Ator** ativa também lembretes de hidratação em intervalos fixos (por exemplo, a cada 2 horas).<br>FA2 – Permissão de notificação [passo 6]<br>&emsp;1 - **Sistema** solicita a permissão de notificações quando o aparelho ainda não a concedeu.<br>FA3 – Horários conflitantes [passo 6]<br>&emsp;1 - **Sistema** informa que dois lembretes ficam a menos de 30 minutos um do outro [RN37].<br>&emsp;2 - Retorna ao passo 3.<br>FA4 – Notificações negadas [passo 6]<br>&emsp;1 - **Sistema** grava os horários e avisa que os lembretes não serão exibidos até a permissão ser concedida.<br>FA5 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Horários gravados e lembretes agendados [UC7.3]. |
| Regras de negócio | RN25: Tipos de refeição: café da manhã, lanche da manhã, almoço, lanche da tarde, jantar e ceia. A sugestão é feita pelos horários definidos em [UC7.1] ou, na falta deles, por faixas padrão.<br>RN37: O intervalo mínimo entre lembretes de refeição é de 30 minutos.<br>RN39: Lembretes respeitam o fuso horário configurado no aparelho do Usuário. |
| Dados Manipulados | **Tipo de refeição** escolhido em lista, obrigatório. **Horário** HH:MM, obrigatório. **Dias da semana** seleção múltipla, ao menos um dia, obrigatório. **Situação** (ativo ou inativo), obrigatória. **Antecedência do lembrete** em minutos, número inteiro de 0 a 60, opcional. **Intervalo do lembrete de água** em horas, número inteiro de 1 a 6, opcional. **Fuso horário** obtido do aparelho. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC7.2 – Alterar Horários de Refeições |
| Finalidade | Alterar, pausar ou desativar lembretes já definidos. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]; ao menos um horário definido [UC7.1]. |
| Fluxo Principal | 1 - **Ator** acessa “Lembretes”.<br>2 - **Sistema** exibe os lembretes cadastrados.<br>3 - **Ator** altera horário, dias da semana ou antecedência de um lembrete.<br>4 - **Ator** confirma.<br>5 - **Sistema** valida [RN37], grava, cancela o agendamento antigo e agenda o novo. |
| Fluxos Alternativos | FA1 – Desativar lembrete [passo 3]<br>&emsp;1 - **Ator** desativa um tipo de refeição.<br>&emsp;2 - **Sistema** cancela os agendamentos desse tipo.<br>FA2 – Pausar todos [passo 3]<br>&emsp;1 - **Ator** pausa todos os lembretes por um período (por exemplo, férias).<br>&emsp;2 - **Sistema** reativa os lembretes automaticamente ao fim do período.<br>FA3 – Horários conflitantes [passo 5]<br>&emsp;1 - **Sistema** informa o conflito [RN37].<br>&emsp;2 - Retorna ao passo 3.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Horários atualizados e lembretes reagendados. |
| Regras de negócio | RN37: O intervalo mínimo entre lembretes de refeição é de 30 minutos.<br>RN39: Lembretes respeitam o fuso horário configurado no aparelho do Usuário. |
| Dados Manipulados | Mesmos dados do [UC7.1]: tipo de refeição, horário, dias da semana, situação e antecedência, com os mesmos tipos e obrigatoriedade. **Período de pausa** com data inicial e final DD/MM/AAAA, opcional. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC7.3 – Enviar Lembrete de Refeição |
| Finalidade | Enviar automaticamente o lembrete no horário definido pelo Usuário. |
| Atores | Serviço de Notificação e Usuário. O Serviço de Notificação entrega a mensagem e o Usuário a recebe. |
| Pré-condições | Lembrete ativo [UC7.1]; conta do Usuário ativa. |
| Fluxo Principal | 1 - **Sistema** identifica, a cada minuto, os lembretes cujo horário foi atingido, considerando o fuso horário do Usuário [RN39].<br>2 - **Sistema** verifica se a refeição daquele tipo já foi registrada no dia [RN38].<br>3 - **Sistema** monta a mensagem (por exemplo, “Hora do almoço! Não esqueça de registrar sua refeição”).<br>4 - **Sistema** envia a mensagem ao **Serviço de Notificação**.<br>5 - **Serviço de Notificação** entrega a notificação no aparelho do **Usuário**.<br>6 - **Sistema** registra o envio. |
| Fluxos Alternativos | FA1 – Refeição já registrada [passo 2]<br>&emsp;1 - **Sistema** não envia o lembrete [RN38].<br>FA2 – Abrir pela notificação [após o passo 5]<br>&emsp;1 - **Ator** (Usuário) toca na notificação.<br>&emsp;2 - **Sistema** abre o registro de refeição [UC4.1] com o tipo já preenchido.<br>FA3 – Falha no envio [passo 5]<br>&emsp;1 - **Sistema** tenta novamente até 3 vezes, com intervalo de 1 minuto, quando o Serviço de Notificação não confirma a entrega.<br>&emsp;2 - **Sistema** registra a falha.<br>FA4 – Token do aparelho inválido [passo 5]<br>&emsp;1 - **Sistema** desativa o token.<br>&emsp;2 - **Sistema** aguarda o próximo login do Usuário para registrar um novo token.<br>FA5 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** registra o erro no log de auditoria.<br>&emsp;2 - **Sistema** interrompe a execução e a repete na próxima oportunidade, sem perda de dados. |
| Pós-condições | Lembrete enviado e registrado no log de envios. |
| Regras de negócio | RN38: O lembrete não é enviado se a refeição daquele tipo já tiver sido registrada no dia.<br>RN39: Lembretes respeitam o fuso horário configurado no aparelho do Usuário. |
| Dados Manipulados | **Lembrete** (tipo de refeição e horário). **Mensagem** texto de até 120 caracteres, gerado pelo sistema. **Data/hora do envio** DD/MM/AAAA HH:MM, gerada pelo sistema. **Situação do envio** gerada pelo sistema (enviado, falha ou cancelado). **Token do aparelho** texto gerado pelo aplicativo. |

## UC8 – Cadastro de Profissional

| Item | Descrição |
|---|---|
| Caso de Uso | UC8.1 – Incluir Profissional |
| Finalidade | Cadastrar um profissional de saúde que poderá acompanhar usuários. |
| Atores | Profissional de Saúde e Administrador. O Profissional se cadastra sozinho; o Administrador também pode incluí-lo. |
| Pré-condições | O e-mail e o registro profissional ainda não estão cadastrados no sistema. |
| Fluxo Principal | 1 - **Ator** acessa “Sou profissional de saúde > Cadastrar”.<br>2 - **Sistema** exibe o formulário.<br>3 - **Ator** informa dados pessoais, de acesso e profissionais e aceita os Termos de Uso.<br>4 - **Ator** confirma.<br>5 - **Sistema** valida os dados [RN01, RN02, RN40].<br>6 - **Sistema** grava o profissional com status Pendente de validação.<br>7 - **Sistema** aciona a validação do registro profissional [UC8.3].<br>8 - **Sistema** informa que o perfil ficará visível aos usuários após a validação [RN41]. |
| Fluxos Alternativos | FA1 – Inclusão pelo Administrador [passo 1]<br>&emsp;1 - **Ator** (Administrador) cadastra o profissional e informa os dados.<br>&emsp;2 - **Sistema** envia uma senha provisória ao e-mail informado e segue para o passo 7.<br>FA2 – E-mail ou registro já cadastrado [passo 5]<br>&emsp;1 - **Sistema** informa a duplicidade e não grava.<br>FA3 – Dados inválidos [passo 5]<br>&emsp;1 - **Sistema** destaca os campos inválidos.<br>&emsp;2 - Retorna ao passo 3.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Profissional gravado com status Pendente de validação; registro enviado para validação [UC8.3]. |
| Regras de negócio | RN01: O e-mail é único no sistema e deve ter formato válido.<br>RN02: A senha deve ter no mínimo 8 caracteres, com ao menos uma letra e um número.<br>RN40: A combinação conselho + número de registro + UF é única no sistema; o CPF também é único.<br>RN41: Somente profissionais com status Validado aparecem na busca [UC9.1] e podem ter vínculos. |
| Dados Manipulados | **Nome** com no máximo 60 caracteres, alfabético e obrigatório. **E-mail** com no máximo 100 caracteres, formato nome@dominio.com, único e obrigatório. **Senha** com 8 a 20 caracteres, ao menos uma letra e um número, obrigatória. **CPF** com 11 dígitos no padrão 999.999.999-99, único e obrigatório. **Profissão** escolhida em lista, obrigatória. **Conselho profissional** escolhido em lista (CRN, CRM, CREF etc.), obrigatório. **Número de registro** numérico de até 10 dígitos, obrigatório. **UF** escolhida em lista, obrigatória. **Especialidades** texto de até 100 caracteres, opcional. **Foto** JPG ou PNG de até 5 MB, opcional. **Descrição** de até 500 caracteres, opcional. **Status** gerado pelo sistema (Pendente de validação, Validado, Rejeitado ou Inativo). |

| Item | Descrição |
|---|---|
| Caso de Uso | UC8.2 – Alterar Profissional |
| Finalidade | Alterar os dados do cadastro profissional. |
| Atores | Profissional de Saúde e Administrador. |
| Pré-condições | Profissional autenticado [UC1.3]; cadastro com status diferente de Inativo. |
| Fluxo Principal | 1 - **Ator** acessa “Meu perfil profissional > Editar”.<br>2 - **Sistema** exibe os dados atuais.<br>3 - **Ator** altera foto, descrição, especialidades ou contato.<br>4 - **Ator** confirma.<br>5 - **Sistema** valida e grava as alterações. |
| Fluxos Alternativos | FA1 – Alteração do registro profissional [passo 3]<br>&emsp;1 - **Ator** altera conselho, número de registro ou UF.<br>&emsp;2 - **Sistema** muda o status para Pendente de validação e aciona o [UC8.3]. Os vínculos ativos são mantidos.<br>FA2 – Alteração pelo Administrador [passo 1]<br>&emsp;1 - **Ator** (Administrador) pesquisa o profissional e o seleciona.<br>&emsp;2 - Segue a partir do passo 2.<br>FA3 – Registro duplicado [passo 5]<br>&emsp;1 - **Sistema** informa que o registro já pertence a outro cadastro [RN40].<br>&emsp;2 - Retorna ao passo 3.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Dados atualizados; se o registro profissional mudou, nova validação pendente. |
| Regras de negócio | RN40: A combinação conselho + número de registro + UF é única no sistema; o CPF também é único. |
| Dados Manipulados | Mesmos dados do [UC8.1]: nome, e-mail, CPF, profissão, conselho, número de registro, UF, especialidades, foto e descrição, com os mesmos tipos, tamanhos e obrigatoriedade. **Data de atualização** gerada pelo sistema (DD/MM/AAAA HH:MM). |

| Item | Descrição |
|---|---|
| Caso de Uso | UC8.3 – Validar Registro Profissional |
| Finalidade | Verificar se o registro no conselho profissional é válido e ativo, aprovando ou rejeitando o cadastro. |
| Atores | Administrador e Profissional de Saúde. O Administrador decide e o Profissional recebe o resultado. |
| Pré-condições | Profissional com status Pendente de validação; Administrador autenticado [UC1.3]. |
| Fluxo Principal | 1 - **Ator** (Administrador) acessa “Profissionais > Pendentes de validação”.<br>2 - **Sistema** lista os cadastros pendentes por ordem de chegada.<br>3 - **Ator** seleciona um cadastro.<br>4 - **Sistema** exibe os dados profissionais e o documento enviado.<br>5 - **Ator** consulta o registro no site do conselho correspondente.<br>6 - **Ator** aprova o cadastro.<br>7 - **Sistema** altera o status para Validado, registra a decisão e notifica o Profissional por e-mail [RN42]. |
| Fluxos Alternativos | FA1 – Rejeição [passo 6]<br>&emsp;1 - **Ator** rejeita o cadastro e informa o motivo.<br>&emsp;2 - **Sistema** altera o status para Rejeitado e notifica o Profissional, que pode corrigir os dados [UC8.2] e reenviar.<br>FA2 – Solicitar documento [passo 6]<br>&emsp;1 - **Ator** pede um documento complementar.<br>&emsp;2 - **Sistema** mantém o cadastro como pendente e notifica o Profissional.<br>FA3 – Registro inativo ou inexistente no conselho [passo 5]<br>&emsp;1 - **Ator** rejeita o cadastro conforme o FA1.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Status alterado para Validado ou Rejeitado; Profissional notificado. |
| Regras de negócio | RN42: A validação deve ser concluída em até 3 dias úteis após o cadastro.<br>RN43: A validação é revista anualmente; se o registro deixar de estar ativo, o profissional é desativado [UC8.4]. |
| Dados Manipulados | **Conselho**, **número de registro** e **UF** conforme o [UC8.1]. **Situação no conselho** escolhida em lista (ativo ou inativo), obrigatória. **Documento comprobatório** PDF, JPG ou PNG de até 10 MB. **Decisão** escolhida em lista (aprovar, rejeitar ou solicitar documento), obrigatória. **Motivo** de até 200 caracteres, obrigatório na rejeição. **Data e responsável** gerados pelo sistema. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC8.4 – Desativar Profissional |
| Finalidade | Desativar o cadastro de um profissional, retirando-o da busca e encerrando seus vínculos. |
| Atores | Administrador e Profissional de Saúde. |
| Pré-condições | Ator autenticado [UC1.3]; profissional com status diferente de Inativo. |
| Fluxo Principal | 1 - **Ator** (Administrador) pesquisa o profissional e seleciona “Desativar”.<br>2 - **Sistema** exibe a quantidade de vínculos ativos e solicita o motivo.<br>3 - **Ator** informa o motivo e confirma.<br>4 - **Sistema** altera o status para Inativo.<br>5 - **Sistema** encerra os vínculos ativos [UC9.5] e notifica os usuários afetados.<br>6 - **Sistema** exibe a confirmação. |
| Fluxos Alternativos | FA1 – Desativação pelo próprio Profissional [passo 1]<br>&emsp;1 - **Ator** (Profissional) solicita a desativação em “Meu perfil profissional” e confirma com a senha.<br>&emsp;2 - **Sistema** segue a partir do passo 4.<br>FA2 – Senha incorreta [passo 1 do FA1]<br>&emsp;1 - **Sistema** não desativa o cadastro.<br>&emsp;2 - Retorna ao passo 1 do FA1.<br>FA3 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Status Inativo; vínculos encerrados [UC9.5]; usuários vinculados notificados. |
| Regras de negócio | RN43: A validação é revista anualmente; se o registro deixar de estar ativo, o profissional é desativado [UC8.4]. |
| Dados Manipulados | **Motivo** de até 200 caracteres, obrigatório. **Senha** obrigatória na desativação pelo próprio Profissional. **Status** gerado pelo sistema. **Data da desativação** gerada (DD/MM/AAAA HH:MM). **Responsável** gerado pelo sistema. |

## UC9 – Vínculo com Profissional

| Item | Descrição |
|---|---|
| Caso de Uso | UC9.1 – Buscar Profissional |
| Finalidade | Pesquisar profissionais de saúde validados para solicitar vínculo. |
| Atores | Usuário. |
| Pré-condições | Usuário autenticado [UC1.3]. |
| Fluxo Principal | 1 - **Ator** acessa “Profissionais > Buscar”.<br>2 - **Ator** informa nome ou aplica filtros.<br>3 - **Sistema** pesquisa apenas profissionais com status Validado [RN41].<br>4 - **Sistema** exibe a lista com nome, foto, profissão, registro e UF.<br>5 - **Ator** seleciona um profissional e o **Sistema** exibe o perfil público [RN44]. |
| Fluxos Alternativos | FA1 – Busca por código de convite [passo 2]<br>&emsp;1 - **Ator** informa o código fornecido pelo profissional.<br>&emsp;2 - **Sistema** exibe diretamente o perfil correspondente.<br>FA2 – Nenhum resultado [passo 4]<br>&emsp;1 - **Sistema** informa que não encontrou profissionais com os filtros aplicados.<br>FA3 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Nenhuma alteração de dados; profissional selecionado pode receber solicitação [UC9.2]. |
| Regras de negócio | RN41: Somente profissionais com status Validado aparecem na busca [UC9.1] e podem ter vínculos.<br>RN44: O perfil público exibe apenas nome, foto, profissão, registro, UF, especialidades e descrição; contato pessoal não é exibido. |
| Dados Manipulados | **Nome** com no máximo 60 caracteres, opcional. **Profissão**, **especialidade** e **UF** escolhidas em lista, opcionais. **Código de convite** alfanumérico de 8 caracteres, opcional. **Perfil público** (nome, foto, profissão, registro, UF, especialidades e descrição), somente leitura. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC9.2 – Solicitar Vínculo |
| Finalidade | Permitir ao Usuário solicitar acompanhamento a um profissional, autorizando o compartilhamento de seus dados. |
| Atores | Usuário e Profissional de Saúde. O Usuário faz a solicitação e o Profissional é notificado. |
| Pré-condições | Usuário autenticado [UC1.3]; profissional selecionado [UC9.1]. |
| Fluxo Principal | 1 - **Ator** seleciona “Solicitar acompanhamento” no perfil do profissional.<br>2 - **Sistema** verifica se o Usuário pode solicitar o vínculo [RN45].<br>3 - **Sistema** executa a autorização de compartilhamento de dados [UC9.3].<br>4 - **Ator** escreve uma mensagem opcional e confirma.<br>5 - **Sistema** grava o vínculo com status Solicitado.<br>6 - **Sistema** notifica o Profissional.<br>7 - **Sistema** informa ao Usuário que a solicitação foi enviada. |
| Fluxos Alternativos | FA1 – Cancelar solicitação [após o passo 7]<br>&emsp;1 - **Ator** cancela a solicitação enquanto o status for Solicitado.<br>&emsp;2 - **Sistema** altera o vínculo para Cancelado.<br>FA2 – Vínculo existente [passo 2]<br>&emsp;1 - **Sistema** informa que já há vínculo ativo ou solicitação pendente com o profissional e não cria nova solicitação [RN45].<br>FA3 – Autorização negada [passo 3]<br>&emsp;1 - **Sistema** não cria a solicitação.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Vínculo com status Solicitado; autorização registrada [UC9.3]; Profissional notificado. |
| Regras de negócio | RN45: O Usuário pode ter no máximo 3 vínculos ativos ou pendentes ao mesmo tempo, e apenas um por profissional.<br>RN46: Solicitações sem resposta em 15 dias expiram automaticamente. |
| Dados Manipulados | **Usuário** e **profissional** identificados pelo sistema. **Mensagem** de até 300 caracteres, opcional. **Data da solicitação** gerada (DD/MM/AAAA HH:MM). **Status do vínculo** gerado pelo sistema (Solicitado, Ativo, Recusado, Cancelado, Expirado ou Encerrado). |

| Item | Descrição |
|---|---|
| Caso de Uso | UC9.3 – Autorizar Compartilhamento de Dados |
| Finalidade | Registrar o consentimento do Usuário para que o profissional acesse seus dados, conforme a LGPD. |
| Atores | Usuário. |
| Pré-condições | Usuário em processo de solicitação de vínculo [UC9.2]. |
| Fluxo Principal | 1 - **Sistema** exibe o termo de compartilhamento, com os dados que o profissional poderá ver.<br>2 - **Sistema** apresenta o escopo, com todos os itens marcados por padrão.<br>3 - **Ator** revisa o escopo e aceita o termo.<br>4 - **Sistema** registra o consentimento e retorna ao [UC9.2]. |
| Fluxos Alternativos | FA1 – Escopo parcial [passo 3]<br>&emsp;1 - **Ator** desmarca itens (por exemplo, fotos das refeições).<br>&emsp;2 - **Sistema** impede o acesso do profissional aos itens desmarcados.<br>FA2 – Alterar escopo depois [fora do fluxo principal]<br>&emsp;1 - **Ator** altera o escopo em “Meus profissionais” com o vínculo ativo.<br>&emsp;2 - **Sistema** registra um novo consentimento.<br>FA3 – Termo recusado [passo 3]<br>&emsp;1 - **Sistema** encerra a solicitação de vínculo.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Consentimento registrado com escopo, data e versão do termo. |
| Regras de negócio | RN47: O diário alimentar é o escopo mínimo obrigatório para que o vínculo exista.<br>RN48: O consentimento pode ser revogado a qualquer momento, o que encerra o vínculo [UC9.5]. |
| Dados Manipulados | **Escopo autorizado** seleção múltipla (diário alimentar, fotos, hidratação, perfil e peso), com o diário alimentar obrigatório. **Aceite do termo** caixa de seleção obrigatória. **Versão do termo** gerada pelo sistema. **Data/hora** (DD/MM/AAAA HH:MM:SS) e **IP** gerados pelo sistema. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC9.4 – Aceitar Vínculo |
| Finalidade | Permitir ao Profissional aceitar ou recusar uma solicitação de acompanhamento. |
| Atores | Profissional de Saúde e Usuário. O Profissional decide e o Usuário é notificado. |
| Pré-condições | Profissional autenticado [UC1.3] e com status Validado; solicitação com status Solicitado. |
| Fluxo Principal | 1 - **Ator** acessa “Solicitações”.<br>2 - **Sistema** lista as solicitações pendentes com nome do Usuário, mensagem e escopo autorizado.<br>3 - **Ator** seleciona uma solicitação e escolhe “Aceitar”.<br>4 - **Sistema** altera o status para Ativo e registra a data de início [RN49].<br>5 - **Sistema** notifica o Usuário. |
| Fluxos Alternativos | FA1 – Recusar [passo 3]<br>&emsp;1 - **Ator** escolhe “Recusar” e pode informar o motivo.<br>&emsp;2 - **Sistema** altera o status para Recusado e notifica o Usuário.<br>FA2 – Solicitação expirada ou cancelada [passo 3]<br>&emsp;1 - **Sistema** informa que a solicitação não está mais disponível [RN46].<br>FA3 – Limite de clientes atingido [passo 3]<br>&emsp;1 - **Sistema** impede o aceite quando o Profissional já tem 100 clientes ativos [RN49].<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Vínculo Ativo (ou Recusado); Usuário notificado; cliente disponível no [UC10.1]. |
| Regras de negócio | RN46: Solicitações sem resposta em 15 dias expiram automaticamente.<br>RN49: Cada profissional pode ter no máximo 100 clientes com vínculo ativo. |
| Dados Manipulados | **Vínculo** identificado pelo sistema. **Decisão** escolhida em lista (aceitar ou recusar), obrigatória. **Data de início** gerada (DD/MM/AAAA). **Motivo da recusa** de até 200 caracteres, opcional. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC9.5 – Encerrar Vínculo |
| Finalidade | Encerrar um vínculo ativo, revogando o acesso do profissional aos dados do Usuário. |
| Atores | Usuário e Profissional de Saúde. |
| Pré-condições | Ator autenticado [UC1.3]; vínculo com status Ativo. |
| Fluxo Principal | 1 - **Ator** acessa “Meus profissionais” e seleciona o vínculo.<br>2 - **Ator** escolhe “Encerrar acompanhamento”.<br>3 - **Sistema** informa que o profissional perderá acesso aos dados e pede confirmação.<br>4 - **Ator** confirma e, opcionalmente, informa o motivo.<br>5 - **Sistema** altera o status para Encerrado, revoga o acesso [RN50] e notifica o Profissional. |
| Fluxos Alternativos | FA1 – Encerramento pelo Profissional [passo 1]<br>&emsp;1 - **Ator** (Profissional) encerra o vínculo em “Meus clientes” [UC10.1].<br>&emsp;2 - **Sistema** notifica o Usuário.<br>FA2 – Encerramento automático [fora do fluxo principal]<br>&emsp;1 - **Sistema** encerra os vínculos quando o Usuário [UC1.4] ou o Profissional [UC8.4] é desativado ou quando o consentimento é revogado [RN48].<br>FA3 – Vínculo já encerrado [passo 2]<br>&emsp;1 - **Sistema** informa que o vínculo não está mais ativo.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Vínculo Encerrado; acesso do profissional revogado imediatamente; outra parte notificada. |
| Regras de negócio | RN48: O consentimento pode ser revogado a qualquer momento, o que encerra o vínculo [UC9.5].<br>RN50: Após o encerramento, o profissional não acessa nenhum dado do Usuário, inclusive do período em que o vínculo esteve ativo. |
| Dados Manipulados | **Vínculo** identificado pelo sistema. **Motivo** de até 200 caracteres, opcional. **Data de encerramento** gerada (DD/MM/AAAA HH:MM). **Quem encerrou** gerado pelo sistema (Usuário, Profissional ou Sistema). |

## UC10 – Acompanhamento de Clientes

| Item | Descrição |
|---|---|
| Caso de Uso | UC10.1 – Listar Clientes Vinculados |
| Finalidade | Exibir ao Profissional a lista de usuários com vínculo ativo e o indicador de registros de cada um. |
| Atores | Profissional de Saúde. |
| Pré-condições | Profissional autenticado [UC1.3] e com status Validado. |
| Fluxo Principal | 1 - **Ator** acessa “Meus clientes”.<br>2 - **Sistema** obtém os vínculos com status Ativo do Profissional.<br>3 - **Sistema** exibe a lista com nome, data de início do vínculo, data do último registro e adesão à meta nos últimos 7 dias [UC5.3].<br>4 - **Ator** seleciona um cliente para ver o diário [UC10.2] ou a evolução [UC10.3]. |
| Fluxos Alternativos | FA1 – Filtrar e ordenar [passo 3]<br>&emsp;1 - **Ator** busca por nome ou ordena por último registro ou adesão.<br>FA2 – Clientes sem registro [passo 3]<br>&emsp;1 - **Sistema** destaca os clientes sem registros há mais de 3 dias.<br>FA3 – Nenhum cliente [passo 2]<br>&emsp;1 - **Sistema** informa que não há clientes vinculados e exibe o código de convite do Profissional.<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Nenhuma alteração de dados. |
| Regras de negócio | RN49: Cada profissional pode ter no máximo 100 clientes com vínculo ativo. |
| Dados Manipulados | **Busca por nome** com no máximo 60 caracteres, opcional. **Ordenação** escolhida em lista (último registro ou adesão), opcional. **Nome do cliente**, **data de início do vínculo** (DD/MM/AAAA), **data do último registro** e **adesão à meta** em percentual, somente leitura. |

| Item | Descrição |
|---|---|
| Caso de Uso | UC10.2 – Consultar Diário do Cliente |
| Finalidade | Permitir ao Profissional ver o diário alimentar de um cliente vinculado, em modo somente leitura. |
| Atores | Profissional de Saúde. |
| Pré-condições | Profissional autenticado [UC1.3]; vínculo ativo com o cliente; diário incluído no escopo autorizado [UC9.3]. |
| Fluxo Principal | 1 - **Ator** seleciona um cliente na lista [UC10.1] e escolhe “Diário”.<br>2 - **Sistema** verifica o vínculo e o escopo autorizado.<br>3 - **Sistema** exibe o diário do dia atual, no formato do [UC4.4], em modo somente leitura [RN52].<br>4 - **Sistema** registra o acesso no log [RN51]. |
| Fluxos Alternativos | FA1 – Navegar entre dias [passo 3]<br>&emsp;1 - **Ator** altera a data consultada.<br>&emsp;2 - **Sistema** retorna ao passo 3.<br>FA2 – Fotos não autorizadas [passo 3]<br>&emsp;1 - **Sistema** não exibe as fotos e indica que não fazem parte do escopo autorizado.<br>FA3 – Vínculo inativo ou acesso revogado [passo 2]<br>&emsp;1 - **Sistema** nega o acesso e remove o cliente da lista [RN50].<br>FA4 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Nenhuma alteração nos dados do cliente; acesso registrado no log [RN51]. |
| Regras de negócio | RN50: Após o encerramento, o profissional não acessa nenhum dado do Usuário, inclusive do período em que o vínculo esteve ativo.<br>RN51: Todo acesso de um profissional aos dados de um cliente é registrado (profissional, cliente, dado acessado, data/hora), e o Usuário pode consultar esse histórico.<br>RN52: O profissional não pode criar, alterar ou excluir dados do cliente. |
| Dados Manipulados | **Data** DD/MM/AAAA, obrigatória. **Refeições, itens e totais nutricionais**, **meta do cliente** e **fotos** (se autorizadas), somente leitura. **Registro de acesso** gerado pelo sistema (profissional, cliente, dado acessado e data/hora). |

| Item | Descrição |
|---|---|
| Caso de Uso | UC10.3 – Consultar Evolução do Cliente |
| Finalidade | Exibir ao Profissional a evolução do cliente em um período: consumo calórico, macronutrientes, adesão à meta, hidratação e peso. |
| Atores | Profissional de Saúde. |
| Pré-condições | Profissional autenticado [UC1.3]; vínculo ativo com o cliente. |
| Fluxo Principal | 1 - **Ator** seleciona um cliente na lista [UC10.1] e escolhe “Evolução”.<br>2 - **Sistema** verifica o vínculo e o escopo autorizado.<br>3 - **Sistema** seleciona por padrão os últimos 30 dias, limitados à data de início do vínculo [RN53].<br>4 - **Sistema** exibe o histórico de consumo [UC5.2], a comparação com a meta [UC5.3], a evolução da hidratação [UC6.4] e do peso, conforme o escopo autorizado.<br>5 - **Sistema** registra o acesso no log [RN51]. |
| Fluxos Alternativos | FA1 – Alterar período [passo 3]<br>&emsp;1 - **Ator** escolhe outro período, respeitando a RN53.<br>&emsp;2 - **Sistema** retorna ao passo 4.<br>FA2 – Exportar [passo 4]<br>&emsp;1 - **Ator** exporta o relatório em PDF para uso na consulta.<br>FA3 – Vínculo inativo ou acesso revogado [passo 2]<br>&emsp;1 - **Sistema** nega o acesso [RN50].<br>FA4 – Período sem registros [passo 4]<br>&emsp;1 - **Sistema** informa que não há dados no período.<br>FA5 – Falha técnica ou indisponibilidade do sistema [Pode ocorrer em qualquer ponto do UC]<br>&emsp;1 - **Sistema** exibe mensagem de erro técnico com confirmação do **Ator**.<br>&emsp;2 - **Sistema** registra o erro no log de auditoria.<br>&emsp;3 - **Sistema** retorna ao menu principal ou à tela inicial do caso de uso. |
| Pós-condições | Nenhuma alteração nos dados do cliente; acesso registrado no log [RN51]. |
| Regras de negócio | RN50: Após o encerramento, o profissional não acessa nenhum dado do Usuário, inclusive do período em que o vínculo esteve ativo.<br>RN51: Todo acesso de um profissional aos dados de um cliente é registrado (profissional, cliente, dado acessado, data/hora), e o Usuário pode consultar esse histórico.<br>RN53: O profissional pode ver dados a partir de 30 dias antes da data de início do vínculo, para ter um histórico de referência. |
| Dados Manipulados | **Período** com data inicial e final no formato DD/MM/AAAA, obrigatórias e limitadas pela RN53. **Médias de calorias e macronutrientes**, **adesão à meta** (percentual), **consumo de água** (ml) e **evolução do peso** (kg), somente leitura. **Formato de exportação** PDF, opcional. |
