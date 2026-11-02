# Contribuindo com o Kiro A11y Agent

Obrigado pelo interesse em contribuir! Este guia explica como colaborar de forma eficiente.

## Código de conduta

Seja respeitoso e construtivo. Discussões técnicas são bem-vindas; ataques pessoais não.

## Como contribuir

1. **Faça um fork** do repositório e clone o seu fork.
2. **Crie uma branch** descritiva a partir de `main`:
   ```bash
   git checkout -b feat/descricao-curta
   # ou fix/, docs/, refactor/, test/
   ```
3. **Configure o ambiente** (veja o README):
   ```bash
   python -m venv venv
   .\venv\Scripts\Activate.ps1   # Windows
   # source venv/bin/activate    # Linux/macOS
   pip install -r requirements.txt
   playwright install chromium
   ```
4. **Faça suas mudanças** seguindo os padrões abaixo.
5. **Valide** rodando o fluxo completo:
   ```bash
   python -m cli.main demo --mock
   ```
6. **Faça commits** claros (veja convenção abaixo) e abra um **Pull Request** contra `main`.

## Padrões de código

- **Python 3.10+**, com type hints onde fizer sentido.
- Siga o estilo já presente no projeto (docstrings em módulos e funções, nomes descritivos).
- Prefira funções puras e pequenas; isole efeitos colaterais (I/O, rede, disco).
- Mantenha a compatibilidade multiplataforma (use `pathlib.Path` e UTF-8 explícito).
- **Não** quebre o Smart Mock/Fallback: qualquer nova regra deve funcionar sem chave de API.

## Convenção de commits

Usamos mensagens no estilo [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: adiciona detecção de aria-expanded ausente
fix: corrige cálculo de contraste para rgba transparente
docs: amplia o guia de configuração do Ollama
refactor: extrai utilitário de luminância
test: adiciona página com keyboard trap
```

## Reportando bugs

Abra uma **issue** incluindo:
- Comando executado e saída (ou screenshot).
- Sistema operacional e versão do Python.
- URL ou trecho de HTML que reproduz o problema (quando possível).
- Comportamento esperado vs. observado.

## Sugerindo melhorias

Abra uma issue descrevendo o problema que a melhoria resolve, não apenas a solução. Isso ajuda a discutir alternativas.

## Acessibilidade

Como este é um projeto de acessibilidade, mudanças que afetem detecção ou correção devem referenciar o **critério WCAG** correspondente e, idealmente, incluir um caso de teste em `tests/`.

## Licença das contribuições

Ao contribuir, você concorda que seu código será licenciado sob a licença **MIT** do projeto.
