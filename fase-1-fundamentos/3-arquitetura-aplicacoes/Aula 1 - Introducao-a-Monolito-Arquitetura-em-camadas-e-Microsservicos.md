# Arquitetura de Aplicações - Aula 01: Monolito, Arquitetura em Camadas e Microsserviços

## 📋 Resumo Executivo

Esta aula explora a evolução arquitetural de sistemas de software, partindo de aplicações monolíticas tradicionais até arquiteturas modernas baseadas em microsserviços. O conteúdo apresenta conceitos fundamentais, desafios práticos e a transição necessária para sistemas mais modulares, flexíveis e escaláveis.

---

## 🏛️ 1. ARQUITETURA MONOLÍTICA

### 1.1 Definição

A arquitetura monolítica é um design de software onde **todos os componentes** de uma aplicação são interligados em uma **única unidade coesa**. Isso significa que:

- Front-end
- Back-end
- Lógica de negócio
- Integração com banco de dados
- Todos os módulos

...coexistem no **mesmo bloco de código** e são implantados como uma **única entidade**.

### 1.2 Características Principais

#### ✅ Unidade Coesa
- Aplicação é uma unidade única e integrada
- Todos os componentes estão interligados
- **Uma única alteração** em qualquer módulo exige:
  - Recompilação de toda a aplicação
  - Reimplantação completa do sistema
  - Testes extensivos de todo o sistema

#### ⚠️ Dependências Fortes
- Todos os módulos dependem uns dos outros
- **Acoplamento forte** entre componentes
- Uma falha em um componente pode afetar todo o sistema
- Efeito cascata de problemas

#### 📈 Dificuldade em Escalar
- Impossível escalar partes específicas isoladamente
- Para escalar uma única função, é necessário:
  - Escalar toda a aplicação
  - Duplicar recursos desnecessariamente
  - Resulta em desperdício de infraestrutura

#### 🐌 Ciclo de Desenvolvimento Lento
- Aplicação é um grande bloco de código
- Qualquer mudança pode introduzir riscos
- Exige testes extensivos antes do deploy
- Ciclos de desenvolvimento, testes e implantação mais lentos
- Maior tempo de time-to-market

#### 🔧 Manutenção Complexa
- À medida que a aplicação cresce, manutenção se torna mais difícil
- Adicionar novos recursos ou corrigir bugs é arriscado
- Alterações podem ter impactos inesperados
- Código legado dificulta evoluções
- "Big ball of mud" em sistemas antigos

### 1.3 Exemplo Prático: Sistema de Gestão de Streaming (TV Bandeirantes)

#### Contexto
Sistema de gerenciamento de streaming desenvolvido com arquitetura monolítica.

#### Funcionalidades Principais
1. **Receber dados do satélite** via master
2. **Converter vídeos** para formato compatível com navegadores
3. **Cortar vídeos** (pequenos trechos/clipes)
4. **Upload de vídeos** em redes sociais

#### Estrutura do Projeto Monolítico

```
Solution 'MediaServices' (18 of 18 projects)
├── App
│   ├── App
│   └── Domain
├── Domain
├── Helpers
│   └── Helper
├── Repositories
│   └── Repositories
└── Services
    ├── Tests
    ├── Encoder
    ├── GarbageMovies
    ├── ManageFiles
    ├── Queue
    ├── QueueRadios
    ├── QueueYouTube
    ├── RecordChannels
    ├── Transcoder
    ├── TranscodeRadioStreaming
    ├── YoutubeMonetize
    ├── UI
    ├── Tests
    └── MediaServices.Web
```

#### Arquitetura em Camadas (Layered Architecture)

```
┌─────────────┐
│     UI      │  ← Interface de usuário
├─────────────┤
│   Service   │  ← Lógica de negócio
├─────────────┤
│    Model    │  ← Modelos de dados
├─────────────┤
│    Data     │  ← Acesso a dados
└─────────────┘
```

**Camadas tradicionais:**
- **UI (User Interface):** Apresentação
- **Service:** Regras de negócio
- **Model:** Entidades e DTOs
- **Data:** Repositórios e acesso ao banco

#### Fluxo Simplificado do Sistema Monolítico

```
┌─────────────────────────────┐
│        Monolítica           │
├─────────────────────────────┤
│ Receber dados do Satélite   │
├─────────────────────────────┤
│ Converter os vídeos         │
├─────────────────────────────┤
│      Camada WEB         ────┼──→ 💾 Banco de dados
├─────────────────────────────┤
│   Cortar os vídeos          │
├─────────────────────────────┤
│   Fazer o upload            │
└─────────────────────────────┘
```

