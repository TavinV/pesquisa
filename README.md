# Plano Intensivo — Auxiliar de TI (14 Dias)
## Versão Otimizada para Mogi das Cruzes

---

## Objetivo Final
Preparar seu amigo para **passar na entrevista** da vaga de Auxiliar de TI, não apenas "aprender coisas".

Foco: **Comunicação (40%) + Capacidade técnica (40%) + Atitude (20%)**

---

## Perfil da Vaga (Relembrando)
- **Local:** Mogi das Cruzes (próximo a ele!)
- **Stack primário:** Windows + Office/Google Sheets + APIs básicas
- **Responsabilidades:** Suporte, APIs, scripts, atendimento L1, infraestrutura
- **Diferencial:** Python, APIs, Webhooks
- **Contratam por:** Comunicação clara, vontade de aprender, organização

---

## Por Que Este Plano é Diferente

| Aspecto | Plano Original | Novo Plano |
|---------|---|---|
| **Foco** | Aprender o máximo | Passar na entrevista |
| **Comunicação** | 30 min no Dia 14 | Dia inteiro (Dia 13) |
| **Atendimento L1** | 1 dia teórico | 2 dias 100% role-play |
| **Projetos** | 1 projeto grande | 2 projetos pequenos |
| **Linux** | Dia 6 full | Condensado Dia 10 |
| **APIs** | Dia 5 | Dia 3 (mais cedo) |
| **Currículo** | Mencionado ao final | Cria no Dia 0 |

---

# ANTES DE COMEÇAR — DIA 0

## Preparação (Sábado ou Domingo antes da Semana 1)

### Criar Currículo Básico
- **Arquivo:** Word ou Google Docs
- **Estrutura:**
  ```
  NOME
  Telefone | Email | LinkedIn
  
  FORMAÇÃO
  Cursando ADS — UMC
  
  HABILIDADES (vai preenchendo ao longo dos 14 dias)
  - Lógica de programação
  - Python básico
  - APIs e Webhooks
  
  PROJETOS (vai preenchendo conforme faz)
  
  EXPERIÊNCIA
  (Deixa em branco por enquanto ou coloca projetos acadêmicos)
  ```
- **Objetivo:** Ter uma estrutura pronta. Não precisa ser perfeito agora.

### Instalar Ferramentas
1. **Python 3.11+** (python.org)
2. **VS Code** (code.visualstudio.com)
3. **Postman** (postman.com) — versão web ou desktop
4. **Git** (git-scm.com)
5. **Um navegador** (Chrome é suficiente)

### Criar uma Pasta do Projeto
```
meu-estudo-ti/
├── dia-1-logica/
├── dia-2-python/
├── dia-3-apis/
└── ... (vai criando conforme avança)
```

**Tempo total:** 1-2 horas. Não pule isso.

---

# SEMANA 1 — FUNDAÇÃO TÉCNICA + APLICAÇÃO

## Estrutura Diária
- **Manhã (2h):** Aprender (leitura + vídeos)
- **Tarde (2-3h):** Praticar (exercícios + código)
- **Noite (30 min):** Revisar + documentar o que aprendeu

---

## DIA 1 — Segunda-feira
### Lógica de Programação Básica

**Conceito:** Problema → Solução → Código

**Aprender (1h30)**
- Variáveis (o que são, por que usamos)
- Condições (`if`, `else`)
- Loops (`for`, `while`)
- Funções (entrada → processamento → saída)

**Recursos:**
- YouTube: "Lógica de programação para iniciantes" (Curso em Vídeo)
- ~45 minutos de vídeo

**Exercícios Práticos (2h30)**
1. **Calculadora simples**
   - Entrada: dois números
   - Saída: soma, subtração, multiplicação, divisão
   - Reeescrever mentalmente: "Recebo dois números. Verifico qual operação o usuário quer. Faço a conta. Mostro o resultado."

2. **Número par ou ímpar**
   - Entrada: um número
   - Saída: "é par" ou "é ímpar"
   - Pensar: "Como verifico? Divido por 2 e vejo se sobra resto."

3. **Média de 3 notas**
   - Entrada: 3 notas
   - Saída: média + "aprovado/reprovado" (média ≥ 6)

4. **Tabuada (loop)**
   - Entrada: um número
   - Saída: 1 × número, 2 × número, ... 10 × número

**Escrever em pseudocódigo primeiro, não em linguagem de programação real.**

**Objetivo do dia:**
Seu amigo consegue **explicar em voz alta** como resolver cada problema. Escrever em pseudocódigo ou papel.

**Pergunta de validação:** "Me explica como você resolveria o problema da tabuada?"

---

## DIA 2 — Terça-feira
### Python Básico (Implementar a Lógica)

