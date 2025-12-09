# 📘 Aula 02 - Microsserviços

**Disciplina:** Arquitetura de Aplicações (12h)  
**Programa:** DevOps e Arquitetura Cloud - Fase 1  
**Instituição:** POSTECH FIAP + Alura

---

## 🎯 Objetivos da Aula

Antes de migrar serviços para o modelo de microsserviços, é fundamental:
- Discutir as principais características da arquitetura de ambos os modelos (monolito vs microsserviços)
- Entender os pontos fortes e fracos de cada proposta
- Utilizar a modelagem estratégica do DDD (Domain-Driven Design) para auxiliar na separação dos contextos

---

## 🏗️ Fluxo de Análise de Domínio

O processo de migração para microsserviços segue estas etapas:

```
1. Analyze domain
   ↓
2. Define bounded contexts
   ↓
3. Define entities, aggregates, and services
   ↓
4. Identify microservices
```

### Etapa 1: Análise do Domínio

Após analisar o domínio com especialistas de negócio, chegamos à separação dos domínios:

```
         Professor
              |
              |
         Empresa ---- ADM (Dashboards)
              |
              |
         Candidato --- Contas
```

### Etapa 2: Contextos Delimitados (Bounded Contexts)

Definição dos limites claros entre serviços:

```
┌─────────────────────────────────────┐
│  Professor                          │
│  - Agendamentos                     │
│  - Feedback                         │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│  Empresa + ADM + Contas             │
│  - Candidatos                       │
│  - Acessos                          │
│  - Dashboards                       │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│  Candidato                          │
│  - Testes                           │
│  - Status                           │
└─────────────────────────────────────┘
```

---

## 🔑 Conceitos Fundamentais

### O que são Microsserviços?

Microsserviços são uma abordagem arquitetural onde:
- Cada serviço é responsável por **uma única funcionalidade**
- Não existe "bala de prata" - requer análise cuidadosa
- É necessário refletir sobre: domínio de negócio, requisitos, características arquiteturais e objetivos

### Princípios de Design

**Estruturação baseada em:**
- ✅ Capacidades de negócios
- ❌ Não em camadas técnicas

**Características importantes:**
- **Baixo acoplamento:** um serviço pode ser alterado sem modificar outros
- **Alta coesão:** cada serviço tem um propósito bem definido e específico
- **Abstração de complexidade:** interface simples ocultando detalhes internos
- **Características arquiteturais individuais:** cada serviço define seus próprios requisitos de desempenho, segurança, etc.

---

## 🧩 Exemplo de Divisão de Microsserviços

### 1. Serviço de Autenticação
- Gerencia autenticação de todos os usuários
- Implementação com JWT (JSON Web Tokens)
- Segurança centralizada (login/logout)
- Tokens compartilháveis entre sistemas usando roles

### 2. Serviço do Administrador (ADM)
Funcionalidades:
- Quantidade de professores cadastrados
- Quantidade de empresas vinculadas
- Quantidade de acessos por empresa
- Quantidade de candidatos por empresa

**Implementação sugerida:** banco de cache (hot cache ou cold cache) com requisições à API quando necessário

### 3. Microsserviço do Professor
Funcionalidades:
- Listar agendamentos de testes em aberto
- Fornecer feedback sobre desempenho dos candidatos

### 4. Microsserviço da Empresa (Company)
Funcionalidades:
- Listar candidatos
- Envio de credenciais de acesso
- Acompanhamento de progresso dos testes

### 5. Microsserviço do Candidato
Funcionalidades:
- Realizar testes
- Ver status do teste (concluído, pendente, feedback disponível)

---

## 📋 Boas Práticas na Arquitetura de Microsserviços

### Desenho de Serviços
- Garantir que cada serviço seja **coeso** e tenha **responsabilidade única** (SRP)
- Refletir domínios de negócio
- Aplicar conceitos do Domain-Driven Design (DDD)

### Dependências e Comunicação
- ❌ **Evitar dependências fortes** - serviços devem ser autônomos
- ✅ **Comunicação mínima e clara** - evitar dependências complexas e latências desnecessárias
- Permitir que cada serviço seja escalado e mantido independentemente

### Características Arquiteturais
Cada microsserviço deve definir individualmente:
- Desempenho
- Segurança
- Tolerância a falhas
- Disponibilidade

> ⚠️ **Atenção:** Comunicação síncrona pode compartilhar características arquiteturais entre serviços, aumentando interdependência

---

## 🌐 Mercado, Cases e Tendências

### Modernização Incremental
- Empresas com sistemas monolíticos enfrentam desafios de manutenção, evolução e escalabilidade
- Decomposição guiada por **bounded contexts** (DDD)
- Reduz risco de reescritas totais
- Permite entrega contínua de valor

### Organização de Times
- **Times multidisciplinares organizados por produto/domínio**
- Conceito: *"You build it, you run it"*
- Mesma equipe responsável por desenvolvimento, operação e evolução
- Arquitetura reflete a estrutura de negócio

### Arquiteturas Guiadas por Observabilidade
Ferramentas essenciais:
- **Prometheus** - métricas
- **Grafana** - visualização
- **ELK Stack** - logs estruturados
- **Jaeger** - tracing distribuído
- **OpenTelemetry** - observabilidade padronizada

### Arquiteturas Sustentáveis
Elementos importantes:
- Documentações vivas
- Versionamento de contratos (OpenAPI, AsyncAPI)
- Automação de testes entre serviços
- Engenharia de plataforma
- Plataformas internas de desenvolvimento
- Bibliotecas reutilizáveis
- Políticas centralizadas de segurança e deploy

---

## ✅ Principais Aprendizados

1. **Análise antes da ação:** entender características, forças e fraquezas dos modelos arquiteturais
2. **DDD como aliado:** modelagem estratégica auxilia na separação de contextos
3. **Autonomia e coesão:** serviços independentes com propósitos bem definidos
4. **Observabilidade:** monitoramento proativo é essencial
5. **Evolução contínua:** arquitetura deve acompanhar mudanças do negócio

---

## 📚 Referências

- AWS. **Microsserviços**. 2019. [Link](https://aws.amazon.com/pt/microservices/)
- BASS, L.; CLEMENTS, P.; KAZMAN, R. **Software Architecture in Practice**. Boston: Addison-Wesley, 2012.
- KRUCHTEN, P. **Architectural Blueprints — The 4+1 View Model of Software Architecture**. IEEE Software, v. 12, n. 6, p. 42-50, 1995.
- SHAW, M.; GARLAN, D. **Software Architecture: Perspectives on an Emerging Discipline**. Upper Saddle River: Prentice Hall, 1996.

---

## 🏷️ Tags

`#Microsserviços` `#ArquiteturaDeSoftware` `#DDD` `#DevOps` `#CloudArchitecture` `#BoundedContexts` `#POSTECH`

---

**Autor do Resumo:** Documentação de Estudo - POSTECH FIAP  
**Data:** Dezembro 2025