#### Problemas Identificados

**Quando uma equipe precisa alterar um módulo (ex: integrar nova rede social):**

1. ❌ Modificar o código inteiro
2. ❌ Testar toda a aplicação
3. ❌ Deploy do projeto completo
4. ❌ Risco de quebrar outras funcionalidades
5. ❌ Downtime durante implantação
6. ❌ Rollback complexo em caso de falha

**Exemplo concreto:**
- Adicionar upload para nova rede social
- Mesmo sendo mudança isolada, afeta todo o sistema
- Requer parada para deploy
- Testes em todas as áreas

---

## 🔷 2. ARQUITETURA DE MICROSSERVIÇOS

### 2.1 Definição

Microsserviços são uma **abordagem arquitetural** que divide uma aplicação em um conjunto de **serviços pequenos, independentes e focados** em uma única tarefa ou funcionalidade.

**Características fundamentais:**
- Serviços "micro" são **autônomos**
- Podem ser **desenvolvidos independentemente**
- Podem ser **implantados independentemente**
- Podem ser **escalados independentemente**
- Cada serviço tem **responsabilidade única**

### 2.2 Características Principais dos Microsserviços

#### 🎯 Autonomia
- Cada microsserviço é independente
- Responsável por uma única função da aplicação
- Self-contained (autocontido)
- Pode falhar sem derrubar todo o sistema

#### 👥 Desenvolvimento Descentralizado
- Equipes podem trabalhar em diferentes microsserviços
- Trabalho isolado e paralelo
- Diferentes linguagens ou tecnologias por serviço
- Times autônomos e multidisciplinares

#### 📊 Escalabilidade Granular
- **Apenas** os serviços que necessitam são escalados
- Otimização de utilização de infraestrutura
- Economia de recursos
- Performance direcionada

#### 🔌 Comunicação via API
- Microsserviços se comunicam por APIs
- Protocolos comuns:
  - **REST** (HTTP/JSON)
  - **GraphQL**
  - **RPC** (Remote Procedure Call)
  - **Mensageria** (RabbitMQ, Kafka)

### 2.3 Comunicação REST entre Microsserviços

#### Status Codes HTTP

```
┌──────┐
│ 1XX  │  INFORMATIVO
├──────┤
│ 2XX  │  SUCESSO
├──────┤
│ 3XX  │  REDIRECIONAMENTO
├──────┤
│ 4XX  │  ERRO DO CLIENTE
├──────┤
│ 5XX  │  ERRO DO SERVIDOR
└──────┘
```

**⚠️ CRÍTICO:** 
Nunca retorne status `200 OK` em caso de erro! A comunicação entre microsserviços depende de status codes corretos.

**Exemplos de uso correto:**
- `200 OK` - Sucesso
- `201 Created` - Recurso criado
- `400 Bad Request` - Dados inválidos
- `401 Unauthorized` - Não autenticado
- `403 Forbidden` - Sem permissão
- `404 Not Found` - Recurso não encontrado
- `500 Internal Server Error` - Erro no servidor
- `503 Service Unavailable` - Serviço indisponível

### 2.4 Benefícios dos Microsserviços

| Benefício | Descrição |
|-----------|-----------|
| **Modularidade** | Sistema dividido em partes menores e gerenciáveis |
| **Flexibilidade Tecnológica** | Cada serviço pode usar stack diferente |
| **Escalabilidade Independente** | Escala apenas o necessário |
| **Deploy Independente** | Atualiza serviços sem afetar outros |
| **Resiliência** | Falha isolada, não derruba todo o sistema |
| **Time-to-Market** | Entregas mais rápidas e frequentes |
| **Manutenção Simplificada** | Código menor e mais focado |
| **Testes Facilitados** | Testa serviços isoladamente |

### 2.5 Desafios dos Microsserviços

#### 🧩 Complexidade Geral

**Problema:**
- Sistema como um todo tende a ser mais complicado
- Coordenação entre serviços exige atenção
- Comunicação distribuída adiciona camadas

**Mitigação:**
- Ferramentas de orquestração (Kubernetes)
- Service mesh (Istio, Linkerd)
- API Gateway centralizado
- Monitoramento distribuído

#### 🧪 Desenvolvimento e Testes

