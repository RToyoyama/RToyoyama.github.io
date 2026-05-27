# EduConnect — Sistema de Gestão Educacional

![Java](https://img.shields.io/badge/Java-11%2B-ED8B00?logo=openjdk&logoColor=white)
![JDK](https://img.shields.io/badge/JDK-21%20LTS-ED8B00?logo=openjdk&logoColor=white)
![OOP](https://img.shields.io/badge/OOP-SOLID-4caf50)
![Status](https://img.shields.io/badge/Status-Completo-success)

[:fontawesome-brands-github: Repositório](https://github.com/RToyoyama/SistemaGestaoEducacional){ .md-button .md-button--primary }

---

## Visão Geral

Sistema de Gestão Educacional completo em Java demonstrando arquitetura em
3 camadas, princípios SOLID e todos os pilares da Programação Orientada a Objetos.

!!! info "Contexto"
    Projeto desenvolvido para a disciplina de Imersão Profissional —
    Aplicando Orientação a Objetos (Unicesumar), com foco em arquitetura
    profissional em Java.

---

## Arquitetura em 3 Camadas

```
┌─────────────────────────────────────────────────┐
│         CAMADA UI (Apresentação)                │
│    Menu.java - Interface com Usuário            │
└──────────────┬──────────────────────────────────┘
               │
┌──────────────▼──────────────────────────────────┐
│       CAMADA SERVICE (Lógica de Negócio)        │
│  AlunoService · ProfessorService · CursoService │
│  TurmaService · SistemaService                  │
└──────────────┬──────────────────────────────────┘
               │
┌──────────────▼──────────────────────────────────┐
│   CAMADA REPOSITORY (Persistência/Dados)        │
│  AlunoRepository · ProfessorRepository          │
│  CursoRepository · TurmaRepository              │
└─────────────────────────────────────────────────┘
```

---

## Funcionalidades

=== "Gestão Acadêmica"
    - Cadastro e busca de alunos e professores
    - Cursos presenciais e EAD
    - Criação e gerenciamento de turmas
    - Registro e validação de avaliações (0–10)
    - Cálculo automático de médias e situação acadêmica

=== "Relatórios"
    - Relatório por aluno, professor, curso ou turma
    - Relatório completo do sistema com estatísticas
    - Dados pré-carregados para demonstração

=== "Autenticação"
    - Três perfis: Aluno, Professor, Administrador
    - Controle de permissões por nível de acesso
    - Validação de login/senha

---

## Conceitos OOP Aplicados

```java
// Encapsulamento — atributo privado com acesso controlado
private double nota;

public boolean atribuirNota(double valor) {
    if (valor >= 0 && valor <= 10) {
        this.nota = valor;
        return true;
    }
    return false;
}
```

```java
// Herança
public class CursoPresencial extends Curso { ... }
public class Aluno extends Usuario { ... }

// Interface
public interface Autenticavel {
    boolean autenticar(String login, String senha);
}

// Classe abstrata
public abstract class Usuario implements Autenticavel, Relatorio {
    public abstract String obterPerfil();
    public abstract void gerarRelatorio();
}
```

---

## Estatísticas do Projeto

| Métrica | Valor |
|---------|-------|
| Total de classes | 24 |
| Linhas de código | ~3.500+ |
| Interfaces implementadas | 2 |
| Classes abstratas | 1 |
| Métodos implementados | 150+ |

---

## Evoluções Futuras

- [ ] Integração com banco de dados (PostgreSQL)
- [ ] API REST para integração com outros sistemas
- [ ] Autenticação JWT
- [ ] Interface gráfica (JavaFX)
- [ ] Relatórios em PDF