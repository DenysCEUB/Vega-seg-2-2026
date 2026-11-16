# 🏗️ Diagrama de Classes

## 1. Introdução

O Diagrama de Classes do NutriCount foi elaborado a partir do **Diagrama de Caso de Uso de segundo nível** e da **Especificação de Caso de Uso**, seguindo o modelo da disciplina (Moodle): um diagrama completo para cada caso de uso agrupador.

- Cada **caso de uso agrupador** (UC1 a UC10) virou uma **classe principal** (ex.: `CadastroUsuario`), com os atributos iguais ao campo *Dados Manipulados* da especificação.
- Cada **caso de uso específico** (UC1.1 a UC10.3) virou uma **classe com a operação do caso de uso** (ex.: `InclusaoUsuario` com `incluir(CadastroUsuario): boolean`).
- Os relacionamentos **«include»** (seta tracejada que sai da classe agrupadora) e **«extend»** (seta tracejada que sai da classe que estende, em direção à agrupadora) seguem o Diagrama de Caso de Uso de segundo nível.
- Os valores fixos (status, tipos, UF etc.) viraram **classes de enumeração** (`<<enum>>`).

### 1.1 Itens mínimos exigidos

| Exigência | Onde aparece |
|---|---|
| **Herança** | `Pessoa` ← `CadastroUsuario` (UC1) e `Pessoa` ← `CadastroProfissional` (UC8) |
| **Composição** | `PerfilMetaCalorica` ◆ `MetaCalorica` e `RegistroPeso`; `DiarioAlimentar` ◆ `ItemRefeicao` e `FotoRefeicao`; `ControleHidratacao` ◆ `RegistroAgua`; `VinculoProfissional` ◆ `Consentimento`, entre outras |
| **Inclusão («include»)** | 16 relações, ex.: `CadastroUsuario` → `AutenticacaoUsuario` (UC1.3) |
| **Extensão («extend»)** | 24 relações, ex.: `InclusaoUsuario` → `CadastroUsuario` (UC1.1) |
| **Classe de enumeração** | Em todos os diagramas, ex.: `TipoRefeicao`, `StatusVinculo`, `NivelAtividade`, `Uf` |

---

## 2. Diagrama por caso de uso agrupador

### 3.1 UC1 — Cadastro de Usuário

`CadastroUsuario` herda de `Pessoa` (herança). As classes `InclusaoUsuario` [UC1.1], `AlteracaoUsuario` [UC1.2] e `DesativacaoUsuario` [UC1.4] estendem o cadastro; `AutenticacaoUsuario` [UC1.3] é incluída. O atributo `perfil` guarda se o usuário é comum ou administrador.