**Problema:**
- Desenvolver serviços interdependentes requer mentalidade diferente
- Ferramentas nem sempre adequadas para dependências múltiplas
- Dificulta refatoração
- Testes de integração complexos
- Ambientes de desenvolvimento local complicados

**Mitigação:**
- Contract testing (Pact, Spring Cloud Contract)
- Mocks e stubs para dependências
- Docker Compose para ambiente local
- Testes end-to-end automatizados

#### 🏛️ Governança Descentralizada

**Problema:**
- Flexibilidade pode gerar falta de padronização
- Diferentes equipes usam linguagens/frameworks variados
- Complica manutenção e integração
- Dificulta rotação de pessoas entre times

**Mitigação:**
- Definir diretrizes comuns (não excessivas)
- Padronização de:
  - Logs (formato, níveis)
  - Monitoramento (métricas, traces)
  - Autenticação/Autorização
  - Contratos de API
- Inner source e code reviews

#### ⏱️ Latência e Comunicação

**Problema:**
- Dividir sistema aumenta comunicação entre serviços
- Latência pode se acumular em cadeia de chamadas
- Network overhead
- Pontos únicos de falha (SPOF)

**Mitigação:**
- Comunicação assíncrona quando possível
- Filas de mensagens (RabbitMQ, Kafka, SQS)
- Cache distribuído (Redis, Memcached)
- Circuit breakers (Hystrix, Resilience4j)
- Timeouts e retry policies
- Event-driven architecture

#### 💾 Consistência de Dados

**Problema:**
- Cada serviço gerencia seus próprios dados
- Dificulta garantir consistência entre serviços
- Transações distribuídas complexas
- ACID tradicional não se aplica facilmente

**Mitigação:**
- **Consistência eventual** (eventual consistency)
- Saga pattern para transações distribuídas
- Event sourcing
- CQRS (Command Query Responsibility Segregation)
- Compensação em caso de falhas

#### 📊 Gerenciamento e Observabilidade

**Problema:**
- Cultura DevOps sólida é essencial
- Correlacionar logs entre serviços é complicado
- Múltiplas chamadas de serviço para uma operação de usuário
- Rastreamento de erros distribuídos

**Mitigação:**
- **Observabilidade como requisito:**
  - Logs centralizados (ELK, Splunk, Datadog)
  - Métricas (Prometheus, Grafana)
  - Traces distribuídos (Jaeger, Zipkin, OpenTelemetry)
- Correlation IDs em todas as requisições
- APM (Application Performance Monitoring)
- Dashboards em tempo real

#### 🔄 Controle de Versão

**Problema:**
- Atualização em um serviço não deve quebrar dependentes
- Compatibilidade entre versões
- Serviços atualizados independentemente

**Mitigação:**
- Versionamento semântico (SemVer)
- API versioning (v1, v2 na URL)
- Backward compatibility sempre que possível
- Deprecation gradual
- Contract-first development

#### 👨‍💻 Competências da Equipe

**Problema:**
- Sistemas distribuídos são mais complexos
- Requer habilidades específicas:
  - Integração contínua (CI/CD)
  - Automação de infraestrutura (IaC)
  - Monitoramento de sistemas distribuídos
  - Debugging distribuído

**Mitigação:**
- Treinamento e capacitação contínua
- Começar com sistema híbrido (não full microservices)
- Adoção gradual
- Pair programming e mentoria
- Documentação robusta

### 2.6 Exemplo Prático: Sistema Refatorado para Microsserviços

#### Transformação do Sistema de Streaming

**⚠️ Observação Importante:**
Este exemplo demonstra a separação em microsserviços, mas **nem todos os pontos** foram implementados na época. É uma **exemplificação didática**.

#### Nova Arquitetura Proposta

```
                    Camada WEB
                        │
        ┌───────────────┼───────────────┐
        │               │               │
 Cortar os vídeos  Converter os   Receber dados
                     vídeos       do Satélite
        │               │               │
        └───────────────┼───────────────┘
                        │
                 Fazer o upload
```

#### Microsserviços Identificados

**1. Microsserviço: Receber Dados do Satélite**

**Responsabilidades:**
- Receber dados do satélite via master
- Armazenar em banco de dados próprio
- Salvar arquivos em storage dedicado

**Características:**
- Banco de dados independente
- Storage dedicado
- API para notificar outros serviços

**2. Microsserviço: Converter Vídeos**

**Responsabilidades:**
- Ler banco de dados (ou consumir fila)
- Buscar caminho do arquivo no disco
- Converter para formato compatível com navegadores
- Notificar conversão concluída

