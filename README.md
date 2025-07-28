# 🧙 Projeto RPG com IA Local — Especificação Geral

## 📌 Visão Geral

Este é um projeto de RPG de mesa digital com foco em imersão, acessibilidade e controle local. O sistema usará **modelos de IA locais** para atuar como mestre, jogadores controlados por IA, NPCs e tradução. Os conteúdos de regras e ambientação serão carregados via **arquivos PDF**, e o sistema será flexível para diferentes estilos de RPG (ex: D&D, Hora de Aventura).

---

## ✅ Requisitos Mínimos

### 🎮 Funcionais

- [ ] Criação de mesas de jogo
- [ ] IA mestre com narrativa e controle total do jogo
- [ ] Jogadores humanos e IA (configurável)
- [ ] Upload de PDFs de regras/ambientação
- [ ] Fichas de personagens com estrutura dinâmica
- [ ] Múltiplas mesas simultâneas
- [ ] Geração de imagens com IA local
- [ ] Tradução automática entre PT-BR e EN via IA
- [ ] Histórico das sessões salvo localmente
- [ ] Criação de NPCs com personalidade e aparência

### 🧠 Inteligência Artificial

- [ ] IA Mestre superinteligente e central
- [ ] IA Tradutora leve e local (PT ↔ EN)
- [ ] IA para jogadores (IA-PCs)
- [ ] IA separada para NPCs importantes com personalidade e aparência
- [ ] Limites configuráveis para IA-PCs e NPCs por mesa

---

## 🧱 Arquitetura Geral

### Backend

- **Framework**: Django + Django REST Framework
- **Banco de Dados**: MongoDB via `djongo`
- **Frontend**: Templates Django
- **IA Local**: Via [Ollama](https://ollama.com) + scripts locais

### Estrutura esperada

```bash
rpg_ai/
├── core/
│   ├── models.py
│   ├── views.py
│   ├── templates/
│   ├── forms.py
│   └── ai/
│       ├── ollama_handler.py
│       ├── image_gen.py
│       ├── translator.py
│       ├── personality_manager.py
│       └── pdf_parser.py
├── static/
├── settings.py
└── manage.py
```

---

## ⚙️ Funcionalidades Técnicas

### 🔤 Tradução

- Traduz prompts do jogador (PT → EN)
- Traduz respostas da IA (EN → PT)
- Realizada por IA local leve (ex: `gemma:2b`)

### 🧠 IA Mestre

- Controla narrativa, regras e NPCs simples
- Utiliza PDFs e histórico como contexto
- Superinteligente e adaptável

### 🤖 Jogadores IA

- Criados com IA separada
- Limitados por `max_ai_players`

### 👑 NPCs Importantes

- Criados com modelo separado, controlado pela IA mestre
- Personalidade e aparência definidas
- Limite controlado por `max_npcs`
- Armazenados em arquivos `.json` com estrutura abaixo:

```json
{
  "nome": "Rei Gondar",
  "personalidade": "orgulhoso, estratégico, ameaçador",
  "contexto": "Rei do reino de Astrelia, comanda com mão de ferro.",
  "modelo": "mistral",
  "aparencia": "Homem idoso, armadura dourada, coroa imensa de rubis, olhos frios e cabelos brancos longos.",
  "imagem_base": "static/img/personagens/rei_gondar_base.png"
}
```

A IA de imagem usará a `aparencia` como prompt e poderá usar `imagem_base` como referência visual para consistência.

---

## 🔧 Configuração de mesa (exemplo)

```json
{
  "nome": "Mesa do Trono Sombrio",
  "sistema": "D&D",
  "descricao": "A guerra dos cinco reinos",
  "max_ai_players": 2,
  "max_npcs": 3,
  "modelos": {
    "mestre": "llama3",
    "tradutor": "gemma:2b",
    "npc_ia": "mistral",
    "npc_premium": ["rei_gondar", "arauto_morto"]
  }
}
```

---

## 🗃️ Banco de Dados

### GameTable

```python
class GameTable(models.Model):
    nome = models.CharField(max_length=100)
    sistema = models.CharField(max_length=100)
    descricao = models.TextField(blank=True)
    max_ai_players = models.IntegerField(default=0)
    max_npcs = models.IntegerField(default=0)
    modelos = models.JSONField()  # mestre, tradutor, npc_ia, npc_premium
```

### Character

```python
class Character(models.Model):
    table_id = models.CharField(max_length=100)
    nome = models.CharField(max_length=100)
    sistema = models.CharField(max_length=100)
    tipo = models.CharField(choices=[('humano', 'Humano'), ('ia', 'IA'), ('npc', 'NPC')])
    atributos = models.JSONField()
    inventario = models.JSONField()
    historia = models.TextField(blank=True)
```

---

## 🛠️ Ferramentas

- **PDF Parsing**: PyMuPDF (`fitz`) ou LangChain com chunking
- **IA via terminal**: Ollama
- **Geração de imagem**: Stable Diffusion local (via CLI ou WebUI)
- **Tradução**: IA leve como `phi`, `gemma`, `tinyllama`

---

## 🚀 Próximos Passos

1. Inicializar projeto Django com suporte a MongoDB
2. Criar models `GameTable` e `Character`
3. Criar estrutura para NPCs importantes em JSON
4. Implementar módulo de tradução
5. Criar integração com IA mestre via script
6. Gerar imagens via prompt + imagem base