**Aprender (1h30)**
- `print()` — exibir texto
- `input()` — receber texto do usuário
- `if`, `else`, `elif` — condições
- `for`, `while` — loops
- `def` — funções
- Listas `[]` — armazenar múltiplos valores
- Indentação (é obrigatória em Python, não é opcional)

**Recursos:**
- YouTube: "Python para iniciantes - aula 1-5" (Guanabara ou Curso em Vídeo)
- ~60 minutos de vídeo
- Se quiser praticar online: **Exercism.io** (Python track, problemas fáceis)

**Exercícios Práticos (2h30)**
Reimplementar os exercícios do Dia 1 **em Python de verdade**.

1. **Calculadora em Python**
   ```python
   # Seu amigo escreve algo assim
   numero1 = int(input("Primeiro número: "))
   numero2 = int(input("Segundo número: "))
   soma = numero1 + numero2
   print(f"A soma é {soma}")
   ```

2. **Sistema de Login (novo)**
   - Usuário e senha hardcoded (é Dia 2, não precisa DB)
   - Se acertar: "Bem-vindo!"
   - Se errar: "Acesso negado"

3. **Agenda de Contatos (novo)**
   - Adicionar nome, telefone
   - Listar contatos
   - Sair
   - Usar `while True` + menu

4. **Lista de Tarefas (novo)**
   - Adicionar tarefa
   - Marcar como pronta
   - Remover tarefa
   - Listar

**Validação:** Todos os 4 scripts rodando e funcionando.

**Salvando no git:**
```bash
cd meu-estudo-ti
git init
git add dia-2-python/
git commit -m "Dia 2: Exercícios Python básico"
```

---

## DIA 3 — Quarta-feira
### APIs e Webhooks (Conceito + Prática)

**Por que no Dia 3?** Porque a vaga pede "APIs e Webhooks" como diferencial. Colocar cedo mostra que seu amigo estuda coisas que importam.

**Aprender (1h30)**
- O que é uma API?
- GET e POST (os dois métodos principais)
- O que é JSON?
- O que é um Webhook?
- Cliente vs Servidor

**Recursos:**
- Vídeo: "APIs para iniciantes em 10 minutos" (DevXP ou similar)
- Ler: https://www.postman.com/resources/postman-student-program/ (tem tutorial grátis)

**Exercícios Práticos (2h30)**

### Parte 1: Postman (sem código)
1. **Fazer requisições GET**
   - URL: `https://reqres.in/api/users?page=2`
   - Ver resposta em JSON
   - Entender o que significa cada campo

2. **Fazer requisições POST**
   - URL: `https://reqres.in/api/users`
   - Body (JSON): `{ "name": "João", "job": "Developer" }`
   - Ver a resposta (geralmente um ID)

3. **Testar Webhook**
   - Ir para: `https://webhook.site/`
   - Copiar o URL único
   - Fazer uma requisição POST para esse URL (no Postman)
   - Ver a requisição aparecer em webhook.site em tempo real

### Parte 2: Python + Requests
```python
import requests

# GET
resposta = requests.get("https://reqres.in/api/users")
print(resposta.json())

# POST
dados = {"name": "João", "job": "Developer"}
resposta = requests.post("https://reqres.in/api/users", json=dados)
print(resposta.json())
```

**Desafio:**
Criar um script Python que:
1. Faz um GET em `https://reqres.in/api/users`
2. Imprime o nome e email de cada usuário

**Validação:** Script funcionando e explicação: "Uma API é como um menu de restaurante. A gente pede (GET/POST) e ela responde (JSON)."

---

## DIA 4 — Quinta-feira
### Suporte Técnico (Fundamentos Práticos)

**Aprender (1h30)**
- CPU, RAM, SSD, HD (diferenças)
- Windows vs Linux (visão geral)
- IP, DNS, Rede local (conceitos básicos)
- Compartilhamento de pasta
- Impressoras

**Recursos:**
- YouTube: "Fundamentos de TI para iniciantes" (~60 min)
- Leitura rápida: O que é um IP? O que é DNS?

**Exercícios Práticos (2h30)**

### Se seu amigo tiver acesso a um PC ou VM:
1. **Instalar um programa** (Firefox, VS Code, etc)
2. **Desinstalar um programa** (Painel de Controle)
3. **Criar uma pasta compartilhada**
   - Pasta > Propriedades > Compartilhamento
   - Compartilhar com outro usuário na rede
4. **Verificar IP do PC**
   - `ipconfig` no CMD
   - Anotar IP local, gateway
5. **Testar conexão de rede**
   - `ping 8.8.8.8` (Google)
   - Entender o que é latência

### Se usar VM (VirtualBox):
1. Instalar Windows 10/11 em VM (tem tutorial no YouTube)
2. Fazer os 5 exercícios acima dentro da VM

**Validação:** Consegue responder:
- "Qual é o seu IP?"
- "Como você instalaria um programa?"
- "Como compartilharia uma pasta?"