**Características:**
- Processamento assíncrono
- Pode ser escalado horizontalmente
- Workers independentes

**3. Microsserviço: Interface (Camada WEB)**

**Responsabilidades:**
- Apresentar dados ao usuário
- Orquestrar chamadas aos serviços
- BFF (Backend for Frontend)

**Características:**
- Ator condutor (arquitetura hexagonal)
- Sem lógica de negócio complexa
- Stateless

**4. Microsserviço: Cortar Vídeos**

**Responsabilidades:**
- Buscar trechos solicitados por usuários
- Criar clips de vídeo
- Processar edições

**Características:**
- Serviço sob demanda
- Operações intensivas de I/O
- Cache de resultados

**5. Microsserviço: Upload para Redes Sociais**

**Responsabilidades:**
- Receber dados por fila
- Realizar upload para rede social específica
- Gerenciar autenticação com APIs externas
- Retry logic para falhas

**Características:**
- Consumidor de fila
- Idempotente
- Rate limiting
- Circuit breaker para APIs externas

#### Comparação: Antes vs Depois

**Arquitetura Monolítica (Antes):**
```
┌─────────────────────────────────────────┐
│                                         │
│    APLICAÇÃO ÚNICA                      │
│    - Todos os componentes juntos        │
│    - Um banco de dados                  │
│    - Deploy único                       │
│    - Escala tudo junto                  │
│                                         │
└─────────────────────────────────────────┘
                    ↓
              💾 Database
```

**Arquitetura de Microsserviços (Depois):**
```
        ┌──────────────┐
        │   Gateway    │
        └──────┬───────┘
               │
    ┌──────────┼──────────┐
    │          │          │
┌───▼───┐  ┌──▼───┐  ┌──▼────┐
│ MS #1 │  │ MS #2│  │ MS #3 │
└───┬───┘  └──┬───┘  └──┬────┘
    │         │         │
  💾DB1     💾DB2     💾DB3
```

#### Benefícios Obtidos na Refatoração

✅ **Isolamento de Falhas**
- Falha em upload não afeta conversão
- Conversão com problemas não impede recebimento de dados

✅ **Escalabilidade Granular**
- Converter vídeos é intensivo: escala só esse serviço
- Upload menos intensivo: menos instâncias

✅ **Deploy Independente**
- Nova integração de rede social: deploy apenas do serviço de upload
- Correção de bug na conversão: não afeta outros serviços

✅ **Tecnologias Específicas**
- Conversão: usar FFmpeg otimizado em container específico
- Upload: usar SDK da rede social em linguagem apropriada

✅ **Manutenção Simplificada**
- Código menor e mais focado
- Equipes especializadas por domínio
- Menos efeitos colaterais

---

## 🏗️ 3. ARQUITETURA EM CAMADAS (LAYERED ARCHITECTURE)

### 3.1 Conceito

Padrão arquitetural que organiza o sistema em **camadas horizontais**, onde cada camada tem uma **responsabilidade específica** e se comunica apenas com as camadas adjacentes.

### 3.2 Camadas Tradicionais

```
┌─────────────────────────────────┐
│     PRESENTATION LAYER          │  ← UI, Controllers, Views
├─────────────────────────────────┤
│      BUSINESS LAYER             │  ← Business Logic, Services
├─────────────────────────────────┤
│    PERSISTENCE LAYER            │  ← Repositories, Data Access
├─────────────────────────────────┤
│       DATABASE LAYER            │  ← Database, File System
└─────────────────────────────────┘
```

### 3.3 Exemplo no Projeto da TV Bandeirantes

```
Solution 'FIAP' (4 of 4 projects)
│
├── 📁 Data          ← Acesso a dados, contexto EF, migrations
│
├── 📁 Model         ← Entidades de domínio, DTOs
│
├── 📁 MVC           ← Controllers, Views, ViewModels
│
└── 📁 Service       ← Regras de negócio, validações
```

### 3.4 Vantagens da Arquitetura em Camadas

| Vantagem | Descrição |
|----------|-----------|
| **Separação de Responsabilidades** | Cada camada tem papel bem definido |
| **Manutenibilidade** | Alterações isoladas por camada |
| **Testabilidade** | Testes unitários por camada |
| **Substituibilidade** | Trocar implementação de uma camada sem afetar outras |

### 3.5 Desvantagens

