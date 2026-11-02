# Changelog

Todas as mudanças notáveis deste projeto serão documentadas neste arquivo.

O formato é baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/)
e este projeto adere ao [Versionamento Semântico](https://semver.org/lang/pt-BR/).

## [1.0.0] - 2026-11-02

### Adicionado
- Pipeline assíncrono completo: `Scanner → Analyzer → Fixer → CLI`.
- **Scanner** (`core/scanner.py`): Playwright headless para URLs e arquivos locais, screenshot full-page em Base64, extração da Accessibility Tree (com fallback via CDP) e filtragem cirúrgica dos 4 pilares via injeção de JavaScript.
- **Analyzer** (`core/analyzer.py`): cliente compatível com o SDK da OpenAI (qualquer `AI_BASE_URL`) com suporte multimodal e **Motor Smart Mock/Fallback** determinístico que funciona sem chave de API.
- **Fixer** (`core/fixer.py`): geração de diffs "Antes/Depois", auto-remediação de DOM com BeautifulSoup e relatório Markdown.
- **CLI** (`cli/main.py`): comandos `scan`, `fix` e `demo` com Typer e Rich.
- Auditoria dos 4 pilares WCAG 2.2 (A e AA): contraste, elementos interativos mudos, semântica de mídia e ordem de foco/teclado.
- Página de teste `tests/broken_page.html` com falhas intencionais.
- Especificações Spec-Driven Development em `.kiro/specs/`, `requirements.md`, `design.md` e `tasks.md`.
- Documentação pública: `README.md`, `docs/USAGE.md`, `docs/CONFIGURATION.md`, `docs/TROUBLESHOOTING.md`, `CONTRIBUTING.md`, `LICENSE` (MIT) e este `CHANGELOG.md`.

[1.0.0]: https://github.com/devrisoles/Kiro-A11y-Agent/releases/tag/v1.0.0