---

## DIA 5 — Sexta-feira
### Projeto 1: Sistema de Cadastro de Usuários

**Objetivo:** Consolidar Dias 1-4 em um projeto real (pequeno).

**Projeto:**
```
Sistema de Cadastro
├── Cadastrar novo usuário (nome, email, telefone)
├── Listar usuários
├── Buscar usuário por nome
├── Salvar em arquivo (usuários.json ou .txt)
└── Menu com opções (enquanto não digita "sair")
```

**Stack:**
- Python puro (sem frameworks)
- Arquivo `.json` como "banco de dados"

**Estrutura:**
```python
import json

# Função para cadastrar
def cadastrar_usuario(nome, email, telefone):
    usuario = {"nome": nome, "email": email, "telefone": telefone}
    usuarios.append(usuario)
    salvar_em_arquivo()

# Função para listar
def listar_usuarios():
    for usuario in usuarios:
        print(f"{usuario['nome']} - {usuario['email']}")

# Função para salvar em arquivo
def salvar_em_arquivo():
    with open("usuarios.json", "w") as f:
        json.dump(usuarios, f, indent=2)

# Função para carregar do arquivo
def carregar_do_arquivo():
    # ... lógica aqui ...
    pass

# Menu principal
while True:
    print("\n=== CADASTRO ===")
    print("1. Cadastrar")
    print("2. Listar")
    print("3. Sair")
    opcao = input("Escolha: ")
    
    if opcao == "1":
        nome = input("Nome: ")
        email = input("Email: ")
        telefone = input("Telefone: ")
        cadastrar_usuario(nome, email, telefone)
    elif opcao == "2":
        listar_usuarios()
    elif opcao == "3":
        break
```

**Tempo:** 3-4 horas (incluindo testes)

**Validação:**
- Rodar o programa
- Adicionar 2-3 usuários
- Fechar o programa
- Abrir de novo: os usuários ainda estão lá?
- Listar todos

**Para documentar:**
- Escrever um README.md explicando como rodar
- Commitar no git

```bash
git add dia-5-projeto-1/
git commit -m "Projeto 1: Sistema de cadastro"
```

**Atualizar currículo:**
Adicionar na seção "PROJETOS":
```
Sistema de Cadastro de Usuários (Python)
- Funcionalidade de criar, listar e persistir usuários em arquivo JSON
- Menu interativo com validações básicas
```

---

## FIM DE SEMANA — Revisão (Sábado)

**Não é para estudar coisas novas.**

**Atividade:** Revisar mentalmente Dias 1-5.

1. **Escreva tudo que aprendeu em uma página** (formato livre)
   - Lógica: o que é, exemplo
   - Python: print, input, if, for, def
   - APIs: O que é, GET, POST
   - TI: CPU, RAM, IP, DNS
   - Projeto 1: O que fez

2. **Assista à própria explicação em voz alta**
   - Grave um vídeo de 5 minutos explicando um dos tópicos
   - Assista depois
   - Note onde ficou confuso

3. **Prépare respostas para Dia 8**
   - "Como você instalaria um programa?"
   - "O que é uma API?"
   - "Como você resolveria 'internet caiu'?"

**Não estude:** Não faça exercícios novos, não abra novos tópicos.

---

# SEMANA 2 — FOCO VAGA + ENTREVISTA

## DIA 6 — Segunda-feira
### Excel / Google Sheets (Básico)

**Aprender (1h)**
- SOMA()
- MÉDIA()
- SE() (if-then-else)
- PROCV() / XLOOKUP()
- Filtros automáticos
- Tabelas dinâmicas (visão geral)

**Recursos:**
- YouTube: "Excel para iniciantes" (~45 min)
- Google Sheets é quase idêntico

**Exercícios Práticos (2h)**

1. **Planilha de Gastos**
   - Coluna A: Data
   - Coluna B: Descrição
   - Coluna C: Valor
   - Fórmula: Total com SOMA()
   - Fórmula: Média com MÉDIA()

2. **Planilha de Vendas (PROCV)**
   - Tabela 1: ID do Produto, Nome, Preço
   - Tabela 2: ID do Produto, Quantidade
   - Fórmula em Tabela 2: PROCV para buscar o Preço
   - Coluna de Total: Preço × Quantidade

3. **Planilha de Chamados (para Dia 9-10)**
   - Colunas: ID, Cliente, Problema, Status (Aberto/Fechado), Prioridade
   - Filtro automático por Status
   - Filtro automático por Prioridade
   - SOMA de "Chamados em aberto"

**Validação:** Planilha de chamados funcionando com filtros.

---

## DIA 7 — Terça-feira
### Segurança da Informação + Backup