| Desvantagem | Descrição |
|-------------|-----------|
| **Acoplamento Vertical** | Dependência forte entre camadas |
| **Performance** | Overhead de múltiplas camadas |
| **Rigidez** | Dificulta mudanças cross-cutting |

---

## 🔄 4. MIGRAÇÃO: MONOLITO PARA MICROSSERVIÇOS

### 4.1 Quando Considerar a Migração?

✅ **Indicadores de que é hora de migrar:**
- Sistema muito grande e difícil de manter
- Deploy lento e arriscado
- Escalabilidade limitada
- Equipes grandes com conflitos de código
- Necessidade de tecnologias diferentes por módulo
- Alta frequência de mudanças em áreas específicas

❌ **Quando NÃO migrar:**
- Sistema pequeno e estável
- Equipe pequena sem experiência em distribuídos
- Requisitos de latência muito baixos
- Domínio simples e bem definido

### 4.2 Estratégias de Migração

#### 1️⃣ Strangler Fig Pattern

**Conceito:**
Gradualmente substituir partes do monolito por microsserviços, até que o monolito "morra" naturalmente.

```
Fase 1:              Fase 2:              Fase 3:
┌──────────┐        ┌──────────┐        ┌─────┐
│          │        │          │        │ MS3 │
│ MONOLITO │   →    │ MONOLITO │   →    ├─────┤
│          │        ├──────────┤        │ MS2 │
└──────────┘        │   MS1    │        ├─────┤
                    └──────────┘        │ MS1 │
                                        └─────┘
```

**Passos:**
1. Identificar bounded context (DDD)
2. Extrair funcionalidade para novo microsserviço
3. Redirecionar tráfego gradualmente
4. Remover código do monolito quando serviço estável

#### 2️⃣ Database per Service

**Princípio:**
Cada microsserviço tem seu próprio banco de dados.

**Antes (Monolito):**
```
┌──────────────┐
│   Monolito   │
└──────┬───────┘
       │
   ┌───▼────┐
   │   DB   │
   │ Único  │
   └────────┘
```

**Depois (Microsserviços):**
```
┌─────┐  ┌─────┐  ┌─────┐
│ MS1 │  │ MS2 │  │ MS3 │
└──┬──┘  └──┬──┘  └──┬──┘
   │        │        │
┌──▼──┐  ┌─▼───┐  ┌─▼───┐
│ DB1 │  │ DB2 │  │ DB3 │
└─────┘  └─────┘  └─────┘
```

#### 3️⃣ API Gateway Pattern

**Função:**
Ponto único de entrada que roteia requisições para microsserviços apropriados.

```
          Cliente
             │
             ▼
      ┌─────────────┐
      │ API Gateway │
      └──────┬──────┘
             │
    ┌────────┼────────┐
    │        │        │
┌───▼──┐ ┌──▼───┐ ┌──▼───┐
│ MS A │ │ MS B │ │ MS C │
└──────┘ └──────┘ └──────┘
```

**Responsabilidades do Gateway:**
- Roteamento
- Autenticação/Autorização
- Rate limiting
- Caching
- Request/Response transformation
- Load balancing
- Monitoring e logging

### 4.3 Padrões de Comunicação

#### Síncrona (REST, gRPC)

```
Cliente → [HTTP/REST] → Microsserviço A
                              ↓
                        [HTTP/REST]
                              ↓
                        Microsserviço B
```

**Vantagens:**
- Simplicidade
- Feedback imediato
- Fácil debugging

**Desvantagens:**
- Acoplamento temporal
- Latência acumulada
- Falha em cascata

#### Assíncrona (Mensageria)

```
Microsserviço A → [Publica] → Fila/Tópico → [Consome] → Microsserviço B
```

**Vantagens:**
- Desacoplamento
- Resiliência
- Escalabilidade

**Desvantagens:**
- Complexidade
- Eventual consistency
- Difícil debugging

### 4.4 Checklist de Migração

- [ ] Identificar bounded contexts
- [ ] Definir estratégia de banco de dados
- [ ] Escolher padrões de comunicação
- [ ] Implementar API Gateway
- [ ] Configurar service discovery
- [ ] Implementar circuit breakers
- [ ] Configurar logging centralizado
- [ ] Implementar distributed tracing
- [ ] Definir estratégia de deploy
- [ ] Configurar monitoramento
- [ ] Treinar equipe

---

## 📊 5. MERCADO, CASES E TENDÊNCIAS

### 5.1 Mercado e Adoção

