# FaceID SaaS — Documento Completo de Projeto

> Versão MVP — Hackathon UMC 2026  
> Autores: Otávio Vinícius Flauzino de Souza  
> Última atualização: Abril 2026

---

## Sumário

1. [Visão geral](#1-visão-geral)
2. [Modelo de negócio](#2-modelo-de-negócio)
3. [Análise competitiva](#3-análise-competitiva)
4. [Produtos](#4-produtos)
5. [Arquitetura do sistema](#5-arquitetura-do-sistema)
6. [Engine de reconhecimento facial](#6-engine-de-reconhecimento-facial)
7. [Anti-spoofing](#7-anti-spoofing)
8. [Logs de auditoria e LGPD](#8-logs-de-auditoria-e-lgpd)
9. [Estrutura de dados completa](#9-estrutura-de-dados-completa)
10. [Multi-tenancy](#10-multi-tenancy)
11. [Fluxos principais](#11-fluxos-principais)
12. [Stack tecnológica](#12-stack-tecnológica)
13. [Infraestrutura e deploy](#13-infraestrutura-e-deploy)
14. [Rotas da API Node.js](#14-rotas-da-api-nodejs)
15. [Superfícies do sistema](#15-superfícies-do-sistema)
16. [Roadmap pós-MVP](#16-roadmap-pós-mvp)
17. [Considerações legais e trabalhistas](#17-considerações-legais-e-trabalhistas)

---

## 1. Visão geral

O FaceID SaaS é uma plataforma de reconhecimento facial como serviço
composta por três produtos distintos:

**FacePresença** — sistema de chamada escolar automatizada via
reconhecimento facial. Multi-tenant, voltado para escolas, institutos
técnicos e universidades. O professor abre a aula no painel e alunos
são reconhecidos automaticamente por um totem (tablet ou celular fixo
na parede). A lista de presença é preenchida em tempo real.

**FacePonto** — sistema de registro de ponto empresarial via
reconhecimento facial. Multi-tenant, voltado para PMEs. Funciona em
modo totem fixo (tablet na parede) ou modo app (celular do funcionário).
Inclui geolocalização para validação de presença física.

**FaceAPI** — API REST pública para desenvolvedores integrarem
reconhecimento facial em suas próprias aplicações. Modelo pay-per-use
por identificação bem-sucedida. Documentação OpenAPI gerada
automaticamente pelo FastAPI.

### Diferencial central

Concorrentes vendem hardware proprietário com contrato de fidelidade
de 12 a 24 meses. O FaceID SaaS elimina completamente o hardware
dedicado — qualquer tablet ou celular vira um ponto de reconhecimento
facial em menos de 5 minutos, sem técnico, sem instalação física,
sem contrato de fidelidade.

---

## 2. Modelo de negócio

### Precificação FacePresença

| Plano | Limite | Preço |
|---|---|---|
| Starter | até 5 turmas ativas | R$ 89/mês |
| Escola | até 20 turmas ativas | R$ 149/mês |
| Instituto | turmas ilimitadas | R$ 249/mês |

### Precificação FacePonto

| Plano | Limite | Preço |
|---|---|---|
| Micro | até 10 funcionários | R$ 49/mês |
| PME | até 30 funcionários | R$ 89/mês |
| Empresa | até 100 funcionários | R$ 179/mês |
| Enterprise | acima de 100 | negociado |

### Precificação FaceAPI

- Free tier: 100 identificações/mês
- Pay-per-use: R$ 0,07 por identificação bem-sucedida
- Plano Dev: R$ 99/mês com 2.000 identificações inclusas

### Sem contrato de fidelidade

Cobrança mensal cancelável a qualquer momento. Essa é a principal
vantagem comercial sobre concorrentes que exigem 12 a 24 meses.

### Projeção de MRR

| Cenário | Tenants | MRR estimado |
|---|---|---|
| Hackathon (demo) | 2–5 | validação |
| 3 meses pós-lançamento | 30 | ~R$ 3.500 |
| 6 meses | 100 | ~R$ 12.000 |
| 12 meses | 300 | ~R$ 38.000 |

---

## 3. Análise competitiva

### Evo Facial (concorrente hardware)

Vende equipamento físico proprietário por R$ 1.485 à vista ou
R$ 98/mês em 16 parcelas. Exige ponto de energia dedicado e cabo de
rede no local de instalação. Software RHiD em nuvem cobrado
separadamente a partir de R$ 69,90/mês para até 30 funcionários.
Contrato mínimo de 12 meses. Instalação em até 7 dias úteis com
visita técnica.

### Mobile/Quiosque (concorrente software)

Sistema sem hardware dedicado com reconhecimento facial via celular.
Plano Lite até 10 funcionários por R$ 49/mês. Plano até 20
funcionários por R$ 119/mês. Inclui geolocalização e modo quiosque.
Não é REP homologado — posicionado como sistema de gestão auxiliar.

### Comparativo

| Critério | Evo Facial | Mobile/Quiosque | FaceID SaaS |
|---|---|---|---|
| Hardware próprio | Obrigatório R$1.485 | Não | **Não** |
| Infraestrutura | Energia + cabo de rede | Wi-Fi | **Wi-Fi** |
| Contrato mínimo | 12 meses | Não informado | **Sem fidelidade** |
| Até 10 funcionários | — | R$ 49/mês | **R$ 49/mês** |
| Até 30 funcionários | R$ 239–319/mês | — | **R$ 89/mês** |
| Anti-spoofing | Não informado | Não informado | **Sim (MiniFASNet)** |
| Log auditável com foto | Não | Não | **Sim (24h)** |
| LGPD documentada | Não informado | Não informado | **Sim** |
| API pública | Não | Não | **Sim (FaceAPI)** |
| Chamada escolar | Não | Não | **Sim (FacePresença)** |

### Posicionamento de pitch

"Nossos concorrentes vendem hardware. Nós vendemos inteligência.
Qualquer tela vira um ponto de reconhecimento facial em 5 minutos,
sem técnico, sem instalação, sem contrato."

---

## 4. Produtos

### 4.1 FacePresença

**Problema:** chamada manual desperdiça 5 a 10 minutos por aula,
é sujeita a erros, fraudes e inconsistências nos registros.

**Solução:** totem fixo (tablet ou celular) reconhece os alunos
automaticamente enquanto entram na sala. O professor só precisa
abrir a aula e revisar ao final.

**Fluxo de uso:**
- Professor abre a aula no painel web
- Alunos passam na frente do totem ao entrar
- Lista de presença preenche em tempo real via SSE
- Alunos que chegaram antes da aula ser aberta têm presença
  consolidada automaticamente via pré-presença (Redis)
- Professor revisa e ajusta manualmente se necessário
- Professor fecha a aula — ausentes são marcados automaticamente

**Diferenciais:**
- Pré-presença: aluno reconhecido antes da aula abrir tem presença
  consolidada quando o professor abre
- Ajuste manual com rastreabilidade de quem alterou e quando
- Relatório de frequência por aluno, turma e período letivo
- Nenhum app para instalar — totem é uma PWA no browser do tablet

### 4.2 FacePonto

**Problema:** registro de ponto manual ou com cartão permite fraudes
(ponto de colega). Hardware dedicado tem custo alto e instalação
complexa.

**Solução:** reconhecimento facial em qualquer tablet ou celular,
com anti-spoofing para prevenir fraudes e log auditável com foto
para conferência.

**Modo totem fixo:**
- Tablet fixo na parede da entrada da empresa
- Loop automático de captura e reconhecimento
- Funciona como totem sem app instalado (PWA)
- Gestor configura o local no painel

**Modo app (futuro pós-MVP):**
- Funcionário instala app no próprio celular
- Vínculo dispositivo-funcionário feito pelo admin via código temporário
- Reconhecimento facial + GPS valida presença no local correto
- Geofence configurável por localização de trabalho

**Diferenciais:**
- Log de auditoria com foto de cada registro (24h)
- Gestor visualiza feed em tempo real de entradas e saídas
- Anti-spoofing impede fraude com foto de colega
- Sem contrato de fidelidade

**Posição legal:**
Posicionado como sistema de gestão de frequência auxiliar, não como
REP homologado (Portaria 671/MTE). Indicado para empresas até 20
funcionários (sem obrigação de REP) ou em empresas com acordo
coletivo REP-A. Ver seção 17 para detalhes.

### 4.3 FaceAPI

API REST para desenvolvedores com dois endpoints principais:

`POST /api/v1/recognize` — identifica uma pessoa em uma imagem
contra uma lista de candidatos fornecida pelo cliente.

`POST /api/v1/embeddings/register` — gera e retorna embedding
criptografado de uma face para armazenamento pelo cliente.

Autenticação por API Key. Rate limiting por plano. Documentação
Swagger gerada automaticamente. Webhooks para notificação assíncrona
(pós-MVP).

---

## 5. Arquitetura do sistema

```
Internet
    │
    ▼
┌─────────────────────────────────────────────────────┐
│                    Nginx                            │
│         reverse proxy · SSL · rate limit            │
└──────┬──────────────────────────┬───────────────────┘
       │                          │
       ▼                          ▼
┌─────────────┐          ┌─────────────────────┐
│   Next.js   │          │    Node.js API       │
│  porta 3000 │          │    porta 4000        │
│             │          │                      │
│  Dashboard  │          │  • JWT auth          │
│  Super Admin│          │  • tenant isolation  │
│  Totem PWA  │◀────SSE──│  • warm cache logic  │
│  App Mobile │          │  • pre-attendance    │
│  (PWA)      │          │  • audit log save    │
└─────────────┘          └──────┬───────┬───────┘
                                │       │
                    ┌───────────┘       └──────────────┐
                    │                                   │
                    ▼                                   ▼
         ┌──────────────────┐              ┌───────────────────┐
         │   PostgreSQL 16  │              │    Redis 7        │
         │                  │              │                   │
         │  • todos os      │              │  • sessões JWT    │
         │    dados         │              │  • warm cache     │
         │    persistentes  │              │    embeddings     │
         │  • embeddings    │              │  • pré-presenças  │
         │    AES-256-GCM   │              │  • sem persistên- │
         │  • audit log     │              │    cia em disco   │
         │    metadata      │              └───────────────────┘
         └──────────────────┘
                    │
                    │ HTTP interno
                    ▼
         ┌──────────────────────────────────────┐
         │         Engine Python                │
         │         FastAPI · porta 8001         │
         │                                      │
         │  • SCRFD — detecção facial           │
         │  • MiniFASNet v1+v2 — liveness       │
         │  • ArcFace R50 — embedding 512d      │
         │  • AES-256-GCM — decrypt em RAM      │
         │  • comparação NumPy vetorizada       │
         │  • cache em memória (dict Python)    │
         │  • audit frames em disco (24h)       │
         │                                      │
         │  workers=2 · modelos em memória      │
         │  zero acesso ao PostgreSQL           │
         └──────────────────────────────────────┘
                    │
                    ▼
         ┌──────────────────────────────────────┐
         │     Audit Frames (filesystem)        │
         │                                      │
         │  ./audit_frames/                     │
         │    {tenant_id}/                      │
         │      {YYYY-MM-DD}/                   │
         │        {timestamp}_{status}_{id}.jpg │
         │                                      │
         │  TTL: 24 horas                       │
         │  Job de limpeza: diário às 03:00     │
         │  Acesso: apenas gestor do tenant     │
         └──────────────────────────────────────┘
```

### Decisões arquiteturais importantes

**Engine sem banco de dados.** A engine Python nunca conecta ao
PostgreSQL. Recebe embeddings criptografados no body do request
(enviados pelo Node), descriptografa em RAM, compara e retorna.
Isso elimina dependência de banco no caminho crítico de performance.

**Cache em memória na engine.** Um dicionário Python
`{tenant_id: {person_id: ndarray}}` mantém embeddings já
descriptografados em RAM entre requisições. Após o primeiro
request de uma turma/turno, as requisições seguintes têm zero
custo de I/O e zero custo de descriptografia.

**Redis sem persistência.** Configurado com `--save ""` e
`--appendonly no`. Dados em Redis são sempre reconstituíveis
pelo Node — se cair, o Node re-popula. Isso garante que
embeddings descriptografados nunca tocam disco fora da engine.

**Warm cache proativo.** Quando uma aula abre ou um turno começa,
o Node popula o cache da engine com os embeddings de todos os
candidatos antes do primeiro reconhecimento. Elimina latência
de cache miss no pico de chegada.

**Workers=2 Uvicorn.** Para o MVP com até 700 usuários em pico
distribuídos em tenants, 2 workers processando ~13 reconhecimentos
por segundo cada são suficientes. Cada worker carrega os modelos
em memória independentemente (~800MB por worker = ~1.6GB total).

---

## 6. Engine de reconhecimento facial

### Modelos utilizados

**SCRFD (parte do buffalo_s InsightFace)**
Detector de faces rápido e preciso. Detecta faces em imagens de
até 640px, retorna bounding box e 5 keypoints para alinhamento.
Descarta detecções com score abaixo de 0.75.
Latência: ~30ms por frame.

**ArcFace R50 (parte do buffalo_s InsightFace)**
Extrator de embeddings de 512 dimensões. Embeddings são
normalizados L2 automaticamente pelo modelo (||e|| = 1).
Treinado em datasets de dezenas de milhões de faces.
Latência: ~80ms por frame.

**MiniFASNet v1 + v2 (Silent-Face-Anti-Spoofing)**
Dois modelos de liveness detection com escalas diferentes.
Ver seção 7 para detalhes completos.

### Serialização de embeddings

O embedding é um `np.ndarray` de shape `(512,)` dtype `float32`.

Serialização: `embedding.astype(np.float32).tobytes()` → 2048 bytes.
Deserialização: `np.frombuffer(plaintext, dtype=np.float32).copy()`.

Não usar base64 — a conversão bytes→base64→bytes é desnecessária
e adiciona ~33% de tamanho sem benefício.

### Criptografia AES-256-GCM

```
Chave: 32 bytes (256 bits) carregada como hex de 64 chars do env
IV: 12 bytes gerados com os.urandom(12) por operação — nunca reutilizar
Plaintext: 2048 bytes do embedding serializado
Output AESGCM.encrypt: ciphertext + tag (tag = últimos 16 bytes)

Armazenamento no banco:
  embedding_iv  BYTEA  — 12 bytes
  embedding_tag BYTEA  — 16 bytes
  embedding_ct  BYTEA  — 2048 bytes
```

A tag de autenticação GCM garante integridade — se o ciphertext
for adulterado no banco, a descriptografia lança `InvalidTag`.
Nunca capturar esse erro silenciosamente.

### Comparação vetorizada

Como embeddings ArcFace são normalizados L2:

```python
# candidate_matrix: shape (N, 512)
# query_embedding: shape (512,)
similarities = candidate_matrix @ query_embedding     # (N,)
distances = np.sqrt(2 * (1 - np.clip(similarities, -1, 1)))  # (N,)
best_idx = np.argmin(distances)
```

Para N=300 candidatos, a operação completa leva < 1ms em NumPy.
Threshold padrão: 1.10 (configurável por tenant em tenant_settings).

Conversão para confidence score (0.0 a 1.0):
```python
confidence = max(0.0, 1.0 - (best_distance / 1.5))
```

### Pipeline completo (ordem com early exit)

```
1. Decodificar JPEG/PNG via OpenCV
   → HTTP 400 se inválido

2. Redimensionar para máximo 640px no maior lado
   → preserva aspect ratio

3. SCRFD detecta faces
   → retorna no_face_detected se ausente
   → seleciona face com maior bounding box (mais próxima)
   → descarta se det_score < 0.75

4. MiniFASNet v1+v2 (liveness)
   → retorna liveness_failed se score < threshold
   → inclui liveness_score no response sempre

5. Análise HSV de saturação (detecção de tela)
   → não bloqueia sozinha
   → penaliza se saturação alta + liveness_score próximo do limite

6. ArcFace R50 extrai embedding 512d

7. Resolve candidatos:
   a. busca no cache em memória (dict Python)
   b. para ausentes: descriptografa embeddings recebidos no request
   c. insere novos no cache

8. Comparação vetorizada NumPy

9. Salva audit frame em disco (fire-and-forget, não bloqueia)

10. Retorna resultado
```

### Contratos da engine

**POST /recognize**

Request:
```json
{
  "tenant_id": "string",
  "image_b64": "string (JPEG base64)",
  "candidates": [
    {
      "person_id": 1,
      "embedding_iv": "hex",
      "embedding_tag": "hex",
      "embedding_ct": "hex"
    }
  ]
}
```

Response (identificado):
```json
{
  "status": "identified",
  "person_id": 1,
  "confidence": 0.87,
  "liveness_score": 0.94,
  "saturation_mean": 82.3,
  "processing_ms": 183
}
```

Response (outros casos):
```json
{
  "status": "not_identified | liveness_failed | no_face_detected",
  "best_distance": 1.23,
  "liveness_score": 0.31,
  "saturation_mean": 142.7,
  "processing_ms": 95
}
```

**POST /embeddings/register**

Recebe imagem multipart, detecta face, extrai embedding,
criptografa e retorna para o Node persistir no PostgreSQL.
A engine não persiste nada.

Response:
```json
{
  "person_id": 1,
  "embedding_iv": "hex",
  "embedding_tag": "hex",
  "embedding_ct": "hex",
  "model_version": "arcface_r50_v1"
}
```

**DELETE /embeddings/{tenant_id}/{person_id}**

Invalida entrada no cache em memória.
Chamado pelo Node quando usuário é removido ou embedding atualizado.

**POST /cache/warm**

Recebe lista de candidatos com embeddings criptografados,
descriptografa e popula cache. Chamado pelo Node ao abrir aula
ou iniciar turno, antes do primeiro reconhecimento.

**GET /health**

```json
{
  "status": "ok",
  "models": {
    "detector": true,
    "embedder": true,
    "liveness_v1": true,
    "liveness_v2": true
  }
}
```

---

## 7. Anti-spoofing

O sistema usa quatro camadas complementares. Nenhuma camada
isolada é infalível — a combinação cobre os ataques casuais
que representam 99% dos casos reais em ambiente corporativo.

### Camada 1 — Variação de pixels (frontend, ~5ms)

**O que detecta:** imagens estáticas — fotos impressas e telas
que não variam entre frames.

**Como funciona:** o frontend captura 3 frames com 250ms de
intervalo (total 500ms), sem nenhum roundtrip de rede. Compara
a diferença absoluta média de pixels entre o frame 1 e o frame 3
numa versão reduzida a 160×120 pixels em escala de cinza. Se a
diferença for menor que o threshold (3.0), descarta silenciosamente
e continua o loop. Se passar, envia o frame do meio para a engine.

**Threshold:** 3.0 (diferença média por pixel em escala 0–255).
Calibrar empiricamente no ambiente real. Típico:
- foto impressa: 0.1–0.5
- tela estática: 0.5–1.5
- pessoa real: 3.0–15.0

**Limitação:** vídeo em loop passa por essa camada.

### Camada 2 — MiniFASNet v1+v2 (engine, ~40ms)

**O que detecta:** fotos impressas, fotos em tela de celular,
máscaras 2D e ataques de replay básicos.

**Como funciona:** dois modelos ONNX treinados especificamente
para anti-spoofing, em escalas diferentes da região da face.

- v1 (escala 2.7x): analisa textura em área menor, captura
  padrões de poros e reflexos de pele
- v2 (escala 4.0x): analisa textura em área maior ao redor
  da face, captura padrões que fotos não reproduzem

Crop para cada modelo:
```
cx, cy = centro do bounding box
w = largura_bbox * escala
h = altura_bbox * escala
crop = frame[cy-h/2:cy+h/2, cx-w/2:cx+w/2]
crop = resize(crop, 80x80)
```

Score final: `v1 * 0.4 + v2 * 0.6`
Threshold padrão: 0.85 (configurável por tenant).

**Pesos ONNX:** baixar de `minivision-lab/Silent-Face-Anti-Spoofing`
no GitHub. Arquivos: `2.7_80x80_MiniFASNetV1.onnx` e
`4_0_80x80_MiniFASNetV2.onnx`.

**Limitação:** foto de alta qualidade em papel fotográfico
com iluminação controlada pode passar. Raro em ambiente real.

### Camada 3 — Análise de saturação HSV (engine, ~5ms)

**O que detecta:** telas de dispositivos com saturação de cor
artificialmente alta.

**Como funciona:** converte crop da face para HSV, analisa o
canal de saturação S. Telas tendem a ter saturação média alta
(>140) com desvio padrão baixo (<45). Pele real tem distribuição
de saturação mais variada.

Não bloqueia sozinha — penaliza o resultado final se:
`saturation_mean > 140 AND saturation_std < 45 AND liveness_score < threshold * 1.2`

**Limitação:** telas de alta qualidade com modo noturno ativado
podem não ser detectadas. Câmeras com saturação aumentada
podem gerar falsos positivos.

### Camada 4 — Log auditável com foto (deterrência, ~0ms no caminho crítico)

**O que faz:** salva o frame capturado junto com os scores de
cada registro. O gestor pode auditar visualmente casos suspeitos.

**Por que funciona:** a maioria das fraudes de ponto são oportunistas.
Saber que existe um registro fotográfico de cada batida inibe a
tentativa antes de acontecer. É a camada mais eficaz em termos
de custo-benefício.

**Implementação:** fire-and-forget com `asyncio.create_task`.
Não bloqueia o pipeline de reconhecimento. Se falhar o salvamento,
loga o erro mas não falha o reconhecimento.

Ver seção 8 para detalhes completos do sistema de auditoria.

### Fluxo encadeado das camadas

```
Frontend (tablet/celular):
  Captura frame 1 → aguarda 250ms
  Captura frame 2 → aguarda 250ms
  Captura frame 3
    │
    ▼
  [Camada 1] Variação de pixels (5ms)
    ├── < 3.0 → descarta silencioso → reinicia loop
    └── ≥ 3.0 → envia frame 2 para engine

Engine Python:
    │
    ▼
  SCRFD detecta face
    ├── sem face → no_face_detected
    └── face detectada
          │
          ▼
        [Camada 2] MiniFASNet v1+v2 (40ms)
          ├── score < 0.85 → liveness_failed
          └── score ≥ 0.85
                │
                ▼
              [Camada 3] Análise HSV (5ms)
                │ (não bloqueia — contribui para penalização)
                │
                ▼
              ArcFace embedding → comparação
                │
                ▼
              [Camada 4] Salva audit frame (async)
                │
                ▼
              Retorna resultado
```

**Latência total para reconhecimento legítimo:**
~500ms captura + ~200ms engine = ~700ms ponta a ponta.

---

## 8. Logs de auditoria e LGPD

### O que é armazenado

Para cada tentativa de reconhecimento (bem-sucedida ou não),
o sistema salva:

**No PostgreSQL (permanente):**
- `recognition_logs`: status, person_id, confidence, liveness_score,
  saturation_mean, processing_ms, device_id, tenant_id, timestamp
- Referência ao arquivo de imagem: `audit_frame_path` (nullable)

**No filesystem (temporário — 24h):**
- Frame JPEG da captura, qualidade 70
- Organizado em `./audit_frames/{tenant_id}/{YYYY-MM-DD}/`
- Nome: `{timestamp_ms}_{status}_{person_id_ou_unknown}.jpg`

### Por que 24 horas é o prazo correto

Um dia cobre o ciclo completo de trabalho — o gestor pode auditar
qualquer registro do dia antes do encerramento do expediente. É
o prazo mínimo necessário para a finalidade declarada e o máximo
defensável sob o princípio de necessidade da LGPD.

Manter mais que 24 horas não agrega valor para a finalidade de
auditoria de fraude diária e aumenta desnecessariamente o risco
de vazamento.

### Job de limpeza automática

Roda todos os dias às 03:00 (horário do servidor, configurável).
Deleta todos os arquivos em `./audit_frames/` com mais de 24 horas.
Atualiza `audit_frame_path` para NULL nos registros correspondentes
em `recognition_logs`.

A exclusão é **comprovável** — o job gera um log de quantos arquivos
foram deletados por tenant, que fica registrado em
`audit_cleanup_logs` no PostgreSQL.

### Base legal LGPD

**Base legal principal:** legítimo interesse do controlador
(empresa cliente / tenant), combinado com execução do contrato
de trabalho.

**Finalidade:** exclusivamente auditoria de fraude de ponto para
o dia corrente. Não inclui treinamento de modelos, análise
comportamental, ou qualquer finalidade secundária.

**Necessidade:** dados mínimos (um frame por registro, qualidade
reduzida para 70% JPEG, descarte automático em 24h).

**Transparência:** obrigatório constar na política de privacidade
do tenant que imagens faciais são capturadas no registro de ponto,
retidas por até 24 horas para fins de auditoria de fraude, e
descartadas automaticamente.

**Acesso restrito:** apenas usuários com role `admin` ou `manager`
do tenant podem visualizar os audit frames. O operador do SaaS
(você) não acessa imagens de tenants. Isso deve estar explícito
no contrato com o tenant e na política de privacidade.

**Consentimento do funcionário:** não é necessário consentimento
explícito separado se o uso estiver descrito na política de uso
do sistema apresentada pelo RH na admissão. Mas precisa estar
documentado que o funcionário foi informado.

### O que você (operador) não pode fazer com as imagens

- Usar para treinar ou melhorar modelos de reconhecimento facial
- Compartilhar com terceiros além do tenant contratante
- Manter além do prazo de 24 horas
- Usar para qualquer finalidade além de auditoria de fraude

### Implementação técnica da restrição de acesso

O endpoint de visualização de audit frames verifica:
1. JWT válido
2. `role IN ('admin', 'manager')`
3. `tenant_id` do JWT bate com o `tenant_id` do frame solicitado

Qualquer divergência retorna HTTP 403. O path do arquivo nunca
é exposto diretamente — o Node serve o arquivo via stream
após validar as três condições acima.

---

## 9. Estrutura de dados completa

### Schema PostgreSQL

Todas as tabelas incluem `created_at TIMESTAMPTZ DEFAULT NOW()`
e `updated_at TIMESTAMPTZ DEFAULT NOW()` salvo indicação contrária.

---

#### Domínio: planos e tenants

```sql
CREATE TABLE plans (
  id              SERIAL PRIMARY KEY,
  name            VARCHAR(100) NOT NULL,
  product_type    VARCHAR(20) NOT NULL
                  CHECK (product_type IN ('attendance','timeclock','both','api')),
  max_users       INT NOT NULL DEFAULT 30,
  price_monthly   NUMERIC(10,2) NOT NULL,
  is_active       BOOLEAN NOT NULL DEFAULT true,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE tenants (
  id              SERIAL PRIMARY KEY,
  plan_id         INT NOT NULL REFERENCES plans(id),
  name            VARCHAR(200) NOT NULL,
  document        VARCHAR(20),           -- CNPJ ou CPF
  type            VARCHAR(20) NOT NULL
                  CHECK (type IN ('school','company')),
  status          VARCHAR(20) NOT NULL DEFAULT 'trial'
                  CHECK (status IN ('active','suspended','trial','cancelled')),
  trial_ends_at   TIMESTAMPTZ,
  timezone        VARCHAR(50) NOT NULL DEFAULT 'America/Sao_Paulo',
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE tenant_settings (
  tenant_id                   INT PRIMARY KEY REFERENCES tenants(id),
  recognition_threshold       FLOAT NOT NULL DEFAULT 1.10,
  liveness_threshold          FLOAT NOT NULL DEFAULT 0.85,
  liveness_enabled            BOOLEAN NOT NULL DEFAULT true,
  static_pixel_threshold      FLOAT NOT NULL DEFAULT 3.0,
  geofence_radius_meters      INT NOT NULL DEFAULT 150,
  allow_manual_override       BOOLEAN NOT NULL DEFAULT true,
  pre_attendance_window_min   INT NOT NULL DEFAULT 30,
  audit_retention_hours       INT NOT NULL DEFAULT 24,
  updated_at                  TIMESTAMPTZ DEFAULT NOW()
);
```

---

#### Domínio: usuários e autenticação

```sql
CREATE TABLE users (
  id              SERIAL PRIMARY KEY,
  tenant_id       INT NOT NULL REFERENCES tenants(id),
  name            VARCHAR(200) NOT NULL,
  email           VARCHAR(200),
  password_hash   VARCHAR(255),          -- NULL para funcionários sem acesso ao painel
  role            VARCHAR(20) NOT NULL
                  CHECK (role IN (
                    'super_admin','admin','manager',
                    'teacher','employee'
                  )),
  status          VARCHAR(20) NOT NULL DEFAULT 'active'
                  CHECK (status IN ('active','inactive','pending')),
  document        VARCHAR(20),           -- CPF
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  updated_at      TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE (tenant_id, email)
);

CREATE TABLE facial_embeddings (
  id              BIGSERIAL PRIMARY KEY,
  tenant_id       INT NOT NULL,
  user_id         INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  embedding_iv    BYTEA NOT NULL,        -- 12 bytes
  embedding_tag   BYTEA NOT NULL,        -- 16 bytes
  embedding_ct    BYTEA NOT NULL,        -- 2048 bytes (512 float32)
  model_version   VARCHAR(50) NOT NULL DEFAULT 'arcface_r50_v1',
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE (user_id, model_version)
);

CREATE TABLE devices (
  id                  SERIAL PRIMARY KEY,
  tenant_id           INT NOT NULL REFERENCES tenants(id),
  user_id             INT REFERENCES users(id),  -- NULL = totem sem vínculo pessoal
  device_fingerprint  VARCHAR(255) NOT NULL,
  device_type         VARCHAR(20) NOT NULL
                      CHECK (device_type IN ('app','totem')),
  label               VARCHAR(100),              -- "Entrada principal", "Celular João"
  api_key_hash        VARCHAR(255) NOT NULL,
  is_active           BOOLEAN NOT NULL DEFAULT true,
  last_seen_at        TIMESTAMPTZ,
  bound_at            TIMESTAMPTZ,
  created_at          TIMESTAMPTZ DEFAULT NOW(),
  UNIQUE (tenant_id, device_fingerprint)
);

CREATE TABLE device_bind_codes (
  id          SERIAL PRIMARY KEY,
  tenant_id   INT NOT NULL REFERENCES tenants(id),
  user_id     INT NOT NULL REFERENCES users(id),
  code        VARCHAR(6) NOT NULL,
  expires_at  TIMESTAMPTZ NOT NULL,
  used        BOOLEAN NOT NULL DEFAULT false,
  created_at  TIMESTAMPTZ DEFAULT NOW()
);
```

---

#### Domínio: localização e geofences

```sql
CREATE TABLE locations (
  id              SERIAL PRIMARY KEY,
  tenant_id       INT NOT NULL REFERENCES tenants(id),
  name            VARCHAR(100) NOT NULL,
  address         VARCHAR(300),
  latitude        FLOAT,
  longitude       FLOAT,
  radius_meters   INT NOT NULL DEFAULT 150,
  is_active       BOOLEAN NOT NULL DEFAULT true,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE user_locations (
  user_id         INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  location_id     INT NOT NULL REFERENCES locations(id) ON DELETE CASCADE,
  PRIMARY KEY (user_id, location_id)
);
```

---

#### Domínio: FacePresença (escolas)

```sql
CREATE TABLE academic_periods (
  id              SERIAL PRIMARY KEY,
  tenant_id       INT NOT NULL REFERENCES tenants(id),
  name            VARCHAR(100) NOT NULL,   -- "2026/1"
  starts_at       DATE NOT NULL,
  ends_at         DATE NOT NULL,
  is_active       BOOLEAN NOT NULL DEFAULT true,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE courses (
  id                  SERIAL PRIMARY KEY,
  tenant_id           INT NOT NULL REFERENCES tenants(id),
  academic_period_id  INT NOT NULL REFERENCES academic_periods(id),
  name                VARCHAR(200) NOT NULL,
  created_at          TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE classes (
  id              SERIAL PRIMARY KEY,
  tenant_id       INT NOT NULL REFERENCES tenants(id),
  course_id       INT NOT NULL REFERENCES courses(id),
  name            VARCHAR(100) NOT NULL,   -- "Turma A — Noturno"
  teacher_id      INT NOT NULL REFERENCES users(id),
  room            VARCHAR(50),
  location_id     INT REFERENCES locations(id),
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE class_enrollments (
  class_id        INT NOT NULL REFERENCES classes(id) ON DELETE CASCADE,
  user_id         INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  enrolled_at     TIMESTAMPTZ DEFAULT NOW(),
  PRIMARY KEY (class_id, user_id)
);

CREATE TABLE class_sessions (
  id              BIGSERIAL PRIMARY KEY,
  tenant_id       INT NOT NULL REFERENCES tenants(id),
  class_id        INT NOT NULL REFERENCES classes(id),
  teacher_id      INT NOT NULL REFERENCES users(id),
  started_at      TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  ended_at        TIMESTAMPTZ,
  status          VARCHAR(20) NOT NULL DEFAULT 'open'
                  CHECK (status IN ('open','closed')),
  created_by      INT NOT NULL REFERENCES users(id)
);

CREATE TABLE attendance_records (
  id                  BIGSERIAL PRIMARY KEY,
  tenant_id           INT NOT NULL,
  class_session_id    BIGINT NOT NULL REFERENCES class_sessions(id),
  user_id             INT NOT NULL REFERENCES users(id),
  status              VARCHAR(20) NOT NULL
                      CHECK (status IN ('present','absent','manual')),
  recognized_at       TIMESTAMPTZ,
  confidence          FLOAT,
  liveness_score      FLOAT,
  was_pre_attendance  BOOLEAN NOT NULL DEFAULT false,
  adjusted_by         INT REFERENCES users(id),
  adjusted_at         TIMESTAMPTZ,
  UNIQUE (class_session_id, user_id)
);

CREATE TABLE pre_attendances (
  id              BIGSERIAL PRIMARY KEY,
  tenant_id       INT NOT NULL,
  location_id     INT NOT NULL REFERENCES locations(id),
  user_id         INT NOT NULL REFERENCES users(id),
  recognized_at   TIMESTAMPTZ NOT NULL,
  confidence      FLOAT NOT NULL,
  liveness_score  FLOAT NOT NULL,
  expires_at      TIMESTAMPTZ NOT NULL,
  consolidated    BOOLEAN NOT NULL DEFAULT false,
  created_at      TIMESTAMPTZ DEFAULT NOW(),
  -- impede duplicata: só a primeira chegada por local por dia conta
  UNIQUE (tenant_id, location_id, user_id, date(recognized_at))
);
```

---

#### Domínio: FacePonto (empresas)

```sql
CREATE TABLE work_schedules (
  id              SERIAL PRIMARY KEY,
  tenant_id       INT NOT NULL REFERENCES tenants(id),
  name            VARCHAR(100) NOT NULL,   -- "Comercial 8h-17h"
  -- [{weekday: 1-7, start_time: "08:00", end_time: "17:00",
  --   tolerance_minutes: 10}]
  entries         JSONB NOT NULL,
  created_at      TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE user_schedules (
  user_id             INT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
  work_schedule_id    INT NOT NULL REFERENCES work_schedules(id),
  starts_at           DATE NOT NULL,
  ends_at             DATE,              -- NULL = indefinido
  PRIMARY KEY (user_id, work_schedule_id, starts_at)
);

CREATE TABLE punch_records (
  id                      BIGSERIAL PRIMARY KEY,
  tenant_id               INT NOT NULL,
  user_id                 INT NOT NULL REFERENCES users(id),
  device_id               INT NOT NULL REFERENCES devices(id),
  location_id             INT REFERENCES locations(id),
  type                    VARCHAR(20) NOT NULL
                          CHECK (type IN ('clock_in','clock_out')),
  punched_at              TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  latitude                FLOAT,
  longitude               FLOAT,
  gps_accuracy_meters     FLOAT,
  geofence_valid          BOOLEAN,
  recognition_confidence  FLOAT NOT NULL,
  liveness_score          FLOAT NOT NULL,
  source                  VARCHAR(20) NOT NULL
                          CHECK (source IN ('app','totem')),
  is_manual               BOOLEAN NOT NULL DEFAULT false,
  manual_reason           TEXT,
  adjusted_by             INT REFERENCES users(id),
  adjusted_at             TIMESTAMPTZ
);
```

---

#### Domínio: reconhecimento e auditoria

```sql
CREATE TABLE recognition_logs (
  id                  BIGSERIAL PRIMARY KEY,
  tenant_id           INT NOT NULL,
  device_id           INT REFERENCES devices(id),
  user_id             INT REFERENCES users(id),  -- NULL se não identificado
  status              VARCHAR(30) NOT NULL
                      CHECK (status IN (
                        'identified','not_identified',
                        'liveness_failed','no_face_detected'
                      )),
  confidence          FLOAT,
  liveness_score      FLOAT,
  saturation_mean     FLOAT,
  processing_ms       INT,
  source_product      VARCHAR(20)
                      CHECK (source_product IN ('attendance','timeclock','api')),
  -- path relativo em ./audit_frames/ — NULL após exclusão
  audit_frame_path    VARCHAR(500),
  -- true enquanto arquivo existir em disco
  audit_frame_exists  BOOLEAN NOT NULL DEFAULT false,
  created_at          TIMESTAMPTZ DEFAULT NOW()
);

CREATE TABLE audit_cleanup_logs (
  id                  SERIAL PRIMARY KEY,
  run_at              TIMESTAMPTZ NOT NULL DEFAULT NOW(),
  files_deleted       INT NOT NULL DEFAULT 0,
  bytes_freed         BIGINT NOT NULL DEFAULT 0,
  tenants_processed   INT NOT NULL DEFAULT 0,
  error_message       TEXT        -- NULL se sucesso
);
```

---

#### Índices críticos

```sql
-- Lookup por API Key (hot path de toda requisição de dispositivo)
CREATE INDEX ON devices (api_key_hash);
CREATE INDEX ON devices (tenant_id, is_active);

-- Queries de presença
CREATE INDEX ON users (tenant_id, status);
CREATE INDEX ON facial_embeddings (tenant_id, user_id);
CREATE INDEX ON class_sessions (tenant_id, status, started_at DESC);
CREATE INDEX ON attendance_records (tenant_id, class_session_id);
CREATE INDEX ON pre_attendances (tenant_id, location_id, consolidated)
  WHERE consolidated = false;

-- Queries de ponto
CREATE INDEX ON punch_records (tenant_id, user_id, punched_at DESC);
CREATE INDEX ON punch_records (tenant_id, punched_at DESC);

-- Auditoria
CREATE INDEX ON recognition_logs (tenant_id, created_at DESC);
CREATE INDEX ON recognition_logs (tenant_id, audit_frame_exists)
  WHERE audit_frame_exists = true;
```

---

## 10. Multi-tenancy

### Estratégia no MVP

Isolamento por `tenant_id` em todas as tabelas. Sem schema
separado por tenant — isso simplifica queries e migrações no MVP.
Cada query obrigatoriamente inclui `WHERE tenant_id = $1`, injetado
pelo Node a partir do JWT.

O JWT do usuário contém:
```json
{
  "sub": "user_id",
  "tenant_id": "123",
  "role": "admin",
  "exp": 1713480000
}
```

A API Key de dispositivos tem o `tenant_id` encoded no payload
(não apenas no hash) para que o middleware extraia sem consulta
adicional ao banco na maioria das rotas.

### Middleware de tenant isolation

Todo request autenticado passa por um middleware que:
1. Extrai `tenant_id` do JWT ou da API Key
2. Injeta em `req.tenantId` e `req.userId`
3. Verifica se o tenant está `active` (cache Redis de 60s)
4. Bloqueia se `suspended` ou `cancelled`

Nenhum controller acessa dados de outro tenant — a validação
é feita no middleware, não espalhada pelo código.

### Roadmap para isolamento completo

Pós-MVP: migrar para schemas PostgreSQL separados por tenant.
Cada tenant terá seu próprio schema com as mesmas tabelas.
Isso provê isolamento de performance (índices menores) e
facilita exportação/exclusão completa de dados de um tenant
(LGPD — direito ao esquecimento).

---

## 11. Fluxos principais

### 11.1 Cadastro de usuário com embedding facial

```
Admin acessa painel → cadastra dados do usuário
    │
    ▼
Admin faz upload de foto ou captura via totem/webcam
    │
    ▼
Node envia imagem para POST /embeddings/register na engine
    │
    ▼
Engine: SCRFD detecta face → ArcFace extrai embedding 512d
→ AES-GCM criptografa → retorna (iv, tag, ct) em hex
    │
    ▼
Node salva em facial_embeddings no PostgreSQL
    │
    ▼
Node invalida cache da engine: DELETE /embeddings/{tenant}/{user}
    │
    ▼
Confirmação ao admin
```

### 11.2 Abertura de aula e warm cache (FacePresença)

```
Professor clica "Abrir Aula" no painel
    │
    ▼
Node cria class_session no PostgreSQL (status: 'open')
    │
    ▼
Node busca alunos da turma (class_enrollments JOIN facial_embeddings)
    │
    ▼
Node envia POST /cache/warm com candidatos + embeddings criptografados
Engine descriptografa e popula cache em memória
    │
    ▼
Node busca pré-presenças no Redis:
  SCAN prepresenca:{tenant_id}:{location_id}:*
    │
    ▼
Para cada pré-presença:
  valida se user está na turma
  valida se timestamp está dentro da janela (tenant_settings.pre_attendance_window_min)
  se válida: INSERT attendance_records (was_pre_attendance: true)
  DEL chave do Redis
    │
    ▼
Emite SSE para o totem: aula aberta, lista de candidatos atualizada
```

### 11.3 Reconhecimento no totem — loop completo

```
Totem (browser/PWA):
  Captura frame 1 → aguarda 250ms
  Captura frame 2 → aguarda 250ms
  Captura frame 3
    │
    ▼
  JS calcula variação de pixels (160×120 grayscale)
    ├── < 3.0 → descarta → aguarda 500ms → reinicia
    └── ≥ 3.0 → envia frame 2 via POST /device/recognize
                header: X-Device-Key: {api_key}

Node:
  Valida API Key → extrai tenant_id e device_id
  Identifica location_id do dispositivo
  Busca candidate_ids (turma ativa ou todos da empresa)
  Busca embeddings de facial_embeddings WHERE user_id = ANY(candidate_ids)
  Chama engine POST /recognize com candidatos

Engine:
  SCRFD → liveness MiniFASNet → HSV → ArcFace → compara cache
  Salva audit frame (async)
  Retorna resultado

Node:
  Se identified:
    Verifica se já existe registro para esse user nessa session
    Se não: INSERT attendance_record OU punch_record
    Emite SSE para painel: {user_id, name, confidence, timestamp}
  Se liveness_failed:
    Salva recognition_log com status liveness_failed
    Não emite SSE visível (opcional: alerta discreto para gestor)
  Se not_identified:
    Salva recognition_log
    Sem ação

Totem:
  Se identified: exibe nome + foto por 3s → cooldown 4s → reinicia
  Se outros: reinicia imediatamente
```

### 11.4 Pré-presença (FacePresença)

```
Reconhecimento bem-sucedido mas sem class_session ativa:

Node verifica: SELECT * FROM class_sessions
  WHERE tenant_id = $1 AND status = 'open'
  AND EXISTS (
    SELECT 1 FROM classes c
    JOIN class_enrollments ce ON ce.class_id = c.id
    WHERE c.location_id = $2 AND ce.user_id = $3
  )
→ nenhuma sessão ativa encontrada

Node verifica se já existe pré-presença para esse user+location hoje
→ SET NX em Redis: prepresenca:{tenant}:{location}:{user_id} = timestamp
→ TTL: pre_attendance_window_min * 60 segundos
→ Se já existe: ignora (preserva timestamp mais antigo)
```

### 11.5 Vínculo de dispositivo

```
Admin acessa painel → "Vincular dispositivo"
→ Seleciona funcionário → gera código de 6 dígitos (válido 5 min)
→ INSERT device_bind_codes

Funcionário abre app no celular
→ App gera device_fingerprint local (hash de características do dispositivo)
→ App exibe tela de código → usuário digita código fornecido pelo admin

App POST /devices/bind {code, device_fingerprint, device_type: 'app'}
→ Node valida código (não expirado, não usado)
→ Node cria registro em devices
→ Node gera API Key exclusiva → retorna ao app
→ App armazena API Key localmente
→ Node marca código como used: true

Admin configura geofences permitidas para o funcionário
```

### 11.6 Bater ponto pelo totem (FacePonto)

```
Funcionário para na frente do totem
→ Loop de captura detecta face com variação ≥ 3.0
→ Envia para engine via Node

Node:
  Valida API Key do totem → obtém location_id
  Busca TODOS os funcionários ativos do tenant com embedding cadastrado
  Chama engine

Engine:
  Pipeline completo → retorna identified com person_id

Node:
  Verifica último punch_record do funcionário (clock_in ou clock_out?)
  Alterna: se último foi clock_in → clock_out, e vice-versa
  INSERT punch_record
  Emite SSE para painel do gestor
```

---

## 12. Stack tecnológica

### Backend (Node.js)

```
Node.js 20 LTS
Express 4
PostgreSQL driver: node-postgres (pg) com pool
Redis client: ioredis
JWT: jsonwebtoken
Validação: zod
Senhas: bcrypt
Variáveis: dotenv
Logs: pino
```

### Frontend (Next.js)

```
Next.js 14 (App Router)
TypeScript
Tailwind CSS
ShadCN/UI para componentes do dashboard
Canvas API nativa para captura e análise de frames
EventSource (SSE) para feed em tempo real
React Hook Form + zod para formulários
```

### Engine facial (Python)

```
Python 3.11+
FastAPI + Uvicorn (workers=2)
InsightFace 0.7.3 (buffalo_s: SCRFD + ArcFace R50)
OpenCV (opencv-python-headless)
NumPy
cryptography (AESGCM)
onnxruntime (MiniFASNet)
pydantic + pydantic-settings
```

### Infraestrutura

```
PostgreSQL 16
Redis 7 Alpine (sem persistência em disco)
Nginx (reverse proxy + SSL)
Docker + Docker Compose
VPS: mínimo 4 vCPUs, 4GB RAM (2 workers Python ~1.6GB modelos)
```

---

## 13. Infraestrutura e deploy

### Docker Compose completo

```yaml
version: '3.9'

services:
  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf:ro
      - ./ssl:/etc/ssl:ro
    depends_on:
      - nextjs
      - node-api

  nextjs:
    build: ./frontend
    environment:
      - NEXT_PUBLIC_API_URL=https://seudominio.com/api
    depends_on:
      - node-api

  node-api:
    build: ./backend
    environment:
      - DATABASE_URL=postgresql://user:pass@postgres:5432/faceid
      - REDIS_URL=redis://redis:6379
      - ENGINE_URL=http://face-engine:8001
      - ENGINE_API_KEY=${ENGINE_API_KEY}
      - JWT_SECRET=${JWT_SECRET}
      - NODE_ENV=production
    depends_on:
      - postgres
      - redis
      - face-engine

  face-engine:
    build: ./face_engine
    environment:
      - AES_KEY=${AES_KEY}
      - ENGINE_API_KEY=${ENGINE_API_KEY}
      - RECOGNITION_THRESHOLD=1.10
      - LIVENESS_THRESHOLD=0.85
      - STATIC_PIXEL_THRESHOLD=3.0
      - LIVENESS_MODEL_PATH=./models
    volumes:
      - audit_frames:/app/audit_frames
    # workers=2: dobra throughput, cada worker ~800MB RAM de modelos
    # NÃO aumentar sem migrar cache para Redis
    command: uvicorn main:app --host 0.0.0.0 --port 8001 --workers 2

  postgres:
    image: postgres:16-alpine
    environment:
      - POSTGRES_DB=faceid
      - POSTGRES_USER=${POSTGRES_USER}
      - POSTGRES_PASSWORD=${POSTGRES_PASSWORD}
    volumes:
      - pgdata:/var/lib/postgresql/data
      - ./migrations:/docker-entrypoint-initdb.d

  redis:
    image: redis:7-alpine
    # Sem persistência — dados são sempre reconstituíveis
    command: redis-server --save "" --appendonly no --maxmemory 256mb --maxmemory-policy allkeys-lru

volumes:
  pgdata:
  audit_frames:
```

### Job de limpeza de audit frames

Cron dentro do container node-api, todo dia às 03:00:

```
0 3 * * * node /app/jobs/cleanup-audit-frames.js
```

O job:
1. Lista todos os arquivos em `./audit_frames/` com mais de 24h
2. Deleta os arquivos
3. Atualiza `audit_frame_exists = false` e `audit_frame_path = NULL`
   nos recognition_logs correspondentes
4. Insere registro em `audit_cleanup_logs`

### Variáveis de ambiente obrigatórias

```env
# PostgreSQL
POSTGRES_USER=
POSTGRES_PASSWORD=
DATABASE_URL=

# Redis
REDIS_URL=

# Engine
ENGINE_API_KEY=          # string aleatória longa
AES_KEY=                 # 64 chars hexadecimal (256 bits)

# JWT
JWT_SECRET=              # string aleatória longa
JWT_EXPIRES_IN=8h

# Reconhecimento
RECOGNITION_THRESHOLD=1.10
LIVENESS_THRESHOLD=0.85
STATIC_PIXEL_THRESHOLD=3.0
```

---

## 14. Rotas da API Node.js

```
── Autenticação ──────────────────────────────────────────
POST   /api/auth/login
POST   /api/auth/refresh
POST   /api/auth/logout

── Super Admin ───────────────────────────────────────────
GET    /api/admin/tenants
POST   /api/admin/tenants
PATCH  /api/admin/tenants/:id
GET    /api/admin/tenants/:id/stats
GET    /api/admin/plans
POST   /api/admin/plans
GET    /api/admin/recognition-logs          -- logs globais
GET    /api/admin/health                    -- saúde da engine

── Tenant — usuários ─────────────────────────────────────
GET    /api/users
POST   /api/users
GET    /api/users/:id
PATCH  /api/users/:id
DELETE /api/users/:id
POST   /api/users/:id/photo                 -- cadastra embedding
DELETE /api/users/:id/embedding             -- remove embedding + invalida cache

── Tenant — dispositivos ─────────────────────────────────
GET    /api/devices
POST   /api/devices/bind-code              -- gera código 6 dígitos
POST   /api/devices/bind                   -- confirma vínculo
PATCH  /api/devices/:id
DELETE /api/devices/:id

── Tenant — localizações ─────────────────────────────────
GET    /api/locations
POST   /api/locations
PATCH  /api/locations/:id
DELETE /api/locations/:id

── Tenant — configurações ────────────────────────────────
GET    /api/settings
PATCH  /api/settings                       -- thresholds, janela pré-presença, etc.

── FacePresença — estrutura acadêmica ────────────────────
GET    /api/attendance/periods
POST   /api/attendance/periods
GET    /api/attendance/courses
POST   /api/attendance/courses
GET    /api/attendance/classes
POST   /api/attendance/classes
GET    /api/attendance/classes/:id/students
POST   /api/attendance/classes/:id/students    -- matrícula
DELETE /api/attendance/classes/:id/students/:userId

── FacePresença — sessões e presenças ────────────────────
GET    /api/attendance/sessions
POST   /api/attendance/sessions                -- abre aula (+ warm cache)
GET    /api/attendance/sessions/:id
PATCH  /api/attendance/sessions/:id/close      -- fecha aula
GET    /api/attendance/sessions/:id/records
PATCH  /api/attendance/records/:id             -- ajuste manual

── FacePresença — relatórios ─────────────────────────────
GET    /api/attendance/reports/frequency       -- ?class_id&period_id
GET    /api/attendance/reports/student/:id     -- histórico do aluno

── FacePonto — escalas ───────────────────────────────────
GET    /api/timeclock/schedules
POST   /api/timeclock/schedules
PATCH  /api/timeclock/schedules/:id
POST   /api/timeclock/users/:id/schedule       -- vincula escala ao funcionário

── FacePonto — registros ─────────────────────────────────
GET    /api/timeclock/records                  -- ?user_id&date_from&date_to
PATCH  /api/timeclock/records/:id              -- ajuste manual com justificativa
GET    /api/timeclock/feed                     -- SSE: feed em tempo real

── FacePonto — relatórios ────────────────────────────────
GET    /api/timeclock/reports/daily            -- ?date&location_id
GET    /api/timeclock/reports/monthly          -- ?month&user_id
GET    /api/timeclock/reports/export           -- CSV

── Auditoria ─────────────────────────────────────────────
GET    /api/audit/recognition-logs             -- ?date&status&user_id
GET    /api/audit/frames/:recognition_log_id   -- serve o JPEG via stream
                                               -- requer role admin/manager
                                               -- valida tenant_id

── Dispositivo (autenticado por X-Device-Key) ────────────
POST   /api/device/recognize                   -- totem solicita reconhecimento
POST   /api/device/punch                       -- app mobile bate ponto
GET    /api/device/session/current             -- sessão ativa na sala do totem
GET    /api/device/stream                      -- SSE: eventos para o totem
GET    /api/device/heartbeat                   -- atualiza last_seen_at
```

---

## 15. Superfícies do sistema

### Super Admin Dashboard

Acessível apenas para `role: super_admin`. Mostra:
- Lista de todos os tenants com status, plano, MRR, usuários ativos
- Volume de reconhecimentos por tenant (últimos 7 dias)
- Saúde da engine (GET /health)
- Gestão de planos e limites
- Logs de auditoria globais (sem acesso a frames de tenants)
- Ações: suspender tenant, alterar plano, resetar API Keys

### Tenant Dashboard — FacePresença

Acessível para `admin`, `manager`, `teacher`.

**Admin/Manager:**
- Cadastro de alunos com foto
- Gestão de turmas, cursos e períodos letivos
- Configurações de threshold e janela de pré-presença
- Relatórios de frequência
- Log de reconhecimentos do dia

**Teacher (professor):**
- Abre e fecha aulas
- Vê lista de presença em tempo real (SSE)
- Ajusta presenças manualmente

### Tenant Dashboard — FacePonto

Acessível para `admin`, `manager`.

- Cadastro de funcionários com foto
- Gestão de escalas e localizações/geofences
- Feed em tempo real de entradas e saídas (SSE)
- Relatório diário e mensal
- Gestão de dispositivos vinculados
- Ajuste manual de ponto com justificativa obrigatória
- Log de reconhecimentos com acesso a audit frames (últimas 24h)

### Totem PWA

URL única por tenant + location:
`https://seudominio.com/totem/{device_token}`

- Abre câmera automaticamente no carregamento
- Mostra instrução: "Olhe para a câmera"
- Loop automático de captura (sem interação do usuário)
- Exibe nome e foto do reconhecido por 3 segundos
- Indicador de status de conexão SSE
- Sem login — autenticado pela URL com token do dispositivo
- Funciona em qualquer browser moderno (Chrome Android recomendado)

### App Mobile (PWA mobile — MVP)

Acessível via browser do celular do funcionário.
URL: `https://seudominio.com/app`

- Tela de vínculo: insere código fornecido pelo admin
- Tela de cadastro de selfie
- Tela principal: botão "Bater Ponto" centralizado
  - Captura selfie + GPS ao pressionar
  - Exibe resultado (presente/recusado) com feedback visual
- Histórico dos últimos 7 registros
- Sem acesso ao painel de gestão

---

## 16. Roadmap pós-MVP

### Fase 2 (3–6 meses pós-hackathon)

- App mobile nativo (React Native) com modo Lock Task (quiosque Android)
- BullMQ para filas assíncronas de reconhecimento
- Redis para warm cache (substituindo cache em memória da engine)
- Schema PostgreSQL separado por tenant (isolamento completo)
- Webhooks para integração com sistemas de RH externos
- FaceAPI v1 pública com portal de desenvolvedor e documentação

### Fase 3 (6–12 meses)

- Homologação como REP-A (Portaria 671) com advogado trabalhista
- Integração com folhas de pagamento (Gupy, Totvs, Senior)
- Relatórios fiscais no formato exigido pelo MTE
- Anti-spoofing de segunda geração (análise de Fourier + depth sensor)
- Multi-região (latência < 100ms para qualquer estado brasileiro)

### Fase 4 (12+ meses)

- FaceAPI enterprise com SLA garantido
- White-label para revendedores
- Módulo de controle de acesso (integração com fechaduras inteligentes)
- Análise de comportamento e detecção de anomalias

---

## 17. Considerações legais e trabalhistas

### Portaria 671/MTE e REP

A Portaria 671 de 2021 regula o REP (Registrador Eletrônico de Ponto)
e exige homologação INMETRO para empresas com mais de 20 empregados
sujeitas ao controle de jornada pela CLT.

O FaceID SaaS **não é posicionado como REP homologado** no MVP.
É um sistema de gestão de frequência auxiliar — exatamente como
os concorrentes mobile do mercado operam.

**Segmentos sem restrição:**
- Empresas até 20 funcionários (sem obrigação de REP)
- Funcionários em regime PJ ou autônomo
- Empresas com acordo coletivo REP-A
- Escolas e instituições educacionais (não sujeitas à Portaria 671)
- Startups e empresas de tecnologia (muitas têm acordo coletivo flexível)

**Segmento com risco (evitar no início):**
- Grandes empresas CLT sem acordo coletivo e com fiscalização ativa do MTE

### LGPD — dados biométricos

Embeddings faciais são dados biométricos sensíveis sob a LGPD.

**Obrigações do operador (você):**
- Documentar finalidade, base legal e prazo de retenção de cada dado
- Garantir exclusão automática dos audit frames após 24h
- Restringir acesso a dados biométricos ao mínimo necessário
- Não usar dados de tenants para treinar modelos sem consentimento explícito
- Disponibilizar mecanismo de exclusão de dados por solicitação do titular

**Obrigações do controlador (tenant/empresa cliente):**
- Informar funcionários sobre captura de dados biométricos
- Obter consentimento documentado ou ter base legal adequada
- Incluir o uso do sistema na política de privacidade da empresa
- Responder solicitações de titulares (acesso, exclusão, portabilidade)

**Contrato com tenants:**
O contrato de prestação de serviços deve estabelecer claramente
que o tenant é o controlador dos dados de seus funcionários/alunos
e você é o operador. Isso delimita as responsabilidades de cada parte.

### Consentimento para audit frames

O funcionário precisa ser informado que:
- Sua imagem facial é capturada a cada registro de ponto
- A imagem é retida por até 24 horas para fins de auditoria de fraude
- Após 24 horas a imagem é excluída automaticamente
- Apenas o gestor responsável pode visualizar as imagens dentro do prazo

Essa informação deve constar em documento assinado pelo funcionário
no momento do cadastro no sistema (de responsabilidade do tenant).