**Aprender (1h30)**
- Phishing (o que é, como reconhecer)
- Senha forte (critérios)
- Backup (por que é importante, quando fazer)
- Ransomware (visão geral)
- MFA (autenticação multi-fator)

**Recursos:**
- YouTube: "Segurança da informação para iniciantes" (~45 min)
- Artigo: LGPD básico (porque a empresa trabalha com biometria)

**Exercícios Práticos (2h)**

1. **Análise de senhas**
   - Seu amigo recebe 5 senhas (ficcionais)
   - Ele avalia cada uma: forte ou fraca? Por quê?
   - Exemplo: "123456" = fraca, "Abc@2024Xyz" = forte

2. **Identificar phishing**
   - Você manda 3 emails ficcionais
   - Ele identifica qual é phishing
   - Explica: "Seria suspeito porque..."

3. **Plano de backup**
   - Seu amigo cria um "plano de backup" fictício para uma empresa
   - Onde guardar? Frequência? Como testar?

4. **Criar uma senha forte**
   - Para cada cenário: Banco, Email corporativo, Notebook pessoal
   - Deve ser diferente para cada

**Validação:** Consegue explicar por que LGPD importa para uma empresa que trabalha com reconhecimento facial.

**Nota:** Isso é importante porque a vaga (VisageX) trabalha com dados biométricos.

---

## DIA 8 — Quarta-feira
### Atendimento Nível 1 — Role-play (Dia 1)

**Estrutura:** Seu amigo **faz** o papel de Auxiliar de TI, você faz o papel de **usuário com problema**.

**Não é leitura, é atuação.**

### Roteiros (você manda mensagem, liga, ou escreve):

**Cenário 1: "Meu Windows está lento"**
- Você: "Oi, meu notebook está ridiculamente lento, não consigo fazer nada!"
- Seu amigo precisa:
  - Ouvir o problema com paciência
  - Perguntar: "Desde quando?" "Abri algum programa novo?" "Tem muito arquivo?"
  - Sugerir: Verificar Task Manager (Ctrl+Shift+Esc), ver qual programa está usando muita CPU/RAM
  - Se não funcionar: Reiniciar
  - Explicar em linguagem simples (ele para explicar para você como se você não entendesse de TI)

**Cenário 2: "Office não abre"**
- Você: "Cliquei no Word e nada acontece. Odeio esse computador!"
- Seu amigo precisa:
  - Perguntar: "Que mensagem de erro você vê?"
  - Se vir erro: Pesquisar a mensagem online (Google)
  - Se não vir nada: Tentar reiniciar o programa
  - Última opção: Desinstalar e reinstalar

**Cenário 3: "Não consigo conectar na impressora"**
- Você: "Preciso imprimir um documento agora! A impressora não funciona!"
- Seu amigo precisa:
  - Perguntar: "Você consegue ver a impressora no seu PC?" (Configurações > Dispositivos > Impressoras)
  - Se sim: Tentar imprimir um teste
  - Se não: Instalar o driver (site do fabricante)
  - Verificar cabo/WiFi da impressora

**Tempo:** 30 min por cenário (3 cenários = 1h30)

**Gravação:** Grave em áudio ou vídeo (WhatsApp, Zoom, OBS).

**Depois:** Escuta a gravação junto com você. O que foi bom? O que precisa melhorar?

**Feedback points:**
- ✓ Ouviu pacientemente?
- ✓ Fez perguntas para entender?
- ✓ Explicou de forma simples?
- ✓ Sugeriu próximos passos?
- ✗ Ficou impaciente?
- ✗ Usou jargão que o usuário não entendeu?

---

## DIA 9 — Quinta-feira
### Atendimento Nível 1 — Role-play (Dia 2)

**Mesma estrutura do Dia 8, novos cenários.**

**Cenário 4: "Meu email não sincroniza"**
- Você: "Não tô recebendo email há horas! É urgente!"
- Seu amigo precisa:
  - Perguntar: Qual email? (Gmail, Outlook, Corporativo?)
  - Perguntar: Consegue acessar pelo navegador?
  - Se sim no navegador mas não no app: Verificar se senha está correta no app
  - Se não no navegador também: Problema é do servidor, nada a fazer

**Cenário 5: "Esqueci minha senha"**
- Você: "Não lembro minha senha do Windows! Como faço?"
- Seu amigo precisa:
  - Se for conta Microsoft: Recovery email/phone
  - Se for conta local Windows: Mais complicado, precisa de reset disk (explicar que deveria ter feito antes)
  - Sugerir: "Vamos tentar recuperar com seu email"

**Cenário 6: "Não consigo acessar a pasta compartilhada"**
- Você: "Meus colegas conseguem acessar a pasta, mas eu não!"
- Seu amigo precisa:
  - Perguntar: "Qual é o endereço da pasta?" (ex: \\servidor\departamento)
  - Verificar se ele está conectado na rede certa
  - Verificar permissões (pode precisar de admin)
  - Sugerir: "Talvez você precise pedir permissão ao TI"