**Estatísticas:**
- 85% das grandes empresas adotaram ou estão adotando microsserviços
- Time-to-market **50% mais rápido** em média
- **Redução de 70%** no tempo de deploy
- **Aumento de 60%** na frequência de releases

### 5.2 Transição Arquitetural

**Mudança de Paradigma:**
- De: decisão puramente técnica
- Para: transformação na forma de projetar, entregar e operar sistemas

**Drivers de adoção:**
- ✅ Escalabilidade
- ✅ Modularidade
- ✅ Independência entre equipes
- ✅ Time-to-market crítico
- ✅ Entregas frequentes sem impacto

### 5.3 Estratégias por Tipo de Empresa

#### Startups

**Abordagem:**
- Experimentação rápida
- Começar com monolito modular
- Migrar para microsserviços quando escalar
- Componentização gradual

**Foco:**
- Velocidade de iteração
- Validação de hipóteses
- MVP rápido

#### Grandes Organizações

**Abordagem:**
- Modernização de sistemas legados
- Migração gradual (strangler fig)
- Partes críticas primeiro
- Coexistência híbrida

**Foco:**
- Redução de risco
- Continuidade de negócio
- ROI comprovado

### 5.4 Cases de Sucesso

#### Mercado Livre

**Escala:**
- Milhões de requisições por dia
- Alta disponibilidade (99.9%+)
- Resiliência comprovada

**Resultados:**
- Arquitetura distribuída robusta
- Escalabilidade horizontal
- Isolamento de falhas
- Deploy contínuo

#### Netflix

**Pioneirismo:**
- Referência mundial em microsserviços
- Chaos Engineering
- Ferramentas open source (Hystrix, Eureka, Zuul)

**Lições:**
- Resiliência é design, não add-on
- Automação é essencial
- Cultura DevOps fundamental

### 5.5 Cultura DevOps e Automação

**Maturidade Necessária:**
- Pipelines de CI/CD
- Containers (Docker)
- Orquestração (Kubernetes)
- Monitoramento distribuído
- Infraestrutura como código (IaC)

**Habilidades Profissionais Exigidas:**
- Desenvolvimento
- Operações
- Observabilidade
- Controle de versões entre serviços
- Debugging distribuído

### 5.6 Tendência: Composição de Serviços e APIs

**Conceito:**
Sistemas são compostos por **blocos reutilizáveis** (microsserviços) que podem ser combinados para formar novas experiências.

**Aplicações:**
- Integrações com parceiros externos
- Redes sociais
- Pagamentos
- Analytics
- IoT

**Tecnologias Habilitadoras:**
- API Gateways
- Comunicação assíncrona por eventos
- Segurança distribuída (OAuth, JWT)
- Service mesh

---

## 🎯 6. BOAS PRÁTICAS E RECOMENDAÇÕES

### 6.1 Design de Microsserviços

#### ✅ Princípios SOLID

- **S** - Single Responsibility Principle
- **O** - Open/Closed Principle
- **L** - Liskov Substitution Principle
- **I** - Interface Segregation Principle
- **D** - Dependency Inversion Principle

#### ✅ Domain-Driven Design (DDD)

**Bounded Contexts:**
- Definir limites claros entre contextos
- Microsserviços devem respeitar bounded contexts
- Linguagem ubíqua por contexto

**Aggregates:**
- Unidade de consistência transacional
- Geralmente um microsserviço por aggregate

#### ✅ Single Responsibility

**Um microsserviço deve:**
- Fazer **uma coisa** muito bem
- Ter **uma razão** para mudar
- Ser **coeso** internamente

#### ✅ Stateless quando possível

- Microsserviços devem ser preferencialmente stateless
- Estado persistido em banco de dados
- Cache distribuído quando necessário
- Facilita escalabilidade horizontal

### 6.2 Comunicação

#### ✅ Contratos de API bem definidos

**OpenAPI/Swagger:**
- Documentação automática
- Contract-first development
- Client code generation

**Versionamento:**
- Semantic versioning
- Deprecation gradual
- Backward compatibility

#### ✅ Idempotência

**Operações devem ser idempotentes:**
- Múltiplas execuções = mesmo resultado
- Crítico em comunicação assíncrona
- Previne duplicação de processamento

#### ✅ Circuit Breakers

**Implementação:**
```
Estado CLOSED (normal)
    ↓ (muitas falhas)
Estado OPEN (bloqueado)
    ↓ (timeout)
Estado HALF-OPEN (testando)
    ↓ (sucesso)
Estado CLOSED
```

