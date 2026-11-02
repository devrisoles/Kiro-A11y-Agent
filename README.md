# 🎃 Kiro A11y Agent — Autonomous Web Accessibility Auditor & Auto-Fixer (WCAG 2.2)

> **Submissão Oficial para o Hackathon Kiroween**  
> **Desenvolvido por kirodev**  
> *De páginas inacessíveis a código 100% conforme com a WCAG 2.2 em segundos — alimentado por Playwright Headless, Accessibility Tree real, Visão Computacional Multimodal e Spec-Driven Development.*

---

## 💡 Inspiração e Problema

De acordo com o estudo **WebAIM Million**, mais de **96% das páginas web** falham em critérios básicos da **WCAG 2.1/2.2**. As ferramentas tradicionais de acessibilidade sofrem de três limitações críticas:
1. **Validadores estáticos são cegos ao navegador real:** Analisam apenas HTML bruto sem calcular estilos CSS herdados, transparências, estados renderizados via JS ou a **Árvore de Acessibilidade (Accessibility Tree)** real exposta aos leitores de tela (NVDA, VoiceOver, JAWS).
2. **Validadores tradicionais não enxergam contexto visual:** Eles conseguem dizer se um atributo `alt` existe, mas não sabem detectar se `alt="imagem"` ou `alt="foto"` é inútil nem gerar uma descrição contextual precisa.
3. **Eles apontam erros, mas não consertam o código:** Desenvolvedores recebem dezenas de alertas técnicos da W3C sem o patch de código pronto para aplicar.

O **Kiro A11y Agent** nasce para resolver isso como um **Agente Autônomo de Ciclo Fechado**: ele abre a página em um navegador real, extrai a Accessibility Tree e estilos computados, analisa o screenshot multimodalmente e **gera + aplica automaticamente os patches de código HTML/CSS/JSX**, revalidando a página até atingir **100/100 de score**.

---

## 🎯 Os 4 Pilares de Auditoria Cirúrgica (Escopo WCAG 2.2 A & AA)

| Pilar | Critérios WCAG 2.1 / 2.2 | Como o Kiro A11y Agent Detecta e Corrige |
| :--- | :--- | :--- |
| **1. Contraste e Visibilidade** | `1.4.3 Contrast (Minimum) (AA)`<br>`1.4.11 Non-text Contrast (AA)` | Coleta `color` e `backgroundColor` computados (resolvendo fundos transparentes na árvore ancestral), calcula a luminância relativa oficial da W3C e sugere/aplica cores hexadecimais com razão $\ge 4.5:1$ (ou $\ge 7.0:1$ AAA). |
| **2. Elementos Interativos Mudos** | `4.1.2 Name, Role, Value (A)`<br>`2.4.4 Link Purpose (A)`<br>`1.3.1 Info & Relationships (A)` | Identifica `<button>`, `<a>`, `<input>`, `<select>` sem texto visível, `aria-label` ou `<label>` na Accessibility Tree e injeta rótulos semânticos baseados no ícone/contexto. |
| **3. Semântica de Mídia (Vision)** | `1.1.1 Non-text Content (A)` | Detecta `<img>` e `<svg>` sem `alt`, com `alt=""` indevido em cards/links ou descrições genéricas (`"imagem"`, `"foto"`) e sintetiza descrições visuais contextuais. |
| **4. Ordem de Foco e Teclado** | `2.4.3 Focus Order (A)`<br>`2.4.7 Focus Visible (AA)`<br>`2.1.1 Keyboard (A)`<br>`2.1.2 No Keyboard Trap (A)` | Corrige `tabindex > 0`, restaura indicadores `:focus-visible` suprimidos por `outline: none`, torna `div[onclick]` acessíveis via teclado (`Enter`/`Space`) e elimina armadilhas de teclado (`Tab`/`Escape` bloqueados). |

---

## 🏗️ Arquitetura Modular e Spec-Driven Development

O projeto foi planejado e construído seguindo a metodologia **Spec-Driven Development** do Kiro:
- 📄 [`requirements.md`](./requirements.md) — Requisitos funcionais no padrão EARS e critérios de aceitação WCAG 2.2.
- 📐 [`design.md`](./design.md) — Arquitetura de pipeline assíncrono, algoritmos de luminância e schema JSON.
- ✅ [`tasks.md`](./tasks.md) — Plano de implementação modular e verificação de ponta a ponta.