**Cenário 7: "Meu antivírus diz que tenho um malware"**
- Você: "Meu PC está com malware! O que faço?"
- Seu amigo precisa:
  - Não entrar em pânico
  - Se for falso positivo (ad pop-up): Ignorar
  - Se for de verdade: Fazer scan, remover
  - Sugerir: "Não baixe arquivos de sites estranhos"

**Tempo:** 2-3 horas (escutando, ajustando, re-fazendo)

**Feedback final:**
- Qual cenário ele foi mais natural?
- Qual precisa treinar mais?

---

## DIA 10 — Sexta-feira
### Linux Básico + Automação Python

**Aprender (1h30)**

**Linux: O essencial**
- O que é Linux?
- Terminal (cmd para Linux)
- Navegação: `pwd`, `cd`, `ls`, `mkdir`, `rm`, `cp`, `mv`
- Ler arquivo: `cat`
- Permissões: `chmod +x` (essencial)

**Recursos:**
- YouTube: "Linux para iniciantes" (~45 min, focar em comandos básicos)

**Exercícios Práticos (2h30)**

### Parte 1: Terminal Linux
Se seu amigo tiver WSL (Windows Subsystem for Linux) ou acesso a Linux:

```bash
pwd                          # Ver pasta atual
cd /home                      # Navegar
mkdir projetos               # Criar pasta
cd projetos
ls -la                       # Listar com detalhes
touch arquivo.txt            # Criar arquivo
cat arquivo.txt              # Ver conteúdo
cp arquivo.txt arquivo2.txt  # Copiar
mv arquivo.txt backup/       # Mover
rm arquivo.txt               # Deletar
```

### Parte 2: Automação Python (Integração)
Criar um script que:
1. Lê um arquivo `.csv` (usando `pandas`)
2. Filtra dados (ex: só usuários ativos)
3. Salva em novo arquivo

```python
import pandas as pd

# Ler CSV
df = pd.read_csv("usuarios.csv")

# Filtrar (ex: status = "ativo")
ativos = df[df['status'] == 'ativo']

# Salvar novo CSV
ativos.to_csv("usuarios_ativos.csv", index=False)

print(f"Salvou {len(ativos)} usuários ativos")
```

**Desafio:** Seu amigo cria um script que lê `usuarios.csv` e retorna:
- Total de usuários
- Usuários por departamento
- Salva em um novo arquivo

**Validação:** Script funcionando + consegue explicar cada linha.

---

## DIA 11 — Sábado (Fim de semana)
### Projeto 2: Sistema de Chamados

**Objetivo:** Projeto mais próximo do dia a dia da vaga.

**Projeto:**
```
Sistema de Chamados
├── Abrir chamado (descrição, prioridade, cliente)
├── Listar chamados (todos, abertos, fechados)
├── Atualizar status (aberto → em andamento → fechado)
├── Buscar por ID ou cliente
├── Salvar em arquivo (chamados.json)
└── Gerar relatório (quantos abertos, quantos fechados, tempo médio)
```

**Stack:**
- Python
- JSON (arquivo)
- Funções organizadas

**Estrutura sugerida:**
```python
import json
from datetime import datetime

class Chamado:
    def __init__(self, id, cliente, descricao, prioridade, status="aberto"):
        self.id = id
        self.cliente = cliente
        self.descricao = descricao
        self.prioridade = prioridade
        self.status = status
        self.data_criacao = datetime.now().isoformat()

def criar_chamado(cliente, descricao, prioridade):
    # Lógica aqui

def listar_chamados(filtro=None):
    # Retorna todos ou filtrados por status

def atualizar_status(id_chamado, novo_status):
    # Muda status do chamado

def gerar_relatorio():
    # Mostra total abertos, fechados, etc

# Menu
while True:
    print("\n=== SISTEMA DE CHAMADOS ===")
    print("1. Abrir novo chamado")
    print("2. Listar chamados")
    print("3. Atualizar status")
    print("4. Gerar relatório")
    print("5. Sair")
    opcao = input("Escolha: ")
    # ...
```

**Tempo:** 3-4 horas

**Validação:**
- Criar 3 chamados
- Listar todos
- Mudar status de um para "em andamento"
- Mudar status de outro para "fechado"
- Gerar relatório: deve mostrar 1 aberto, 1 em andamento, 1 fechado

**Documentar:**
- README explicando como usar
- Escrever um "changelog" do que foi feito

**Atualizar currículo:**
```
Sistema de Chamados (Python)
- Criar, listar e atualizar chamados
- Persistência em JSON
- Relatório de status
- Demonstra organização e pensamento orientado ao problema
```

