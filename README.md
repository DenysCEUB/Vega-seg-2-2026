# 🏗️ Diagrama de Classes

## 1. Introdução

O Diagrama de Classes do NutriCount foi elaborado a partir do **Diagrama de Caso de Uso de segundo nível** e da **Especificação de Caso de Uso**, seguindo o modelo da disciplina:

- Cada **caso de uso agrupador** (UC1 a UC10) virou uma **classe principal** (ex.: `CadastroUsuario`, `CadastroRefeicao`), com os atributos tirados do campo *Dados* da especificação.
- Cada **caso de uso específico** (UC1.1 a UC10.3) virou uma **classe com a operação do caso de uso** (ex.: `InclusaoUsuario.incluir(CadastroUsuario) : boolean`).
- Os relacionamentos **«include»** e **«extend»** do diagrama de caso de uso foram mantidos entre essas classes. Quando o caso de uso é «include», a seta sai da classe agrupadora para a classe incluída. Quando é «extend», a seta sai da classe que estende para a classe agrupadora.
- Os valores fixos das regras de negócio (status, tipos de refeição, objetivos, UF etc.) viraram **classes de enumeração**.

### 1.1 Itens mínimos exigidos

| Exigência | Onde aparece no diagrama |
|---|---|
| **Herança** | `CadastroUsuario`, `CadastroProfissional` e `Administrador` herdam da classe abstrata `Pessoa` |
| **Composição** | `CadastroRefeicao` ◆ `ItemRefeicao` e `FotoRefeicao`; `CadastroPerfil` ◆ `MetaCalorica` e `RegistroPeso`; `ControleHidratacao` ◆ `RegistroAgua`; `Vinculo` ◆ `Consentimento`; `AcompanhamentoCliente` ◆ `LogAcesso`; `CadastroUsuario` ◆ `CadastroPerfil` |
| **Agregação** | `CadastroUsuario` ◇ `CadastroLembrete` |
| **Inclusão («include»)** | 17 relações, ex.: `CadastroUsuario` → `AutenticacaoUsuario` (UC1.3), `SolicitacaoVinculo` → `AutorizacaoCompartilhamento` (UC9.3), `RegistroRefeicao` → `ConsultaAlimento` (UC3.3) |
| **Extensão («extend»)** | 24 relações, ex.: `InclusaoUsuario` → `CadastroUsuario` (UC1.1), `AnexoFotoRefeicao` → `RegistroRefeicao` (UC4.5) |
| **Classe de enumeração** | 19 enumerações, ex.: `TipoRefeicao`, `StatusVinculo`, `NivelAtividade`, `Uf` |
| **Demais relações** | Associações com multiplicidade (ex.: `CadastroUsuario` 1 — 0..3 `Vinculo`, RN45), realização (`TacoApiAdapter` implementa `BaseNutricionalExterna`) e dependências «use» |

### 1.2 Legenda

| Elemento | Significado |
|---|---|
| Caixa verde | Classe do caso de uso agrupador (UC1 a UC10) |
| Caixa branca com [UCx.y] | Classe do caso de uso específico |
| Caixa cinza | Classe de domínio auxiliar (parte de uma composição) |
| Caixa amarela «enumeration» | Classe de enumeração |
| Caixa azul «abstract» / lilás «interface» | Classe abstrata / interface |
| Seta tracejada azul «include» | Inclusão |
| Seta tracejada laranja «extend» | Extensão |
| Seta com triângulo vazado | Herança (ou realização, se tracejada) |
| Losango preenchido / vazado | Composição / agregação |

---

## 2. Diagrama de Classes completo

<img width="8380" height="2668" alt="diagrama-classes-completo" src="https://github.com/user-attachments/assets/6657cc0d-7a63-4afc-8145-36f97b443a01" />

> A imagem completa é grande. Clique nela para abrir em tamanho real e ampliar. Os recortes abaixo mostram cada caso de uso agrupador com as suas classes.

---

## 3. Diagrama por caso de uso agrupador

### 3.1 UC1 — Cadastro de Usuário
Classes: `CadastroUsuario` (herda de `Pessoa`), `InclusaoUsuario` [UC1.1, «extend»], `AlteracaoUsuario` [UC1.2, «extend»], `AutenticacaoUsuario` [UC1.3, «include»], `DesativacaoUsuario` [UC1.4, «extend»].

