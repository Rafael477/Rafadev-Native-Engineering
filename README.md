# Lyrien Native Engineering

[![Lyrien Native Engineering](https://img.shields.io/badge/Skill-Lyrien%20Native%20Engineering-blue)](https://github.com/seu-usuario/lyrien-native-engineering)

Uma especificação profissional para criação de uma Skill de engenharia de software usando Skill Creator.

Criado por **Lyrien**.

---

## Sobre o Projeto

O **Lyrien Native Engineering** é um blueprint de Skill criado para gerar um assistente de programação especializado em desenvolvimento nativo e engenharia de software.

A proposta é criar uma Skill com foco em:

- Código confiável.
- Menos alucinações.
- Melhor análise de erros.
- Debug estruturado.
- Validação de build.
- Boas práticas de engenharia.

---

## Tecnologias Cobertas

A Skill gerada possui conhecimento direcionado para:

- C
- C++
- C#
- Lua
- LuaJIT
- Cheat Engine Lua
- Windows Internals
- WinAPI
- Formato PE
- Debugging
- Performance
- Segurança de código

---

## Requisitos

Antes de criar a Skill, instale as dependências necessárias.

### Node.js

O Skill Creator depende do ambiente Node.js.

Verifique sua instalação:

```bash
node --version
npm --version
```

Caso não tenha Node.js instalado:  
[![Node.js](https://img.shields.io/badge/Node.js-43853D?style=for-the-badge&logo=node.js&logoColor=white)](https://nodejs.org/)

### Instalação do Skill Creator

Esta Skill utiliza o Skill Creator da Anthropic.

Instale usando:

```bash
npx skills add https://github.com/anthropics/skills --skill skill-creator
```

Após a instalação, o Skill Creator estará disponível para gerar novas Skills.

---

## Usando este Projeto

O arquivo principal de especificação está localizado em:

```
archive/lyrien-native-engineering-spec.md
```

Esse arquivo contém todas as instruções necessárias para criar a Skill final.  
**Ele não é a Skill pronta.**  
Ele é um documento de engenharia usado pelo Skill Creator para gerar a estrutura completa.

### Criando a Skill

1. Abra seu ambiente de IA compatível com Skills.
2. Envie o arquivo: `archive/lyrien-native-engineering-spec.md`
3. Depois solicite:

> Crie uma Skill usando esta especificação.  
> Siga todos os requisitos exatamente.  
> Gere toda a estrutura da Skill.

O Skill Creator deverá gerar a seguinte estrutura:

```
lyrien-native-engineering/
├── SKILL.md
├── languages/
│   ├── c.md
│   ├── cpp.md
│   ├── csharp.md
│   ├── lua.md
│   └── lua-cheat-engine.md
├── windows/
│   ├── winapi.md
│   ├── pe-format.md
│   └── drivers.md
├── security/
│   ├── secure-coding.md
│   ├── reverse-engineering.md
│   └── debugging.md
└── workflows/
    ├── code-review.md
    └── build-validation.md
```

---

## Como a Skill Funciona

Após gerada, a Skill atua como um engenheiro de software especializado.

### Desenvolvimento

- Criação de código.
- Refatoração.
- Arquitetura.
- Otimização.
- Revisão técnica.

### Debug

Segue um processo estruturado:

1. **Reproduzir** – Criar uma forma confiável de repetir o problema.
2. **Isolar** – Utilizar logs, debuggers, tracing, profilers.
3. **Analisar** – Encontrar estado incorreto, suposições inválidas, causa raiz.
4. **Corrigir** – Aplicar uma solução real.
5. **Prevenir** – Adicionar proteção contra novos erros.

---

## Áreas de Conhecimento

### C

Programação de baixo nível: memória, ponteiros, undefined behavior, alocadores, segurança.

### C++

C++ moderno: RAII, templates, STL, performance, concorrência.

### C#

.NET: async/await, Span, Memory, interop, runtime.

### Lua / LuaJIT

Metatables, scripts, integração nativa, performance.

### Cheat Engine Lua

Trainers, Memory Records, eventos, automação.

### Windows

WinAPI, processos, threads, memória, PE, conceitos de drivers.

---

## Filosofia

A Skill segue:

- Correção antes de otimização.
- Causa raiz antes de solução temporária.
- Código validado antes de assumir funcionamento.
- Engenharia antes de tentativa e erro.

Ela evita:

- APIs inventadas.
- Código incompleto.
- Soluções sem contexto.
- Afirmações não verificadas.

---

## Autor

**Lyrien**

---

## Comunidade de Estudos

Entre no Discord:

<a href="https://discord.gg/PaneXdSEaP">
  <img src="https://img.shields.io/badge/Discord-Entrar%20na%20Comunidade-5865F2?style=for-the-badge&logo=discord">
</a>
