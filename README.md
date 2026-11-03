<div align="center">
⚡ Rafadev Native Engineering ⚡
Blueprint de Alta Performance para Agentes de Engenharia de Software Nativo
Uma especificação arquitetural profissional para criação de uma Skill avançada usando o Skill Creator.

Criado por Lyrien | Estruturado por Rafadev
</div>
🎯 A Visão
O Lyrien Native Engineering não é apenas um prompt; é um blueprint de Skill rigorosamente desenhado para forjar um assistente de programação de elite, especializado em desenvolvimento nativo e engenharia de software de baixo nível.
Nossa arquitetura de IA é baseada em princípios rígidos para garantir resultados de nível de produção:
> 🛡️ Código Confiável • 🚫 Menos Alucinações • 🔍 Análise de Erros Cirúrgica
> 
> ⚙️ Debug Estruturado • ✅ Validação de Build • 📐 Engenharia Limpa
> 
🛠️ Arsenal Tecnológico
A Skill gerada possui um contexto profundo e especializado nas seguintes tecnologias e domínios:
| Linguagens & Runtimes | Sistemas & Internals | Segurança & Performance |
|---|---|---|
| <kbd>C</kbd> <kbd>C++</kbd> <kbd>C#</kbd> | <kbd>Windows Internals</kbd> | <kbd>Debugging Avançado</kbd> |
| <kbd>Lua</kbd> <kbd>LuaJIT</kbd> | <kbd>WinAPI</kbd> | <kbd>Otimização de Performance</kbd> |
| <kbd>Cheat Engine Lua</kbd> | <kbd>Formato PE</kbd> | <kbd>Segurança de Código</kbd> |
⚙️ Arquitetura de Conhecimento
<details>
<summary><b>Clique para expandir as áreas de domínio da Skill</b></summary>


 * C: Programação de baixo nível: memória, ponteiros, undefined behavior, alocadores, segurança.
 * C++: C++ moderno: RAII, templates, STL, performance, concorrência.
 * C#: Ecossistema .NET: async/await, Span, Memory, interop, runtime.
 * Lua / LuaJIT: Metatables, scripts, integração nativa, performance extrema.
 * Cheat Engine Lua: Trainers, Memory Records, eventos, automação.
 * Windows: WinAPI, gestão de processos e threads, memória, estrutura PE, conceitos de drivers.
   </details>
🚀 Como Instalar e Gerar a Skill
Para transformar esta especificação em uma Skill funcional, você precisará de um ambiente preparado.
1. Pré-requisitos (Node.js)
O Skill Creator depende do ambiente Node.js. Verifique sua instalação:
node --version
npm --version

2. Instalação do Skill Creator
Esta Skill utiliza o Skill Creator da Anthropic. Instale a ferramenta via CLI:
npx skills add https://github.com/anthropics/skills --skill skill-creator

3. Pipeline de Geração
O motor principal deste projeto reside no arquivo de especificação: archive/lyrien-native-engineering-spec.md. Ele é o documento de engenharia usado para gerar a estrutura completa.
graph LR
    A[📄 lyrien-native-engineering-spec.md] -->|Input| B(🧠 IA com Skill Creator)
    B -->|Geração Estruturada| C{📂 lyrien-native-engineering/}
    style A fill:#1e1e1e,stroke:#333,stroke-width:2px,color:#fff
    style B fill:#0052cc,stroke:#003d99,stroke-width:2px,color:#fff
    style C fill:#238636,stroke:#1a6227,stroke-width:2px,color:#fff

Passo a passo:
 * Abra seu ambiente de IA compatível com Skills.
 * Forneça o arquivo de especificação.
 * Envie o seguinte prompt de ativação:
   > "Crie uma Skill usando esta especificação. Siga todos os requisitos exatamente. Gere toda a estrutura da Skill."
   > 
📂 Árvore de Diretórios Gerada
A IA construirá a seguinte estrutura modular:
lyrien-native-engineering/
├── SKILL.md
├── languages/
│   ├── c.md, cpp.md, csharp.md, lua.md, lua-cheat-engine.md
├── windows/
│   ├── winapi.md, pe-format.md, drivers.md
├── security/
│   ├── secure-coding.md, reverse-engineering.md, debugging.md
└── workflows/
    ├── code-review.md, build-validation.md

🔬 Metodologia de Operação
Uma vez gerada, a Skill atua como um Engenheiro Staff especializado.
Desenvolvimento Ativo
Atua em Criação de código, Refatoração, Arquitetura, Otimização e Revisão técnica rigorosa.
O Protocolo de Debug (RIACP)
A Skill não "chuta" soluções. Ela segue um pipeline determinístico:
sequenceDiagram
    participant R as 1. Reproduzir
    participant I as 2. Isolar
    participant A as 3. Analisar
    participant C as 4. Corrigir
    participant P as 5. Prevenir
    
    R->>I: Cria repetição confiável
    I->>A: Aplica logs, profilers e debuggers
    A->>C: Identifica estado/suposição falha (Causa Raiz)
    C->>P: Aplica correção real
    P-->>R: Adiciona proteção contra regressão

⚖️ A Filosofia do Código
Nossa Skill é governada por um manifesto de qualidade inegociável.
| ✅ Os Pilares (O que seguimos) | ❌ As Proibições (O que evitamos) |
|---|---|
| Correção antes de otimização | APIs inventadas ou alucinadas |
| Causa raiz antes de solução temporária | Código incompleto com placeholders |
| Código validado antes de assumir funcionamento | Soluções rasas sem contexto profundo |
| Engenharia rigorosa antes de tentativa e erro | Afirmações não verificadas tecnicamente |
<div align="center">
<sub>Criado com 🖤 por <b>Lyrien</b> | Desenvolvido para a comunidade de Engenharia de Software Nativo</sub>
</div>