<img width="1747" height="1085" alt="classes-uc1" src="https://github.com/user-attachments/assets/6971568d-2256-4dfa-91f2-d389fababa50" />

### 3.2 UC2 — Perfil e Meta Calórica
Classes: `CadastroPerfil` (composto por `MetaCalorica` e `RegistroPeso`), `AlteracaoPerfil` [UC2.1], `DefinicaoMetaCalorica` [UC2.2], `CalculoNecessidadeCalorica` [UC2.3, «include»], `ConsultaPerfil` [UC2.4, «include»].

<img width="1727" height="1088" alt="classes-uc2" src="https://github.com/user-attachments/assets/34704c61-d1b7-4bf5-a322-0bb3e62cbaa4" />

### 3.3 UC3 — Catálogo de Alimentos
Classes: `CadastroAlimento`, `InclusaoAlimento` [UC3.1], `AlteracaoAlimento` [UC3.2], `ConsultaAlimento` [UC3.3, «include»], `DesativacaoAlimento` [UC3.4], `ImportacaoAlimento` [UC3.5], que usa a interface `BaseNutricionalExterna`, implementada por `TacoApiAdapter`.

<img width="2668" height="1062" alt="classes-uc3" src="https://github.com/user-attachments/assets/8e5c6982-a35e-4ef6-948b-9d5ac6f7b423" />

### 3.4 UC4 — Diário Alimentar
Classes: `CadastroRefeicao` (composta por `ItemRefeicao` e `FotoRefeicao`), `RegistroRefeicao` [UC4.1], `AlteracaoRefeicao` [UC4.2], `ExclusaoRefeicao` [UC4.3], `ConsultaDiario` [UC4.4, «include»] e `AnexoFotoRefeicao` [UC4.5], que estende `RegistroRefeicao`. `ItemRefeicao` guarda cópia dos valores nutricionais do alimento (RN20).

<img width="1717" height="1780" alt="classes-uc4" src="https://github.com/user-attachments/assets/2a0f87bc-f7a8-4c37-bf79-25b9ce81858c" />

### 3.5 UC5 — Relatórios de Consumo
Classes: `RelatorioConsumo`, `ConsultaResumoDiario` [UC5.1], `ConsultaHistorico` [UC5.2] e `ComparacaoMeta` [UC5.3], todos «include». `ComparacaoMeta` devolve a enumeração `SituacaoMeta` (RN33).

<img width="1914" height="1091" alt="classes-uc5" src="https://github.com/user-attachments/assets/8d3cb8a5-a8b6-46bd-b8b9-3d2f0b7e8e0f" />

### 3.6 UC6 — Controle de Hidratação
Classes: `ControleHidratacao` (composto por `RegistroAgua`), `DefinicaoMetaAgua` [UC6.1], `RegistroConsumoAgua` [UC6.2], `AlteracaoConsumoAgua` [UC6.3], `ConsultaEvolucaoHidratacao` [UC6.4, «include»].

<img width="1588" height="812" alt="classes-uc6" src="https://github.com/user-attachments/assets/79eaabc4-94ef-4a21-afe3-50b7d213b8b0" />

### 3.7 UC7 — Lembretes de Refeição
Classes: `CadastroLembrete`, `DefinicaoHorario` [UC7.1], `AlteracaoHorario` [UC7.2], `EnvioLembrete` [UC7.3, «include»], que usa a interface `ServicoNotificacao` e verifica se a refeição já foi registrada (RN38).

<img width="1193" height="1458" alt="classes-uc7" src="https://github.com/user-attachments/assets/2220f471-bef8-4e92-b5db-6f7b3c1fa8f1" />

### 3.8 UC8 — Cadastro de Profissional
Classes: `CadastroProfissional` (herda de `Pessoa`), `InclusaoProfissional` [UC8.1], `AlteracaoProfissional` [UC8.2], `ValidacaoRegistro` [UC8.3, «include»], executada pelo `Administrador`, e `DesativacaoProfissional` [UC8.4].

<img width="1616" height="893" alt="classes-uc8" src="https://github.com/user-attachments/assets/f90ad062-4c75-4780-b4d5-872e98d9378c" />