**Commitar:**
```bash
git add dia-11-projeto-2/
git commit -m "Projeto 2: Sistema de chamados"
```

---

## DIA 12 — Domingo (Fim de semana)
### Automação Prática + IA

**Aprender (1h)**
- ChatGPT para programação (como usar)
- Automação que você usaria no dia a dia (pyautogui, pandas)
- No-code (Zapier, IFTTT) — conceitual

**Exercícios Práticos (2h)**

### Exercício 1: Usar ChatGPT para gerar código
- Prompt: "Crie um script Python que lê um CSV e conta quantas linhas tem"
- Seu amigo copia, testa, adapta, entende

### Exercício 2: Automação com pyautogui (opcional, avançado)
```python
import pyautogui
import time

# Simula cliques e digitação
pyautogui.click(100, 100)  # Clica na posição (100, 100)
time.sleep(0.5)
pyautogui.typewrite('João')  # Digita texto
pyautogui.press('enter')
```

Usar isso para: Preencher um formulário repetitivo no navegador.

### Exercício 3: Gerar relatório em Excel com Python
```python
from openpyxl import Workbook

wb = Workbook()
ws = wb.active

ws['A1'] = 'Meu Relatório'
ws['A2'] = 'Chamados Fechados'
ws['A3'] = 10

wb.save('relatorio.xlsx')
```

**Desafio:** Criar um script que:
1. Lê `usuarios.csv`
2. Gera um relatório em Excel com:
   - Total de usuários
   - Usuários por status
   - Data do relatório

**Validação:** Relatório em `.xlsx` aberto no Excel, pronto para mostrar.

---

## DIA 13 — Segunda-feira
### Comunicação + Entrevista (DIA INTEIRO)

**Este é o dia mais importante. Não pule, não encurte.**

**Objetivo:** Treinar como se comportar em uma entrevista real.

**Estrutura: 5 horas**

### Bloco 1: Respostas Padrão (1h)

Seu amigo **memoriza e treina em voz alta** estas respostas:

**P: "Fale sobre você"**
> "Sou nome, tenho 18 anos, estou cursando ADS na UMC. Gosto de tecnologia e programação. Nos últimos 2 semanas estudei Python, APIs, suporte técnico e criei 2 projetos práticos: um sistema de cadastro e um sistema de chamados. Estou animado para trabalhar em uma empresa que faz facial recognition, porque acho importante entender como dados sensíveis são protegidos."

**P: "Por que quer trabalhar aqui?"**
> "Vocês trabalham com facial recognition e LGPD, que são áreas importantes. Quero aprender como implementar essas coisas com responsabilidade. Além disso, a vaga menciona APIs e Python, que estou estudando agora."

**P: "Você sabe Python?"**
> "Tenho conhecimento básico. Consigo ler código, entender o que está acontecendo, e escrever scripts simples. Criei alguns projetos práticos: um sistema que lê e escreve dados em JSON. Não sou experiente, mas aprender rápido é meu ponto forte."

**P: "Já trabalhou com APIs?"**
> "Estou estudando agora. Entendo o básico: como funciona GET, POST, JSON. Já consegui fazer requisições simples em Postman e em Python com a biblioteca requests. Tessei um consumo básico de API. Ainda tenho muito a aprender, mas tenho uma base sólida."

**P: "Como você lida com problemas que não sabe resolver?"**
> "Primeiro respiro e não entro em pânico. Segundo, pesquiso online — Google, documentação, Stack Overflow. Terceiro, se não conseguir, peço ajuda a alguém com experiência. E depois anoto o que aprendi para não esquecer. Às vezes o próprio usuário tem mais informação, então pergunto antes de supor."

**P: "Conte uma situação em que você cometeu um erro"**
> "Num projeto escolar, escrevi um código sem comentários. Quando voltei pra mexer semana depois, não entendia mais o que tinha feito (risos). Aprendi que documentação é importante. Agora sempre comento meu código e uso nomes de variáveis claros."

**P: "Como você se vê em 1 ano?"**
> "Trabalhando aqui, tendo aprendido muito mais sobre APIs, segurança e como sistemas de reconhecimento facial funcionam. Quero virar um Auxiliar de TI que as pessoas confiam e que consegue resolver problemas sem sempre pedir ajuda."

**P: "Tem alguma pergunta pra gente?"**
> (Sempre ter perguntas. Mostra interesse.)
> "Sim! Qual é a stack atual de vocês para os sistemas de APIs? E como é o processo de onboarding para um Auxiliar de TI novo? Qual é o maior desafio que o time enfrenta agora?"

### Bloco 2: Entrevista Simulada (2h)

**Prepare-se para ser entrevistador sério.**

Você faz o papel de gerente de RH ou tech lead. Use as perguntas acima + adicione:
- "Como você instalaria um programa?"
- "O que é uma API?"
- "Como você descobriria se um problema é do Windows ou da aplicação?"
- "Me mostra um dos teus projetos"