![Diagrama de classes UC1 — Cadastro de Usuário](https://github.com/user-attachments/assets/32377d15-8f9a-4d6f-9c80-bca3e99f8312)

### 3.2 UC2 — Perfil e Meta Calórica

`PerfilMetaCalorica` é composta por `MetaCalorica` e `RegistroPeso`. `AlteracaoPerfil` [UC2.1] e `DefinicaoMetaCalorica` [UC2.2] estendem; `CalculoNecessidadeCalorica` [UC2.3] e `ConsultaPerfil` [UC2.4] são incluídas.

![Diagrama de classes UC2 — Perfil e Meta Calórica](https://github.com/user-attachments/assets/21bfa840-6ee3-47fc-9044-7ca012ec9558)

### 3.3 UC3 — Catálogo de Alimentos

`CatalogoAlimento` é composto por `MedidaCaseira`. `InclusaoAlimento` [UC3.1], `AlteracaoAlimento` [UC3.2], `DesativacaoAlimento` [UC3.4] e `ImportacaoAlimentoBaseExterna` [UC3.5] estendem; `ConsultaAlimento` [UC3.3] é incluída (e também usada pelo UC4.1).

![Diagrama de classes UC3 — Catálogo de Alimentos](https://github.com/user-attachments/assets/b34785d7-51a0-4abb-9a50-0f1afb00838d)

### 3.4 UC4 — Diário Alimentar

`DiarioAlimentar` é composto por `ItemRefeicao` e `FotoRefeicao`. `RegistroRefeicao` [UC4.1], `AlteracaoRefeicao` [UC4.2] e `ExclusaoRefeicao` [UC4.3] estendem; `AnexoFotoRefeicao` [UC4.5] estende o `RegistroRefeicao`; `ConsultaDiario` [UC4.4] é incluída.

![Diagrama de classes UC4 — Diário Alimentar](https://github.com/user-attachments/assets/5dc37690-2158-46f3-8cae-a2ff86fa15e5)

### 3.5 UC5 — Relatórios de Consumo

`RelatorioConsumo` é composto por `ConsumoDiario`. As três classes [UC5.1 a UC5.3] são incluídas.

![Diagrama de classes UC5 — Relatórios de Consumo](https://github.com/user-attachments/assets/c146005c-4aaf-4733-b600-871a46b3863a)

### 3.6 UC6 — Controle de Hidratação

`ControleHidratacao` é composto por `RegistroAgua`. `DefinicaoMetaAgua` [UC6.1], `RegistroConsumoAgua` [UC6.2] e `AlteracaoConsumoAgua` [UC6.3] estendem; `ConsultaEvolucaoHidratacao` [UC6.4] é incluída.

![Diagrama de classes UC6 — Controle de Hidratação](https://github.com/user-attachments/assets/55264279-e125-4436-acbe-b5cc67608826)

### 3.7 UC7 — Lembretes de Refeição

`LembreteRefeicao` é composto por `LogEnvio`. `DefinicaoHorarioRefeicao` [UC7.1] e `AlteracaoHorarioRefeicao` [UC7.2] estendem; `EnvioLembreteRefeicao` [UC7.3] é incluída.

![Diagrama de classes UC7 — Lembretes de Refeição](https://github.com/user-attachments/assets/5a892471-b7fd-4609-a108-088e8e9745c4)

### 3.8 UC8 — Cadastro de Profissional

`CadastroProfissional` herda de `Pessoa` e é composto por `ValidacaoRegistro`. `InclusaoProfissional` [UC8.1], `AlteracaoProfissional` [UC8.2] e `DesativacaoProfissional` [UC8.4] estendem; `ValidacaoRegistroProfissional` [UC8.3] é incluída.

![Diagrama de classes UC8 — Cadastro de Profissional](https://github.com/user-attachments/assets/0db2b6e5-fa41-423c-8581-792d2b59540a)

### 3.9 UC9 — Vínculo com Profissional

`VinculoProfissional` é composto por `Consentimento`. `SolicitacaoVinculo` [UC9.2], `AceiteVinculo` [UC9.4] e `EncerramentoVinculo` [UC9.5] estendem; `BuscaProfissional` [UC9.1] é incluída; `AutorizacaoCompartilhamentoDados` [UC9.3] é incluída pela `SolicitacaoVinculo`.

![Diagrama de classes UC9 — Vínculo com Profissional](https://github.com/user-attachments/assets/42ec4d00-2157-4335-8aa7-c0fbe7ab3502)

### 3.10 UC10 — Acompanhamento de Clientes

`AcompanhamentoCliente` é composto por `LogAcesso`. As três classes [UC10.1 a UC10.3] são incluídas.

![Diagrama de classes UC10 — Acompanhamento de Clientes](https://github.com/user-attachments/assets/dc0039bb-6b74-4c15-b587-c878e3eb8089)

---

## 3. Rastreabilidade: caso de uso × classe

| Caso de uso | Classe | Relação | Operação |
|---|---|---|---|
| UC1 | CadastroUsuario | agrupador | desativar(string) |
| UC1.1 | InclusaoUsuario | «extend» | incluir(CadastroUsuario) |
| UC1.2 | AlteracaoUsuario | «extend» | alterar(CadastroUsuario) |
| UC1.3 | AutenticacaoUsuario | «include» | autenticar(string, string) |
| UC1.4 | DesativacaoUsuario | «extend» | desativar(CadastroUsuario) |
| UC2 | PerfilMetaCalorica | agrupador | calcularIMC() |
| UC2.1 | AlteracaoPerfil | «extend» | alterar(PerfilMetaCalorica) |
| UC2.2 | DefinicaoMetaCalorica | «extend» | definir(PerfilMetaCalorica) |
| UC2.3 | CalculoNecessidadeCalorica | «include» | calcular(PerfilMetaCalorica) |
| UC2.4 | ConsultaPerfil | «include» | consultar(PerfilMetaCalorica) |
| UC3 | CatalogoAlimento | agrupador | desativar(string) |
| UC3.1 | InclusaoAlimento | «extend» | incluir(CatalogoAlimento) |
| UC3.2 | AlteracaoAlimento | «extend» | alterar(CatalogoAlimento) |
| UC3.3 | ConsultaAlimento | «include» | consultar(string) |
| UC3.4 | DesativacaoAlimento | «extend» | desativar(CatalogoAlimento) |
| UC3.5 | ImportacaoAlimentoBaseExterna | «extend» | importar(string) |
| UC4 | DiarioAlimentar | agrupador | calcularTotalCalorias() |
| UC4.1 | RegistroRefeicao | «extend» | registrar(DiarioAlimentar) |
| UC4.2 | AlteracaoRefeicao | «extend» | alterar(DiarioAlimentar) |
| UC4.3 | ExclusaoRefeicao | «extend» | excluir(DiarioAlimentar) |
| UC4.4 | ConsultaDiario | «include» | consultar(date) |
| UC4.5 | AnexoFotoRefeicao | «extend» de UC4.1 | anexar(DiarioAlimentar, FotoRefeicao) |
| UC5 | RelatorioConsumo | agrupador | exportar(FormatoExportacao) |
| UC5.1 | ConsultaResumoDiario | «include» | consultar(date) |
| UC5.2 | ConsultaHistorico | «include» | consultar(date, date) |
| UC5.3 | ComparacaoConsumoMeta | «include» | comparar(RelatorioConsumo) |
| UC6 | ControleHidratacao | agrupador | totalDoDia(date) |
| UC6.1 | DefinicaoMetaAgua | «extend» | definir(ControleHidratacao) |
| UC6.2 | RegistroConsumoAgua | «extend» | registrar(ControleHidratacao, RegistroAgua) |
| UC6.3 | AlteracaoConsumoAgua | «extend» | alterar(RegistroAgua) |
| UC6.4 | ConsultaEvolucaoHidratacao | «include» | consultar(ControleHidratacao) |
| UC7 | LembreteRefeicao | agrupador | pausar(date) |
| UC7.1 | DefinicaoHorarioRefeicao | «extend» | definir(LembreteRefeicao) |
| UC7.2 | AlteracaoHorarioRefeicao | «extend» | alterar(LembreteRefeicao) |
| UC7.3 | EnvioLembreteRefeicao | «include» | enviar(LembreteRefeicao) |
| UC8 | CadastroProfissional | agrupador | desativar(string) |
| UC8.1 | InclusaoProfissional | «extend» | incluir(CadastroProfissional) |
| UC8.2 | AlteracaoProfissional | «extend» | alterar(CadastroProfissional) |
| UC8.3 | ValidacaoRegistroProfissional | «include» | validar(CadastroProfissional) |
| UC8.4 | DesativacaoProfissional | «extend» | desativar(CadastroProfissional) |
| UC9 | VinculoProfissional | agrupador | encerrar(string) |
| UC9.1 | BuscaProfissional | «include» | buscar(string) |
| UC9.2 | SolicitacaoVinculo | «extend» | solicitar(VinculoProfissional) |
| UC9.3 | AutorizacaoCompartilhamentoDados | «include» de UC9.2 | autorizar(VinculoProfissional, Consentimento) |
| UC9.4 | AceiteVinculo | «extend» | aceitar(VinculoProfissional), recusar(VinculoProfissional, string) |
| UC9.5 | EncerramentoVinculo | «extend» | encerrar(VinculoProfissional) |
| UC10 | AcompanhamentoCliente | agrupador | registrarAcesso(EscopoDado) |
| UC10.1 | ListagemClientesVinculados | «include» | listar() |
| UC10.2 | ConsultaDiarioCliente | «include» | consultar(AcompanhamentoCliente) |
| UC10.3 | ConsultaEvolucaoCliente | «include» | consultar(AcompanhamentoCliente) |

## 4. Enumerações

| Enumeração | Valores | Origem |
|---|---|---|
| StatusConta | ATIVO, INATIVO, BLOQUEADO | UC1.1, UC1.3, UC1.4 |
| PerfilAcesso | USUARIO, ADMINISTRADOR | UC1.1 |
| Sexo | MASCULINO, FEMININO | UC1.1, UC2.1, RN14 |
| NivelAtividade | SEDENTARIO, LEVEMENTE_ATIVO, MODERADAMENTE_ATIVO, MUITO_ATIVO, EXTREMAMENTE_ATIVO | RN10 |
| Objetivo | PERDER_PESO, MANTER_PESO, GANHAR_PESO | RN15 |
| OrigemMeta | CALCULADA, PERSONALIZADA | UC2.2, UC6.1 |
| CategoriaAlimento | CEREAIS, LEGUMINOSAS, CARNES_OVOS, LATICINIOS, FRUTAS, VERDURAS_LEGUMES, BEBIDAS, OUTROS | UC3.1 |
| UnidadeMedida | GRAMA, MILILITRO | UC3.1 |
| OrigemAlimento | MANUAL, BASE_EXTERNA | UC3.1, UC3.5 |
| Visibilidade | PUBLICA, PRIVADA | RN19 |
| TipoRefeicao | CAFE_DA_MANHA, LANCHE_DA_MANHA, ALMOCO, LANCHE_DA_TARDE, JANTAR, CEIA | RN25 |
| FormatoFoto | JPG, PNG, HEIC | RN29 |
| SituacaoMeta | DENTRO_DA_META, ABAIXO_DA_META, ACIMA_DA_META | RN33 |
| FormatoExportacao | PDF, CSV | UC5.2 |
| DiaSemana | SEG, TER, QUA, QUI, SEX, SAB, DOM | UC7.1 |
| CanalNotificacao | PUSH, EMAIL | UC7.1 |
| SituacaoEnvio | ENVIADO, FALHA, NAO_ENVIADO | UC7.3 |
| Profissao | NUTRICIONISTA, NUTROLOGO, MEDICO, EDUCADOR_FISICO | UC8.1 |
| ConselhoProfissional | CRN, CRM, CREF | UC8.1, RN40 |
| StatusValidacao | PENDENTE, VALIDADO, REJEITADO, INATIVO | UC8.3 |
| DecisaoValidacao | APROVADO, REJEITADO, DOCUMENTO_SOLICITADO | UC8.3 |
| StatusVinculo | SOLICITADO, ATIVO, RECUSADO, CANCELADO, EXPIRADO, ENCERRADO | UC9.2 a UC9.5, RN46 |
| EscopoDado | DIARIO_ALIMENTAR, FOTOS, HIDRATACAO, PERFIL_PESO | UC9.3, RN47 |
| Uf | 27 unidades da federação | UC8.1 |