**Ferramentas:**
- Hystrix (Netflix)
- Resilience4j
- Polly (.NET)

### 6.3 Dados

#### ✅ Database per Service

**Cada microsserviço:**
- Tem seu próprio banco de dados
- Não compartilha esquema
- Não acessa banco de outros serviços

#### ✅ Eventual Consistency

**Aceitar:**
- Dados podem estar temporariamente inconsistentes
- Sistemas eventualmente convergem
- ACID substituído por BASE (Basically Available, Soft state, Eventual consistency)

#### ✅ Saga Pattern

**Para transações distribuídas:**
```
Serviço A: Reservar produto
    ↓ (sucesso)
Serviço B: Processar pagamento
    ↓ (falha)
Serviço A: Compensação - Liberar reserva
```

### 6.4 Observabilidade

#### ✅ Três Pilares

**1. Logs**
- Logs estruturados (JSON)
- Correlation ID em todas as requisições
- Centralização (ELK, Splunk, Datadog)

**2. Métricas**
- Latência
- Taxa de erro
- Throughput
- Saturação (CPU, memória, disco)

**3. Traces**
- Distributed tracing (Jaeger, Zipkin)
- OpenTelemetry
- Visualização de fluxo completo

#### ✅ Health Checks

```
GET /health
{
  "status": "UP",
  "components": {
    "database": "UP",
    "queue": "UP",
    "cache": "UP"
  }
}
```

### 6.5 Segurança

#### ✅ Zero Trust Architecture

**Princípio:**
- Nunca confiar, sempre verificar
- Autenticação em todas as camadas
- Criptografia everywhere

#### ✅ OAuth 2.0 / JWT

**Para autenticação distribuída:**
- Tokens stateless
- Claims e scopes
- Refresh tokens

#### ✅ API Gateway

**Centraliza:**
- Autenticação
- Autorização
- Rate limiting
- Input validation

### 6.6 Deployment

#### ✅ Blue-Green Deployment

```
Produção → [Blue] ← Tráfego 100%
Staging →  [Green] ← Deploy nova versão
           ↓
Produção → [Green] ← Switch de tráfego
Antigo →   [Blue] ← Rollback rápido se necessário
```

#### ✅ Canary Releases

```
Versão Antiga → 90% do tráfego
Versão Nova   → 10% do tráfego
              ↓ (monitorar métricas)
Aumentar gradualmente para 100%
```

#### ✅ Feature Flags

**Controle de features:**
- Habilita/desabilita features em runtime
- A/B testing
- Gradual rollout
- Kill switch em caso de problemas

---

## 📚 7. GLOSSÁRIO

**API (Application Programming Interface):** Interface que define como sistemas se comunicam.

**API Gateway:** Ponto único de entrada que roteia requisições para microsserviços.

**Bounded Context:** Limite lógico de um modelo de domínio (DDD).

**Circuit Breaker:** Padrão que previne falhas em cascata.

**Consistency:** Garantia de que dados estão sincronizados.

**Container:** Unidade de software que empacota código e dependências.

**Correlation ID:** Identificador único que rastreia requisição através de múltiplos serviços.

**Eventual Consistency:** Modelo onde dados convergem para consistência ao longo do tempo.

**Idempotent:** Operação que produz mesmo resultado quando executada múltiplas vezes.

**IaC (Infrastructure as Code):** Gerenciamento de infraestrutura via código.

**Monolito:** Aplicação onde todos os componentes estão em uma única unidade.

**Microsserviço:** Serviço pequeno, independente e focado em uma única responsabilidade.

**REST (Representational State Transfer):** Estilo arquitetural para APIs baseadas em HTTP.

**Saga:** Padrão para gerenciar transações distribuídas.

**Service Discovery:** Mecanismo para localizar serviços dinamicamente.

**Service Mesh:** Camada de infraestrutura para comunicação entre serviços.

**Stateless:** Serviço que não mantém estado entre requisições.

**Strangler Fig Pattern:** Estratégia de migração gradual de monolito para microsserviços.

---

## ✅ 8. CHECKLIST DE TRANSIÇÃO

### Planejamento

- [ ] Identificar bounded contexts (DDD)
- [ ] Mapear dependências entre módulos
- [ ] Definir ordem de migração
- [ ] Estimar esforço e riscos
- [ ] Obter buy-in de stakeholders