```text
Kiro A11y Agent/
├── .kiro/specs/kiro-a11y-agent/  # Especificações Kiro (requirements.md, design.md, tasks.md)
├── requirements.md               # Especificação de Requisitos (EARS + WCAG 2.2)
├── design.md                     # Arquitetura técnica e fluxo multimodal
├── tasks.md                      # Checklist de engenharia e validação
├── core/
│   ├── scanner.py                # Async Playwright + Screenshot B64 + Accessibility Tree + JS DOM Filter
│   ├── analyzer.py               # Cliente OpenAI SDK (qualquer BASE_URL) + Motor Smart Mock/Fallback
│   └── fixer.py                  # Gerador de diffs Antes/Depois, Auto-Fixer DOM e Relatório Markdown
├── cli/
│   └── main.py                   # Interface CLI interativa com Typer e Rich (scan, fix, demo)
├── tests/
│   └── broken_page.html          # Mini E-commerce realista com falhas intencionais nos 4 pilares
├── reports/
│   ├── audit_report.md           # Relatório executivo e técnico gerado automaticamente
│   ├── fixed_page.html           # Página HTML corrigida automaticamente pelo agente (Score 100/100)
│   └── latest_screenshot.png     # Captura visual de página inteira
├── .env.example                  # Template de configuração flexível de IA
├── requirements.txt              # Dependências Python 3.10+
└── README.md                     # Documentação Devpost
```

---

## 🚀 Instalação e Execução Rápida

### 1. Pré-requisitos
- **Python 3.10+**

### 2. Instalar dependências e navegador Headless
```bash
cd "Kiro-A11y-Agent"
python -m venv venv

# No Windows (PowerShell):
.\venv\Scripts\Activate.ps1

# No Linux/macOS:
# source venv/bin/activate

pip install -r requirements.txt
playwright install chromium
```

### 3. Configurar `.env` (Opcional — Possui Modo Smart Mock Automático!)
Copie `.env.example` para `.env`:
```bash
copy .env.example .env
```
> **Nota para os Jurados do Kiroween:** Você **não precisa** configurar uma chave de API para testar! Se `AI_API_KEY` estiver em branco, o `analyzer.py` ativa automaticamente o **Modo Smart Mock/Fallback**, analisando o DOM real e a Accessibility Tree extraídos pelo Playwright e gerando todos os patches e relatórios em tempo real.

---

## 💻 Comandos da CLI (`typer` + `rich`)

### 1. Rodar a Demonstração Completa do Hackathon (`demo`)
Executa o ciclo autônomo completo em `tests/broken_page.html`: escaneia a página quebrada, gera `reports/audit_report.md`, cria a página corrigida `reports/fixed_page.html` e **re-audita a página corrigida** comprovando que o score salta para **100/100**:
```bash
python -m cli.main demo
```

### 2. Auditar qualquer URL ou arquivo local (`scan`)
```bash
# Auditar a página de testes local
python -m cli.main scan tests/broken_page.html

# Auditar uma URL real na web
python -m cli.main scan https://example.com
```

### 3. Gerar e Aplicar Patches Automaticamente (`fix`)
```bash
# Gera a versão corrigida em reports/fixed_page.html e exibe os diffs Antes/Depois no terminal
python -m cli.main fix tests/broken_page.html

# Para aplicar diretamente no arquivo original (cria backup .bak automaticamente):
python -m cli.main fix tests/broken_page.html --in-place
```

---

## 📊 Exemplo de Resultado Real (`demo`)

| Métrica | Antes (`tests/broken_page.html`) | Depois (`reports/fixed_page.html`) |
| :--- | :---: | :---: |
| **Score de Acessibilidade** | `0 / 100` (Crítico) | **`100 / 100`** |
| **1. Contraste e Visibilidade** | 3 falhas graves (`1.68:1` a `2.12:1`) | **0 falhas (`> 11.0:1` AAA)** |
| **2. Elementos Interativos Mudos** | 5 controles sem nome acessível | **0 falhas (`aria-label` + `<label>`)** |
| **3. Semântica de Mídia** | 5 mídias sem `alt` ou com `alt` genérico | **0 falhas (`alt` contextual)** |
| **4. Ordem de Foco e Teclado** | 6 falhas (`tabindex > 0`, `outline: none`, trap) | **0 falhas (`:focus-visible` + livre)** |
| **Status WCAG 2.2 Nível AA** | ❌ Não Conforme | ✅ **100% Conforme** |

---

## 🛠️ Tecnologias Utilizadas
- **Python 3.10+**
- **Playwright (Async API)** — Renderização real, cálculo CSS computado, Screenshot Full-Page e Accessibility Tree (`page.accessibility.snapshot()` / CDP `Accessibility.getFullAXTree`).
- **OpenAI Python SDK** — Compatível com OpenRouter, OpenAI, Anthropic, Grok, Gemini e Ollama via `AI_BASE_URL`.
- **BeautifulSoup4** — Manipulação cirúrgica do DOM para aplicação de patches HTML/CSS.
- **Typer & Rich** — Interface de linha de comando moderna com tabelas coloridas, spinners e syntax highlighting.

---

## 👤 Autoria

Projeto **desenvolvido por kirodev**.