### 3.9 UC9 — Vínculo com Profissional
Classes: `Vinculo` (composto por `Consentimento`), `BuscaProfissional` [UC9.1, «include»], `SolicitacaoVinculo` [UC9.2], que inclui `AutorizacaoCompartilhamento` [UC9.3], `AceiteVinculo` [UC9.4] e `EncerramentoVinculo` [UC9.5].

<img width="1815" height="1187" alt="classes-uc9" src="https://github.com/user-attachments/assets/47b1a9f8-f605-46dd-b13e-f0079f2a8c40" />

### 3.10 UC10 — Acompanhamento de Clientes
Classes: `AcompanhamentoCliente` (composto por `LogAcesso`, RN51), `ListagemClientes` [UC10.1], `ConsultaDiarioCliente` [UC10.2] e `ConsultaEvolucaoCliente` [UC10.3], todos «include».

<img width="1627" height="1024" alt="classes-uc10" src="https://github.com/user-attachments/assets/d7887b5f-8b87-41b2-85e1-73008928c6ea" />

---

## 4. Rastreabilidade: caso de uso × classe

| Caso de uso | Classe | Relação | Operação |
|---|---|---|---|
| UC1 | CadastroUsuario | agrupador | desativar(), reativar() |
| UC1.1 | InclusaoUsuario | «extend» | incluir(CadastroUsuario) |
| UC1.2 | AlteracaoUsuario | «extend» | alterar(CadastroUsuario) |
| UC1.3 | AutenticacaoUsuario | «include» | autenticar(email, senha), recuperarSenha(email) |
| UC1.4 | DesativacaoUsuario | «extend» | desativar(CadastroUsuario) |
| UC2 | CadastroPerfil | agrupador | calcularIMC() |
| UC2.1 | AlteracaoPerfil | «extend» | alterar(CadastroPerfil) |
| UC2.2 | DefinicaoMetaCalorica | «extend» | definir(CadastroPerfil, MetaCalorica) |
| UC2.3 | CalculoNecessidadeCalorica | «include» | calcular(CadastroPerfil) |
| UC2.4 | ConsultaPerfil | «include» | consultar(CadastroPerfil) |
| UC3 | CadastroAlimento | agrupador | desativar(motivo) |
| UC3.1 | InclusaoAlimento | «extend» | incluir(CadastroAlimento) |
| UC3.2 | AlteracaoAlimento | «extend» | alterar(CadastroAlimento) |
| UC3.3 | ConsultaAlimento | «include» (também incluído por UC4.1) | consultar(termo) |
| UC3.4 | DesativacaoAlimento | «extend» | desativar(CadastroAlimento) |
| UC3.5 | ImportacaoAlimento | «extend» | importar(codigoExterno) |
| UC4 | CadastroRefeicao | agrupador | calcularTotalCalorias() |
| UC4.1 | RegistroRefeicao | «extend» | registrar(CadastroRefeicao) |
| UC4.2 | AlteracaoRefeicao | «extend» | alterar(CadastroRefeicao) |
| UC4.3 | ExclusaoRefeicao | «extend» | excluir(CadastroRefeicao) |
| UC4.4 | ConsultaDiario | «include» | consultar(data) |
| UC4.5 | AnexoFotoRefeicao | «extend» de UC4.1 | anexar(CadastroRefeicao, FotoRefeicao) |
| UC5 | RelatorioConsumo | agrupador | exportar(formato) |
| UC5.1 | ConsultaResumoDiario | «include» | consultar(RelatorioConsumo) |
| UC5.2 | ConsultaHistorico | «include» | consultar(RelatorioConsumo) |
| UC5.3 | ComparacaoMeta | «include» | classificar(RelatorioConsumo) : SituacaoMeta |
| UC6 | ControleHidratacao | agrupador | totalDoDia(data) |
| UC6.1 | DefinicaoMetaAgua | «extend» | definir(ControleHidratacao) |
| UC6.2 | RegistroConsumoAgua | «extend» | registrar(ControleHidratacao, RegistroAgua) |
| UC6.3 | AlteracaoConsumoAgua | «extend» | alterar(RegistroAgua) |
| UC6.4 | ConsultaEvolucaoHidratacao | «include» | consultar(ControleHidratacao) |
| UC7 | CadastroLembrete | agrupador | pausar(ate) |
| UC7.1 | DefinicaoHorario | «extend» | definir(CadastroLembrete) |
| UC7.2 | AlteracaoHorario | «extend» | alterar(CadastroLembrete) |
| UC7.3 | EnvioLembrete | «include» | enviar(CadastroLembrete) |
| UC8 | CadastroProfissional | agrupador | desativar(motivo) |
| UC8.1 | InclusaoProfissional | «extend» | incluir(CadastroProfissional) |
| UC8.2 | AlteracaoProfissional | «extend» | alterar(CadastroProfissional) |
| UC8.3 | ValidacaoRegistro | «include» | validar(CadastroProfissional) |
| UC8.4 | DesativacaoProfissional | «extend» | desativar(CadastroProfissional) |
| UC9 | Vinculo | agrupador | encerrar(motivo) |
| UC9.1 | BuscaProfissional | «include» | buscar(filtro) |
| UC9.2 | SolicitacaoVinculo | «extend» | solicitar(Vinculo) |
| UC9.3 | AutorizacaoCompartilhamento | «include» de UC9.2 | autorizar(Vinculo, Consentimento) |
| UC9.4 | AceiteVinculo | «extend» | aceitar(Vinculo), recusar(Vinculo, motivo) |
| UC9.5 | EncerramentoVinculo | «extend» | encerrar(Vinculo) |
| UC10 | AcompanhamentoCliente | agrupador | registrarAcesso(dado) |
| UC10.1 | ListagemClientes | «include» | listar(CadastroProfissional) |
| UC10.2 | ConsultaDiarioCliente | «include» | consultar(AcompanhamentoCliente) |
| UC10.3 | ConsultaEvolucaoCliente | «include» | consultar(AcompanhamentoCliente) |

