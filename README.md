# FromTech Platform
### Mapa completo de produtos, arquitetura e status
**FromTech · Caio Rodrigues · Maringá, PR · Atualizado: Agosto 2026**

---

## O que é a FromTech

Empresa de tecnologia e dados para o setor industrial e para pequenas empresas.
Fundada por Caio Rodrigues, PJ com CNAE 6204-0/00, sediada em Maringá, PR.

**Tagline:** *"Tecnologia e dados para a indústria que produz."*

> A FromTech não faz animação 3D. Foco em análise de dados industrial,
> sistemas de gestão e produtos digitais.

---

## Estrutura de produtos

FromTech
│
├── ALUPRO ──────────────── App mobile Android — produto separado
│
└── FromTech Platform ───── Super app web modular
├── Módulo Carreira GRATUITO
├── Módulo Financeiro R$ 9,90/mês
├── Módulo Fiscal R$ 14,90/mês
├── Módulo Aprendizado R$ 9,90/mês
├── Módulo Vocacional integrado ao Carreira
└── Plano Completo R$ 34,90/mês


---

## ALUPRO — Produto separado

> App mobile Android de catálogo e identificação de perfis de alumínio

| Item | Detalhe |
|---|---|
| Repositório | [scaiorodrigues/ALUPRO](https://github.com/scaiorodrigues/ALUPRO) |
| Stack | React Native + Expo Router + Supabase + Python/FastAPI |
| Infra | VPS Hostinger KVM2 · Ubuntu 24.04 · IP 31.97.19.180 |
| Banco | 679+ perfis catalogados (foto, dimensões, peso/metro, fornecedores) |
| Lançamento | Feira São Paulo — setembro 2026 |
| Monetização | Assinatura anual B2B |
| Status | ⚠️ Infra parcialmente configurada — SQL, DNS e SSL pendentes |

**Pendências técnicas:**
- [ ] Executar schema SQL no Supabase
- [ ] Criar storage bucket e admin user
- [ ] Apontar DNS
- [ ] Configurar SSL

---

## FromTech Platform — Super app web modular

> PWA único com múltiplos módulos. Um login, dados integrados entre módulos.

### Stack padrão

Frontend HTML + CSS + JavaScript vanilla
Hospedagem Netlify
Banco Supabase (PostgreSQL com RLS)
Auth Supabase Auth
IA Gemini API (gratuita) — carreira, fiscal, legislação
Anthropic API (claude-sonnet-4-6) — flashcards, relatórios
Deploy Netlify — fromtech.com.br ou subdomínios
Design Space Grotesk + Inter + JetBrains Mono

#0D1117 · 
#2F7CF6 · 
#F4F7FF


### Princípios de desenvolvimento

- Mobile-first obrigatório — testar em 375px primeiro
- Todo código em index.html — nunca frameworks JS externos
- RLS habilitado no Supabase desde o início
- Módulos se conectam — dados do Carreira alimentam o Financeiro
- Módulo Carreira sempre gratuito e acessível sem login

---

### Módulo 1 — Carreira (GRATUITO)

**CarreiraPatrimônio** — simulador de profissão × salário × patrimônio

| Item | Detalhe |
|---|---|
| Documento | `carreira-patrimonio-webapp.md` |
| Status | ✅ Arquitetado — pronto para desenvolvimento |
| IA | Gemini API — interpreta profissão em texto livre |
| APIs | FIPE (veículos) · BrasilAPI (CNPJ) · ZAP Imóveis (links) |

**O que faz:**
- Usuário digita profissão e região em texto livre
- Gemini interpreta e retorna salário mediano por região
- Calcula faixa de imóvel (30% renda × 360 meses)
- Calcula faixa de veículo (20% renda × 48 meses)
- Mostra exemplos reais de veículos via FIPE
- Links filtrados para ZAP Imóveis e Viva Real
- Cargos públicos equivalentes com salários reais
- Sugestões de profissões em crescimento na região

**Conexões com outros módulos:**

→ Módulo Financeiro botão "Como juntar a entrada?" — pré-preenche meta
→ Módulo Aprendizado botão "Gerar deck de estudo" — pré-preenche tema
→ Módulo Vocacional teste RIASEC leva ao CarreiraPatrimônio


---

### Módulo 2 — Financeiro (R$ 9,90/mês)

**Simulador de metas financeiras** — planejamento da entrada do imóvel e investimentos

| Item | Detalhe |
|---|---|
| Status | ✅ Arquitetado — integra ao CarreiraPatrimônio |
| Banco | Tabelas: metas_financeiras, aportes |

**O que faz:**
- Simulador de meta — valor, prazo, renda, aporte mensal
- Comparador de produtos: LCI, CDB, Tesouro Selic, Poupança
  - Desconta IR pela tabela regressiva correta
  - Destaca o melhor produto para o prazo informado
- Gráfico de progresso — projetado vs real (Chart.js)
- Registro de aportes mensais
- Conexão automática com faturamento do MEI Fácil

**Taxas configuradas (Selic 14,75% — 2026):**
```javascript
LCI:      90% CDI isento IR  → ~12,96% líquido
CDB:     100% CDI            → ~11,88% líquido (IR 17,5%)
Tesouro: 100% CDI            → ~11,96% líquido
Poupança: 7,50% aa           → isento IR
```

---

### Módulo 3 — Fiscal (R$ 14,90/mês)

**MEI Fácil** — gestão fiscal completa para MEIs e Simples Nacional

| Item | Detalhe |
|---|---|
| Documento | `mei-facil-webapp.md` |
| Status | ✅ Arquitetado — timing crítico: set/2026 |
| APIs | TecnoSpeed/Plugnotas (NFS-e) · BrasilAPI · Gemini · Infosimples |
| Timing | Resolução CGSN nº 189/2026 — NFS-e obrigatória em setembro 2026 |

**O que faz:**
- Emissão de NFS-e via API credenciada (TecnoSpeed ou Plugnotas)
- Painel DAS — valor, vencimento, status, link PGMEI
- Barra de faturamento anual MEI (limite R$ 144.900)
- Relatório de faturamento para IR — exportação PDF e Excel
- Feed de legislação filtrado pelo Gemini (DOU automatizado)
- Alertas de vencimento e mudanças na lei

> ⚠️ Posicionamento legal: ferramenta de gestão fiscal —
> NÃO consultoria contábil. Aviso obrigatório em todas as telas.

**Modelo de negócio:**

Gratuito 5 notas/mês
Essencial R$ 14,90/mês — notas ilimitadas + DAS automático
Pro R$ 34,90/mês — múltiplos CNPJs + API


---

### Módulo 4 — Aprendizado (R$ 9,90/mês)

**FlashFromTech** — flashcards inteligentes com IA e revisão espaçada

| Item | Detalhe |
|---|---|
| Status | ✅ Arquitetado — prompt Claude Code pronto |
| IA | Anthropic API (claude-sonnet-4-6) — geração automática de cards |
| Algoritmo | SM-2 — mesmo do Anki — implementado em JS puro |

**O que faz:**
- Usuário digita tema → IA gera deck completo automaticamente
- Algoritmo SM-2 calcula quando cada card precisa ser revisado
- Modo matinal (05h–12h): criar + revisar
- Modo noturno (20h–05h): só revisar, máx 15 minutos
- Gráfico de streak e estatísticas de progresso

**Botões de avaliação SM-2:**

Esqueci → 0 | Difícil → 1 | Bom → 3 | Fácil → 5


---

### Módulo 5 — Vocacional (integrado ao Carreira)

**Teste vocacional científico** — RIASEC + Gardner + GOPC

| Item | Detalhe |
|---|---|
| Status | ✅ Arquitetado — prompt Claude Code pronto |
| Metodologias | RIASEC (Holland) + Inteligências Múltiplas (Gardner) + GOPC |
| Públicos | Adolescentes (14–17) e adultos (18+) com linguagem adaptada |

**O que faz:**
- 70 perguntas em ~12 minutos divididas em 3 blocos
- Calcula código Holland (ex: RIA, ISC, ESA)
- Gemini interpreta e retorna perfil + 6 profissões compatíveis
- Salários reais por região para cada profissão
- Gráfico radar RIASEC e barras de inteligências (Chart.js)
- Funciona sem login — resultado completo exige cadastro

---

## Produtos em desenvolvimento independente

### Sistema de Trading Automatizado

| Item | Detalhe |
|---|---|
| Status | ✅ Prompt Claude Code pronto |
| Stack | Python + MetaTrader5 + Supabase + Anthropic API |
| Corretora | Clear Corretora (grupo XP) — corretagem zero ETFs |
| ETFs | IVVB11 · BOVA11 · HASH11 · SMAL11 |

**Fases de desenvolvimento:**

Semana 1–2 Simulação — MODO_SIMULACAO = True
Semana 3 Paper trading com dados reais
Semana 4 R$ 500–1.000 real mínimo
Mês 2+ Escala gradual com supervisão diária

**Duas telas:**
- Frente: diário do dia com checklist gamificado
- Verso: mapa de progresso com objetivos mensais e calendário

---

## Serviços ativos — receita real

### Newsletter de Alumínio

| Item | Detalhe |
|---|---|
| Cliente | Ecoalumi Alumínio S/A |
| Valor | R$ 1.000/mês (aprovação da diretoria pendente) |
| Formato | Curadoria semanal do mercado global de alumínio |
| Contratante | FromTech como prestadora de serviço |
| Status | ⚠️ Negociado — formalização pendente |

### Sistema de gestão de ferramentas

| Item | Detalhe |
|---|---|
| Cliente | Ecoalumi Alumínio S/A |
| Status | ✅ Em produção — uso diário |
| Stack | Vanilla JS + Supabase + Google Sheets + Netlify |

---

## Projetos criativos

### Livros infantis — Isadora

| Item | Detalhe |
|---|---|
| Processo | Desenho à mão → refinamento com Adobe Firefly/ClipDrop |
| Publicação | Amazon KDP + Simplíssimo |
| Status | ⚠️ Conteúdo criado — ilustrações pendentes |

### Verath — novela de fantasia

| Item | Detalhe |
|---|---|
| Conceito | Análise de dados como magia — nomenclatura técnica integrada |
| Repositório | [scaiorodrigues/framework-consciencia-pilares-de-vida](https://github.com/scaiorodrigues/framework-consciencia-pilares-de-vida) |
| Status | ⚠️ Mundo construído — Capítulo 1 pendente |
| Co-autoria | Claude — projeto separado |

---

## Status geral — o que está pronto vs pendente

### ✅ Pronto

- Site FromTech — arquivo HTML gerado (publicar no Netlify)
- Documento de contexto — `fromtech-contexto-projeto.md`
- Arquitetura FromTech Platform — prompt Claude Code
- Arquitetura MEI Fácil — documento completo
- Arquitetura CarreiraPatrimônio — documento completo
- Arquitetura FlashFromTech — prompt Claude Code
- Arquitetura Sistema de Trading — prompt Claude Code
- Instrução Módulo Vocacional — prompt Claude Code
- Newsletter negociada com gestor da Ecoalumi
- Framework de Consciência — repositório ativo com atualizações

### ⚠️ Pendente — próximos passos

**Urgente (agosto–setembro 2026)**
- [ ] Publicar site FromTech no Netlify
- [ ] Formalizar contrato da newsletter com Ecoalumi
- [ ] Executar SQL do ALUPRO no Supabase
- [ ] Apontar DNS e SSL do ALUPRO
- [ ] ALUPRO pronto para a feira de setembro

**Médio prazo (outubro–dezembro 2026)**
- [ ] FromTech Platform — Módulo Carreira no ar
- [ ] QuestLog — validar uso pessoal por 30 dias
- [ ] Abrir conta na Clear Corretora (trading)

**Longo prazo (2027)**
- [ ] MEI Fácil — lançar aproveitando obrigatoriedade NFS-e
- [ ] FlashFromTech — integrar na plataforma
- [ ] Módulo Financeiro — integrar ao CarreiraPatrimônio
- [ ] Ilustrar e publicar primeiro livro da Isadora
- [ ] Capítulo 1 de Verath

---

## Documentos de referência

| Documento | Conteúdo |
|---|---|
| `fromtech-contexto-projeto.md` | Contexto completo para Claude — instrução de projeto |
| `carreira-patrimonio-webapp.md` | Arquitetura completa do CarreiraPatrimônio |
| `mei-facil-webapp.md` | Arquitetura completa do MEI Fácil |
| `fromtech-site.html` | Site institucional pronto para publicar |

---

## Repositórios relacionados

| Repositório | Produto |
|---|---|
| [scaiorodrigues/ALUPRO](https://github.com/scaiorodrigues/ALUPRO) | App mobile de perfis de alumínio |
| [scaiorodrigues/framework-consciencia-pilares-de-vida](https://github.com/scaiorodrigues/framework-consciencia-pilares-de-vida) | Framework de desenvolvimento pessoal |

---

*FromTech · Caio Rodrigues · Maringá, PR*
*Tecnologia e dados para a indústria que produz*