**Formato:**
- 45 min de entrevista (sim, real)
- 15 min de feedback imediato
- Grave em vídeo (sério)

**Entrevista simulada checklist:**
- [ ] Cumprimento e apresentação
- [ ] Respiração normal (não muito rápido)
- [ ] Contato visual (mesmo remotamente)
- [ ] Respostas são um parágrafo, não uma frase (mostra pensamento)
- [ ] Faz perguntas de volta
- [ ] Agradece ao final

### Bloco 3: Explicar Projetos (1h)

Seu amigo **explica em voz alta** os 2 projetos:

**Projeto 1: Sistema de Cadastro**
- "Criei um sistema que permite cadastrar usuários com nome, email e telefone. O programa tem um menu com opções de cadastrar, listar e sair. Os dados são salvos em um arquivo JSON, então quando você fecha o programa e abre de novo, os usuários ainda estão lá."
- Entrevistador pergunta: "Por que você usou JSON e não um banco de dados?"
- Resposta: "Era um projeto de aprendizado. JSON é mais simples para começar. Num projeto real, eu usaria um banco de dados como PostgreSQL."

**Projeto 2: Sistema de Chamados**
- "Criei um sistema de chamados mais completo. Tem funções para criar um chamado, listar (todos ou filtrados), atualizar o status, e gerar um relatório. Isso é bem parecido com o que vocês usam no dia a dia (SimpleDesk, Callix)."
- Entrevistador pergunta: "Como você trataria um chamado urgente?"
- Resposta: "A gente teria um campo de prioridade. Numa empresa real, chamados urgentes iam para o topo da fila e a gente tratava primeiro."

### Bloco 4: Revisar Gravação (1h)

Você assiste junto com ele.
- O que foi bem?
- O que precisa melhorar?
- Qual resposta foi mais natural?
- Qual ficou truncada?

**Re-grave se necessário.** Repita até ficar bom.

**Objetivo:** Que ele sinta-se confiante para a entrevista real.

---

## DIA 14 — Terça-feira
### Revisão Final + Entrevista Simulada 2 + Currículo

**Estrutura: 5 horas**

### Bloco 1: Revisão Mental (1h)

Seu amigo **não estuda nada novo.** Só revisa o que já sabe:
- Python: consegue criar um script simples?
- APIs: consegue fazer um GET e um POST?
- TI: consegue explicar CPU, RAM, IP?
- Projetos: consegue explicar o que fez?

### Bloco 2: Segunda Entrevista Simulada (2h)

**Mesmo formato do Dia 13, mas com perguntas diferentes.**

Desta vez, você foca em:
- Comportamento sob pressão ("Qual é o seu maior ponto fraco?")
- Trabalho em equipe ("Como você trabalha com pessoas mais experientes?")
- Criatividade ("Como você resolveria um problema que nunca viu?")

### Bloco 3: Currículo Final (1h)

**Versão final, pronta para enviar.**

Estrutura:
```
NOME COMPLETO
Email | Telefone | LinkedIn (opcional)

FORMAÇÃO
Cursando Análise e Desenvolvimento de Sistemas — UMC
Período: 2024-2026 (ou o período real)

HABILIDADES TÉCNICAS
- Python (básico): variáveis, funções, listas, manipulação de arquivos
- APIs e Webhooks: GET, POST, JSON, Postman
- Lógica de Programação: estruturas de controle, funções
- Suporte Técnico: Windows, Office, diagnóstico básico
- Ferramentas: Excel, Google Sheets, Git, VS Code
- Linux: comandos básicos (navegação, arquivo)

PROJETOS
Sistema de Cadastro de Usuários (Python)
• Criação, listagem e persistência de usuários em JSON
• Menu interativo com validações
• Repositório: github.com/seu-usuario/cadastro-usuarios

Sistema de Chamados (Python)
• Abertura, listagem e atualização de chamados
• Geração de relatórios de status
• Simulação de fluxo de suporte técnico
• Repositório: github.com/seu-usuario/sistema-chamados

EXPERIÊNCIA (Se tiver algo)
[Deixar em branco ou colocar projetos acadêmicos]

IDIOMA
Português: Nativo
Inglês: Básico (estudando)

OBSERVAÇÕES
Motivado a aprender rapidamente. Interesse específico em APIs, automação e segurança de dados.
```

**Antes de enviar:**
- Verificar erros de digitação
- Verificar que os projetos estão no GitHub (comitar tudo)
- Enviar para você revisar

### Bloco 4: Checklist Final (1h)