### Infraestrutura

- [ ] Configurar ambiente de containers (Docker)
- [ ] Configurar orquestração (Kubernetes)
- [ ] Implementar service discovery
- [ ] Configurar API Gateway
- [ ] Configurar load balancers

### Observabilidade

- [ ] Implementar logging centralizado
- [ ] Configurar métricas (Prometheus/Grafana)
- [ ] Implementar distributed tracing
- [ ] Configurar alertas
- [ ] Criar dashboards de monitoramento

### Comunicação

- [ ] Definir padrões de comunicação (REST/mensageria)
- [ ] Implementar circuit breakers
- [ ] Configurar retry policies
- [ ] Implementar timeouts
- [ ] Documentar APIs (OpenAPI)

### Dados

- [ ] Definir estratégia de dados
- [ ] Implementar database per service
- [ ] Configurar replicação se necessário
- [ ] Implementar eventual consistency
- [ ] Implementar saga pattern para transações distribuídas

### Segurança

- [ ] Implementar autenticação centralizada
- [ ] Configurar autorização por serviço
- [ ] Implementar rate limiting
- [ ] Configurar TLS/HTTPS
- [ ] Implementar secrets management

### CI/CD

- [ ] Configurar pipelines por serviço
- [ ] Implementar testes automatizados
- [ ] Configurar deploy automatizado
- [ ] Implementar blue-green ou canary
- [ ] Configurar rollback automatizado

### Equipe

- [ ] Treinar equipe em microsserviços
- [ ] Treinar equipe em containers/K8s
- [ ] Treinar equipe em observabilidade
- [ ] Estabelecer práticas DevOps
- [ ] Definir ownership por serviço

---

## 🎓 9. PRÓXIMOS PASSOS

### Para Aprofundamento

**Conceitos:**
- Domain-Driven Design (DDD)
- Event-Driven Architecture
- CQRS (Command Query Responsibility Segregation)
- Event Sourcing
- Service Mesh (Istio, Linkerd)

**Tecnologias:**
- Docker e Docker Compose
- Kubernetes
- API Gateways (Kong, Traefik, Nginx)
- Message Brokers (RabbitMQ, Kafka, SQS)
- Service Discovery (Consul, Eureka)

**Observabilidade:**
- ELK Stack (Elasticsearch, Logstash, Kibana)
- Prometheus + Grafana
- Jaeger / Zipkin
- OpenTelemetry
- Datadog / New Relic

**Práticas:**
- 12 Factor App
- CI/CD avançado
- Chaos Engineering
- SRE (Site Reliability Engineering)

### Recursos Recomendados

**Livros:**
- "Building Microservices" - Sam Newman
- "Microservices Patterns" - Chris Richardson
- "Domain-Driven Design" - Eric Evans
- "Designing Data-Intensive Applications" - Martin Kleppmann

**Cursos:**
- Kubernetes completo
- Docker para desenvolvedores
- Design de APIs REST
- Arquitetura de software moderna

**Comunidades:**
- Cloud Native Computing Foundation (CNCF)
- Microservices.io
- DDD Community
- DevOps communities

---

## 📝 RESUMO FINAL

Esta aula apresentou a **evolução arquitetural** de sistemas de software, começando pela **arquitetura monolítica tradicional**, passando pela **arquitetura em camadas**, até chegar aos **microsserviços modernos**.

**Pontos-chave:**

1. **Monolitos** são adequados para sistemas pequenos, mas se tornam difíceis de manter e escalar conforme crescem.

2. **Microsserviços** oferecem benefícios significativos em modularidade, escalabilidade e velocidade de entrega, mas introduzem complexidade na comunicação, dados e operações.

3. A **transição** de monolito para microsserviços deve ser **gradual e planejada**, considerando as capacidades da equipe e necessidades do negócio.

4. **DevOps**, **observabilidade** e **automação** são pilares fundamentais para o sucesso com microsserviços.

5. Não existe "bala de prata" - cada arquitetura tem seus trade-offs e deve ser escolhida baseada no contexto específico do projeto.

**Lembre-se:** A decisão arquitetural deve considerar não apenas aspectos técnicos, mas também **maturidade da equipe**, **cultura organizacional** e **requisitos de negócio**.

---

**Gerado para:** Jamal - DevOps & Arquitetura Cloud (360h)  
**Programa:** FIAP + Alura POSTECH  
**Fase:** 1 - Arquitetura de Aplicações  
**Data:** Dezembro 2025