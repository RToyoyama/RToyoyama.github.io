# Torre de Hanói

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white)
![uv](https://img.shields.io/badge/uv-package%20manager-DE5FE9?logoColor=white)
![OOP](https://img.shields.io/badge/OOP-SOLID-4caf50)
![Ruff](https://img.shields.io/badge/Lint-Ruff-D7FF64)

[:fontawesome-brands-github: Repositório](https://github.com/RToyoyama/hanoi_tower){ .md-button .md-button--primary }

---

## Visão Geral

Implementação do problema clássico da **Torre de Hanói** em Python, com foco
em boas práticas de engenharia de software: orientação a objetos, separação
de responsabilidades, configuração externalizada e interface de linha de comando.

!!! info "Por que este projeto?"
    Mais do que resolver o problema, o objetivo foi aplicar princípios reais
    de engenharia: nenhum valor hardcoded, cada classe com uma única
    responsabilidade, e configuração por YAML com prioridade clara.

---

## Arquitetura

```
main.py
  └── cli.py
        ├── config/config.py  ←─── config/config.yml
        └── jogo.py
              ├── disco.py
              ├── torre.py
              └── visualizador.py
                    └── config/config.py
```

**Prioridade de configuração:**

```
argumento CLI  >  config.yml  >  código
```

---

## Stack Utilizada

- **Python 3.11** — linguagem principal
- **uv** — gerenciamento de dependências
- **PyYAML** — configuração externalizada
- **Ruff** — lint e formatação
- **Pytest** — testes unitários

---

## Implementação

### Configuração externalizada (YAML)

```yaml
jogo:
  discos_padrao: 3
  discos_maximo: 25
  torre_origem:   "A"
  torre_destino:  "C"
  torre_auxiliar: "B"

cli:
  passo_a_passo:  false
  delay_segundos: 0.0
  colorido:       true
```

### CLI

```bash
# Resolve com N discos, passo a passo, com delay visual
uv run main.py 5 --passo-a-passo --delay 0.3

# Apenas a lista de movimentos
uv run main.py 3 --so-movimentos
```

---

## Conceitos OOP Aplicados

| Classe | Princípio / Padrão |
|---|---|
| `Disco` | Encapsulamento, dunder methods (`__eq__`, `__lt__`, `__str__`, `__repr__`) |
| `Torre` | Composição, exceções de domínio, protocolo container (`__len__`, `__iter__`) |
| `Jogo` | Orquestração, `@dataclass`, `@property`, método privado |
| `Visualizador` | **Single Responsibility** — só renderiza, nunca decide |
| `ConfigJogo` | `@dataclass(frozen=True)` — imutável por design |
| `config.py` | Singleton via módulo, fail-fast, `yaml.safe_load` |

---

## O Algoritmo

A Torre de Hanói é resolvida com **recursão** usando dividir para conquistar:

```python
def _resolver(self, n, origem, destino, auxiliar):
    if n == 1:
        self._mover(origem, destino)
        return
    self._resolver(n - 1, origem, auxiliar, destino)
    self._mover(origem, destino)
    self._resolver(n - 1, auxiliar, destino, origem)
```

O número mínimo de movimentos é sempre **2ⁿ − 1**:

| Discos | Movimentos |
|--------|-----------|
| 3 | 7 |
| 5 | 31 |
| 10 | 1.023 |
| 20 | 1.048.575 |
| 64 | ~1,8 × 10¹⁹ |

---

## Evoluções Futuras

- [ ] Interface gráfica com Pygame
- [ ] Modo interativo — usuário move os discos manualmente
- [ ] Benchmark de performance para N grande