```
TÉCNICO
[ ] Consegue rodar os 2 projetos sem erro
[ ] Consegue explicar cada linha do código principal
[ ] Consegue responder "O que é uma API?"
[ ] Consegue explicar como instalaria um programa

COMUNICAÇÃO
[ ] Consegue fazer uma apresentação pessoal em 1 minuto
[ ] Consegue responder a "Por que quer trabalhar aqui?"
[ ] Consegue lidar com "Você não sabe X" de forma positiva
[ ] Consegue fazer perguntas inteligentes pro entrevistador

DOCUMENTAÇÃO
[ ] Currículo pronto
[ ] GitHub com 2 projetos
[ ] README em cada projeto
[ ] Conhece a URL do GitHub de cor

MENTALIDADE
[ ] Vontade de aprender (genuína)
[ ] Sabe o que é LGPD e por que importa
[ ] Conhece a vaga e a empresa (pesquisou)
[ ] Tem história pra contar ("Criei 2 projetos em 2 semanas")
```

---

# APÓS O DIA 14 — O Que Fazer

## Enviando o Currículo
- **Email:** curriculo@atualittarh.com.br
- **Assunto:** Auxiliar de TI – Mogi das Cruzes
- **Corpo:**
  ```
  Olá,
  
  Segue anexo meu currículo para a vaga de Auxiliar de TI em Mogi das Cruzes.
  
  Tenho interesse específico em trabalhar com APIs, automação e segurança de dados.
  
  Fico à disposição.
  
  Att,
  [Nome]
  ```
- **Anexo:** PDF do currículo

## Alternativas de Contato
- **Whatsapp:** (11) 3374-2443 (mensagem breve, profissional)
- **Telefone:** (11) 3374-2441 (chamada de 2-3 minutos apresentando-se)

## Depois que Enviar
- **Não fica ansioso.** Eles respondem em 3-5 dias.
- **Continue estudando.** Não para aqui. Aprofunda em:
  - Python
  - APIs (estude FastAPI ou Flask)
  - Banco de dados (PostgreSQL, MongoDB)
- **Conecta no LinkedIn** com a empresa e com pessoas da área de TI

---

# Resumo Visual: O Que Seu Amigo Vai Ter Depois de 14 Dias

| Item | Status |
|------|--------|
| **Lógica de Programação** | ✓ Entende bem |
| **Python Básico** | ✓ Consegue criar scripts simples |
| **APIs (GET/POST/JSON)** | ✓ Consegue usar Postman e Python |
| **Suporte Técnico** | ✓ Consegue diagnosticar problemas básicos |
| **Excel** | ✓ PROCV, SOMA, Filtros |
| **Linux** | ✓ Conhecimento básico (não é essencial) |
| **Segurança** | ✓ Entende phishing, senha, backup |
| **Git/GitHub** | ✓ Consegue commitar projetos |
| **2 Projetos** | ✓ No GitHub, rodando, documentados |
| **Entrevista** | ✓ Treinado, responde bem |
| **Currículo** | ✓ Pronto para enviar |

**O mais importante:** Ele não é um especialista em nada, mas consegue conversar sobre tudo um pouco. Isso é exatamente o que uma vaga de **Auxiliar de TI** procura.

---

# Dicas Finais para Você

1. **Acompanhamento diário**
   - Pergunta todo dia: "Como foi? O que aprendeu?"
   - Se tiver dúvida, ajuda com 5-10 minutos de dica, não resolve por ele

2. **Role-play é tudo (Dias 8-9 e 13)**
   - Não deixa ele estudar entrevista em texto
   - Faz parecer real

3. **Não deixa ele perfeccionista**
   - Não precisa ser perfeito, precisa ser funcional
   - Um projeto ruim é melhor que nenhum projeto

4. **GitHub é currículo dele**
   - Que todos os 2 projetos estejam públicos
   - Escreva bons README

5. **Entrevista real virá**
   - Depois do 14º dia, ele vai ser chamado (muito provavelmente)
   - Antes da entrevista, releia o Dia 13 e treina de novo

---

# Materiais Recomendados (Links)

## Python
- Curso em Vídeo: https://www.youtube.com/c/CursoemVideo (Python Guanabara)
- Exercism.io: https://exercism.io/ (exercícios práticos)
- Real Python: https://realpython.com (referência)

## APIs
- Postman Learning: https://learning.postman.com/
- Reqres: https://reqres.in/ (fake API para testar)
- Webhook.site: https://webhook.site/ (testar webhooks)

## TI
- CompTIA+ Study Material
- David Bombal (YouTube) — Redes, Linux

## Excel
- Microsoft Learn: https://learn.microsoft.com/excel (oficial)

## Segurança
- OWASP Top 10
- LGPD: https://www.gov.br/cidadania/pt-br/acesso-a-informacao/lgpd

---

**Você consegue. Seu amigo consegue. 14 dias é tempo suficiente para passar nessa entrevista.**

---

*Versão otimizada e simplificada — Maio 2026*