## 5. Enumerações

| Enumeração | Valores | Regra de origem |
|---|---|---|
| StatusConta | ATIVO, INATIVO, BLOQUEADO | UC1.1, UC1.3, UC1.4 |
| Sexo | MASCULINO, FEMININO | UC1.1, RN14 |
| NivelAtividade | SEDENTARIO, LEVEMENTE_ATIVO, MODERADAMENTE_ATIVO, MUITO_ATIVO, EXTREMAMENTE_ATIVO | RN10 |
| Objetivo | PERDER_PESO, MANTER_PESO, GANHAR_PESO | RN15 |
| OrigemMeta | CALCULADA, PERSONALIZADA | UC2.2, UC6.1 |
| CategoriaAlimento | CEREAIS, LEGUMINOSAS, CARNES_OVOS, LATICINIOS, FRUTAS, VERDURAS_LEGUMES, BEBIDAS, OUTROS | UC3.1 |
| UnidadeMedida | GRAMA, MILILITRO, UNIDADE, COLHER, XICARA | UC3.1, UC4.1 FA02 |
| OrigemAlimento | MANUAL, BASE_EXTERNA | UC3.1, UC3.5 |
| Visibilidade | PUBLICA, PRIVADA | RN19 |
| TipoRefeicao | CAFE_DA_MANHA, LANCHE_DA_MANHA, ALMOCO, LANCHE_DA_TARDE, JANTAR, CEIA | RN25 |
| SituacaoMeta | DENTRO_DA_META, ABAIXO_DA_META, ACIMA_DA_META | RN33 |
| DiaSemana | SEG a DOM | UC7.1 |
| CanalNotificacao | PUSH, EMAIL | UC7 |
| Profissao | NUTRICIONISTA, NUTROLOGO, MEDICO | UC8.1 |
| ConselhoProfissional | CRN, CRM, CREF | UC8.1, RN40 |
| StatusValidacao | PENDENTE, VALIDADO, REJEITADO, INATIVO | UC8.3 |
| StatusVinculo | SOLICITADO, ATIVO, RECUSADO, CANCELADO, EXPIRADO, ENCERRADO | UC9.2 a UC9.5, RN46 |
| EscopoDado | DIARIO_ALIMENTAR, FOTOS, HIDRATACAO, PERFIL_PESO | UC9.3, RN47 |
| Uf | 27 unidades da federação | UC8.1 |
