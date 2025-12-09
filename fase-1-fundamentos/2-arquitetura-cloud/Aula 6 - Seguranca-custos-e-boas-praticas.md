# Arquitetura Cloud - Aula 06: Segurança, Custos e Boas Práticas

## 📋 Resumo Executivo

Esta aula aborda os pilares fundamentais da gestão de ambientes cloud: segurança robusta, gestão inteligente de custos e aplicação de boas práticas arquitetônicas. O conteúdo capacita para transitar de uma postura reativa para uma gestão estratégica e otimizada da nuvem.

---

## 🔐 1. SEGURANÇA NA NUVEM

### 1.1 Panorama de Ameaças

- Até 2025: segurança na nuvem pode consumir **20% do orçamento total de cibersegurança**
- Ameaças crescentes exigem investimento estratégico, não apenas operacional
- Ambientes híbridos/multi-nuvem aumentam complexidade de detecção

### 1.2 Modelo de Responsabilidade Compartilhada (SRM)

**Divisão de responsabilidades:**

| Responsável | Escopo | O que inclui |
|-------------|--------|--------------|
| **Provedor** | Segurança **DA** nuvem | Infraestrutura física, hardware, software, rede, instalações |
| **Cliente** | Segurança **NA** nuvem | Dados, configurações de rede, SO, aplicações, IAM |

**Variação por modelo de serviço:**
- **IaaS:** Cliente tem mais responsabilidades (SO, runtime, aplicações)
- **PaaS:** Responsabilidades compartilhadas em runtime e middleware
- **SaaS:** Cliente foca em dados e controle de acesso

**Desafios principais:**
- Falta de compreensão aprofundada do modelo
- Complexidade em ambientes multi-nuvem
- Erros de configuração = causa comum de violações
- Visibilidade limitada sobre camadas inferiores da infraestrutura

### 1.3 Gerenciamento de Identidade e Acesso (IAM)

#### Conceitos Fundamentais

**Autenticação**
- Verificação de identidade (quem você é)
- **MFA (Autenticação Multifator):** adiciona camada extra de segurança
  - "Algo que você sabe" (senha, PIN)
  - "Algo que você tem" (token, notificação push, SMS/OTP)
  - "Algo que você é" (biometria)
- **SSO (Single Sign-On):** autenticação única para múltiplos recursos
- **FIDO2:** padrão mais seguro de MFA

**Autorização**
- Define nível e tipo de acesso permitido
- Determina quais ações podem ser realizadas

**Federação de Identidade**
- Permite usar credenciais existentes de outros sistemas
- Elimina necessidade de múltiplos logins
- Facilita integração corporativa

**Roles (Funções)**
- Agrupamento de usuários com privilégios similares
- Herança automática de permissões
- Gestão eficiente em larga escala

**Políticas**
- Documentos formais que definem permissões
- Especificam ações permitidas em recursos
- Base para princípio do privilégio mínimo

### 1.4 Criptografia

#### Criptografia em Trânsito (TLS/SSL)

**Processo:**
1. Criptografia dos dados antes da transmissão
2. Autenticação dos endpoints
3. Descriptografia e verificação de integridade na chegada

**Implementações:**
- **TLS (Transport Layer Security):** protocolo padrão
- **QUIC:** protocolo moderno do Google
- **Forward Secrecy:** proteção contra comprometimento futuro de chaves
- **BoringSSL:** implementação open source do Google

**Aplicações:**
- Comunicação entre site e provedor cloud
- Tráfego entre dois serviços
- Tráfego VM-to-VM dentro de VPC
- Criptografia em camadas de rede (AES-128-GCM)

#### Criptografia em Repouso (KMS)

**Características:**
- **AES-256:** padrão de criptografia
- Proteção durante armazenamento
- Criptografia por padrão (ex: Google Cloud)

**Cloud KMS (Key Management Service):**
- Criação e gestão de chaves próprias
- Criptografia de envelope
- Funcionalidades: criar, rotacionar, rastrear, excluir chaves

**Hierarquia de Chaves:**
```
Dados → Criptografados por DEK (Data Encryption Key)
DEK → Criptografada por KEK (Key Encryption Key)
KEK → Armazenada em Keystore centralizado
```

**Benefícios:**
- Proteção contra atacantes (dados ilegíveis sem chaves)
- Redução da superfície de ataque
- Controle de acesso centralizado
- Foco em proteção das chaves
- Aumento da privacidade

### 1.5 Segurança de Rede

#### Virtual Private Cloud (VPC)

**Conceito:**
- Segmento de rede isolado no provedor cloud
- Controle total sobre conectividade, roteamento e segurança
- Replica funcionalidade de rede on-premise

**Segmentação de Rede:**

| Tipo de Sub-rede | Propósito | Exemplos |
|------------------|-----------|----------|
| **Pública** | Acesso direto à internet | Load Balancers |
| **Privada** | Sem acesso direto à internet | Servidores de aplicação, bancos de dados |
| **Protegida** | Tráfego não sai da VPC | Bancos de dados críticos |

**Alta Disponibilidade:**
- Distribuição entre múltiplas Zonas de Disponibilidade (AZs)
- Aumenta resiliência da arquitetura

#### Network Access Control Lists (NACLs)

**Características:**
- Firewall **stateless** no nível da sub-rede
- Regras de permissão E negação
- Processamento em ordem numérica

**Configurações:**
- NACL padrão: permite todo tráfego
- NACLs personalizadas: negam todo tráfego por padrão

#### Security Groups (Grupos de Segurança)

**Características:**
- Firewall **stateful** no nível da ENI (Elastic Network Interface)
- Se entrada permitida → saída automática (e vice-versa)
- Cada recurso com ENI precisa de pelo menos um security group

**Configurações:**
- Grupo padrão: permite tráfego de saída e entrada entre recursos do mesmo grupo
- Grupos personalizados: permitem saída, negam entrada por padrão

#### AWS Web Application Firewall (WAF)

**Função:**
- Proteção de aplicações web públicas
- Primeira linha de defesa
- Bloqueia tráfego malicioso próximo à origem

**Vantagens:**
- Totalmente gerenciado
- Integração com múltiplos serviços AWS
- Regras pré-construídas disponíveis

#### AWS Network Firewall

**Função:**
- Segurança de perímetro de VPCs
- Totalmente gerenciado
- Alta disponibilidade e escalabilidade

**Recursos:**
- Firewall stateful
- Filtragem web
- Prevenção de intrusões
- Centralização de segurança multi-VPC

#### VPN (Virtual Private Network)

**Função:**
- Conexão segura entre VPC e rede on-premise via internet

**Componentes:**
- Virtual Private Gateway
- Customer Gateway
- Site-to-Site VPN Connection

#### Direct Connect

**Função:**
- Conexão de rede dedicada entre AWS e data center on-premise

**Vantagens:**
- Maior largura de banda que VPN
- Menor latência
- Não depende da internet pública

**Componentes:**
- Interface virtual
- Conexão física dedicada

---

## 💰 2. GESTÃO INTELIGENTE DE CUSTOS

### 2.1 FinOps (Financial Operations)

**Definição:**
Prática colaborativa que integra equipes de finanças, engenharia e negócios para gerenciar e otimizar custos da nuvem.

**Disciplinas emergentes:**
- **CapOps:** DevOps + FinOps para otimizar computação e armazenamento
- **RevOps:** Gestão do ciclo de receita e contenção de custos

### 2.2 Estratégias de Otimização de Custos

#### Otimização de Armazenamento

**Ações:**
- Identificar e eliminar snapshots não utilizados
- Remover volumes EBS não anexados
- Configurar políticas de ciclo de vida de dados
- Provisionar camadas adequadas de armazenamento

**Técnicas:**
- Limpeza de dados
- Compressão
- Indexação
- Agregação
- Particionamento
- Deduplicação

#### Computação Serverless

**Características:**
- Recursos utilizados apenas quando necessário
- Faturamento baseado em duração do processo
- Elimina custos de manutenção de servidores
- Sem gerenciamento de infraestrutura

#### Autoscaling e Balanceamento de Carga

**Benefícios:**
- Adição/remoção automática de servidores
- Baseado na carga atual
- Utilização eficiente de recursos
- Ajuste dinâmico à demanda

#### Contêineres e Orquestração

**Requisito:**
- Relatórios de custo por contêiner individual
- Visibilidade sobre gastos em clusters
- Rastreamento granular

#### Instâncias Spot / VMs de Baixa Prioridade

**Características:**
- Aproveitamento de infraestrutura ociosa
- Grandes descontos
- **Risco:** podem ser terminadas se surgir demanda de maior preço

**Casos de uso:**
- Cargas de trabalho tolerantes a interrupção
- Processamento em batch
- Análises não críticas

#### Instâncias Reservadas (RIs) e Savings Plans

**Características:**
- Compromisso de uso de longo prazo
- Descontos significativos
- Ideal para cargas previsíveis

**Ferramentas:**
- AWS Cost Management
- Azure Advisor
- Recomendações automáticas

#### Otimização de Transferência de Dados

**Estratégias:**
- Reduzir transferências inter-região
- Utilizar CDNs (Content Delivery Networks)
- Implementar caching de dados
- Considerar estratégia de preços do provedor por região/AZ

#### Agendamento de Recursos

**Aplicação:**
- Desligar recursos não utilizados
- Exemplo: servidores de desenvolvimento
  - Desligar no final do expediente
  - Ligar novamente pela manhã
- Automação via scripts ou ferramentas

#### Otimização de Computação

**Processo:**
1. Monitorar uso de CPU e memória
2. Identificar recursos superprovisionados (uso 0-20%)
3. Redimensionar para capacidade apropriada
4. Revisar periodicamente

#### Otimização de Rede

**Ações:**
- Remover Load Balancers não utilizados
- Liberar endereços IP públicos ociosos
- Redimensionar Gateways subutilizados
- Desativar NVAs (Network Virtual Appliances) desnecessários

#### Automação e IaC (Infraestrutura como Código)

**Benefícios:**
- Criação de infraestrutura sob demanda (minutos)
- Eliminação de recursos persistentes desnecessários
- Contas inteiras podem ser provisionadas rapidamente
- Ambientes efêmeros (teste, desenvolvimento)

### 2.3 Ferramentas Nativas dos Provedores

#### Amazon Web Services (AWS)

| Ferramenta | Função |
|------------|--------|
| **AWS Trusted Advisor** | Identifica instâncias ociosas, volumes subutilizados |
| **AWS Compute Optimizer** | Recomendações de dimensionamento |
| **AWS Cost Management** | Recomendações de RI e Savings Plans |
| **Amazon S3 Storage Lens** | Análise de armazenamento S3 |

#### Microsoft Azure

| Ferramenta | Função |
|------------|--------|
| **Azure Advisor** | Recomendações de RI, Savings Plans |
| **Azure Monitor** | Monitoramento de recursos |
| **Azure Resource Graph** | Consultas em recursos |
| **Azure Network Watcher** | Análise de rede |

#### Google Cloud Platform (GCP)

| Ferramenta | Função |
|------------|--------|
| **Cost Recommender** | Recursos subutilizados, auto-scaling, redimensionamento |
| **Recommendations** | RI, Spot Instances |
| **Tags** | Alocação de custos |

### 2.4 Processo de Otimização (4 Passos)

#### 1. Buscar Oportunidades
- Examinar potencial de economia por categoria
- Utilizar ferramentas nativas para análise profunda
- Identificar padrões de desperdício

#### 2. Identificar e Refinar
- Revisar lista completa de oportunidades
- Validar precisão das recomendações
- Priorizar: esforço vs economia
- Considerar: tempo, recursos, urgência, risco

#### 3. Executar Ações
- Avaliar utilidade dos recursos
- Determinar prazos
- Otimizar tempos de execução
- Minimizar transferências inter-região
- Gerenciar ciclo de vida do armazenamento
- Limpar infraestrutura desnecessária
- Evitar duplicação de dados
- Entender requisitos de HA/DR
- Habilitar versionamento
- Utilizar IaC
- Implementar serverless
- Configurar autoscaling
- Reduzir custos de consulta
- Ajustar nível de logging

#### 4. Relatar e Examinar
- Comunicar economias identificadas
- Reportar economias alcançadas
- Disposição das recomendações
- Planejar ações futuras
- Feedback loop contínuo

---

## 🏛️ 3. GOVERNANÇA DA NUVEM

### 3.1 Conceito

Criação, implementação e monitoramento de políticas que orientam operações em ambientes cloud, garantindo:
- Desempenho consistente
- Alinhamento com objetivos de negócio
- Segurança de dados
- Gerenciamento de riscos
- Conformidade legal
- Controle de custos
- Eficiência e colaboração

### 3.2 Tagging (Marcação)

#### Fundamento da Governança

**Definição:**
Metadados específicos da organização adicionados aos recursos cloud.

**Finalidades:**
- Alocação de custos
- Relatórios financeiros
- Otimização de recursos
- Conformidade regulatória
- Segurança e auditoria

#### Exemplos de Tags

```
ambiente = teste
custo = projeto
db = database
compliance = hipaa
time = 12x05
```

#### Política de Tagging Consistente

**Convenções de nomenclatura:**
- Ortografia padronizada
- Capitalização uniforme
- Espaçamento consistente
- Aplicação uniforme entre equipes

#### Limitações por Provedor

| Provedor | Limitações |
|----------|-----------|
| **Google Cloud** | Limites específicos de tags/recurso |
| **AWS** | Restrições de comprimento de chaves/valores |
| **Azure** | Regras de sensibilidade a maiúsculas |

### 3.3 Automação

#### Importância

**Benefícios:**
- Implementação eficaz de tags
- Aplicação automática de políticas
- Redução de erro humano
- Consistência em escala

#### Configurações Automatizadas

**Plataformas de gerenciamento podem:**
- Anexar tags automaticamente a recursos criados de templates
- Alertar sobre tags ausentes
- Forçar uso de tags obrigatórias
- Em ambientes rigorosos: terminar instâncias não taggeadas

**Exemplo de aplicação:**
```
Template → Recurso criado → Tags aplicadas automaticamente
                         ↓
                 Se tags ausentes → Alerta
                         ↓
        (Opcional) Terminação após X horas
```

### 3.4 Auditoria (Monitoramento)

#### Processo de Implementação em 5 Etapas

**Etapa 1: Definição da Política de Tags**
- Criar processo claro
- Envolver partes interessadas
- Obter feedback e adesão
- Documentar padrões

**Etapa 2: Relatórios Contínuos**
- Relatórios semanais da equipe de administração
- Demonstrar nível de cobertura
- Ilustrar estado atual
- Rastrear melhorias

**Etapa 3: Alertas Automatizados**
- E-mails diários ou semanais
- Identificação de recursos sem tags necessárias
- Notificação aos responsáveis

**Etapa 4: Alertas com Terminação (Opcional)**
- Período de graça (ex: 24 horas)
- Terminação automática se não taggeado
- **Apenas para ambientes não-produção**
- Alternativa: e-mail ao gerente da área

**Etapa 5: Monitoramento Contínuo**
- Relatórios semanais permanentes
- Acompanhamento de níveis de cobertura
- Demonstração de estado atual
- Tracking de melhorias ao longo do tempo

#### Progressão Disciplinada

```
Definição → Relatórios → Alertas → Terminação → Monitoramento
                                  (opcional)    Contínuo
```

**Resultado:**
- Governança deixa de ser teórica
- Torna-se prática operacional
- Mitiga erro humano
- Garante conformidade em escala
- Leva à otimização de custos e segurança

---

## 🏗️ 4. WELL-ARCHITECTED FRAMEWORK

### 4.1 Visão Geral

Framework da AWS para design de arquiteturas cloud de qualidade, fornecendo guia abrangente para sistemas:
- Eficientes
- Confiáveis
- Seguros
- Otimizados em custos

### 4.2 Os 6 Pilares

#### 1️⃣ Excelência Operacional

**Foco:**
- Suportar desenvolvimento e execução de cargas de trabalho
- Obter insights sobre operações
- Melhorar continuamente processos e procedimentos
- Entregar valor de negócio

**Práticas-chave:**
- Automação de operações
- Documentação como código
- Pequenas mudanças frequentes e reversíveis
- Antecipação de falhas
- Aprendizado com falhas operacionais

#### 2️⃣ Segurança

**Foco:**
- Alavancar tecnologias cloud para proteger dados, sistemas e ativos
- Aprimorar postura de segurança

**Áreas:**
- Gerenciamento de identidade e acesso
- Controles de detecção
- Proteção de infraestrutura
- Proteção de dados
- Resposta a incidentes

**Princípios:**
- Implementar forte identidade
- Habilitar rastreabilidade
- Aplicar segurança em todas as camadas
- Automatizar práticas de segurança
- Proteger dados em trânsito e em repouso
- Manter pessoas longe de dados
- Preparar-se para eventos de segurança

#### 3️⃣ Confiabilidade

**Foco:**
- Capacidade da carga de trabalho de realizar função pretendida
- De forma correta e consistente quando esperado
- Operar e testar durante todo o ciclo de vida

**Componentes:**
- Recuperação de falhas de infraestrutura ou serviço
- Aquisição dinâmica de recursos de computação
- Mitigação de interrupções

**Práticas:**
- Testar procedimentos de recuperação
- Recuperar-se automaticamente de falhas
- Escalar horizontalmente
- Parar de adivinhar capacidade
- Gerenciar mudanças através de automação

#### 4️⃣ Eficiência de Performance

**Foco:**
- Usar recursos de computação eficientemente
- Atender requisitos do sistema
- Manter eficiência com mudança de demanda e evolução de tecnologias

**Áreas:**
- Seleção de tipos e tamanhos de recursos
- Otimização ao longo do tempo

**Práticas:**
- Democratizar tecnologias avançadas
- Tornar-se global em minutos
- Usar arquiteturas serverless
- Experimentar com mais frequência
- Considerar simpatia mecânica

#### 5️⃣ Otimização de Custos

**Foco:**
- Executar sistemas para entregar valor de negócio ao menor preço possível

**Áreas:**
- Entendimento e controle de onde dinheiro é gasto
- Seleção de recursos apropriados
- Análise de gastos ao longo do tempo
- Escalamento para atender necessidades de negócio

**Práticas:**
- Implementar gerenciamento financeiro na nuvem
- Adotar modelo de consumo
- Medir eficiência geral
- Parar de gastar dinheiro em trabalho pesado de datacenter
- Analisar e atribuir despesas

#### 6️⃣ Sustentabilidade

**Foco (Tendência 2025):**
- Minimizar impacto ambiental
- Data centers energeticamente eficientes
- Energia renovável

**Práticas:**
- Entender seu impacto
- Estabelecer metas de sustentabilidade
- Maximizar utilização
- Antecipar e adotar novas ofertas de hardware/software mais eficientes
- Usar serviços gerenciados
- Reduzir impacto downstream de cargas de trabalho

### 4.3 Aplicação Prática e Trade-offs

#### Conceito de Trade-offs

**Realidade:**
- Não existe arquitetura perfeita em todos os pilares
- Melhorar um pilar pode impactar outro
- Decisões devem ser conscientes e documentadas

**Exemplos de Trade-offs:**

| Ação | Benefício | Trade-off Possível |
|------|-----------|-------------------|
| Aumentar segurança | Melhor proteção | Maior custo, complexidade operacional |
| Otimizar custos | Economia | Pode reduzir redundância/resiliência |
| Maximizar performance | Melhor experiência | Custo mais alto |
| Alta disponibilidade | Maior confiabilidade | Custo elevado, complexidade |

#### Processo de Avaliação

1. **Avaliar** arquitetura atual contra os pilares
2. **Identificar** áreas de melhoria
3. **Priorizar** baseado em objetivos de negócio
4. **Implementar** melhorias incrementalmente
5. **Medir** resultados
6. **Iterar** continuamente

#### Integração com Outras Práticas

**FinOps + GreenOps:**
- Otimização simultânea de custos e sustentabilidade
- Decisões holísticas
- Abordagem integrada

**IA/ML + Segurança:**
- Detecção inteligente de ameaças
- Automação de resposta
- Análise preditiva

---

## 📊 5. TENDÊNCIAS DE MERCADO 2025

### 5.1 Soluções Multi-Cloud e Nuvem Híbrida

**Drivers:**
- Dispersão geográfica de clientes
- Pilhas de tecnologia distribuídas
- Reutilização de serviços híbridos
- Adoção gradual da nuvem

**Padrões:**
- Arquiteturas orientadas a dados
- Arquiteturas orientadas a serviços
- Arquiteturas orientadas a processos

### 5.2 Edge Computing

**Características:**
- Processamento próximo à fonte de dados
- Baixa latência
- Processamento em tempo real

**Projeção:**
- Mercado global: **US$ 250 bilhões até 2025**

**Casos de uso:**
- IoT
- Veículos autônomos
- Realidade aumentada
- Streaming de vídeo

### 5.3 Computação Serverless (FaaS)

**Projeção:**
- **50%+ das empresas** globais até 2025

**Benefícios:**
- Foco no código, não na infraestrutura
- Faturamento baseado em consumo
- Escalabilidade automática
- Sem gerenciamento de servidores

### 5.4 Integração de IA e ML

**Crescimento:**
- **40% de aumento** na adoção de serviços cloud impulsionados por IA até 2025

**Aplicações:**
- Automação de processos
- Segurança aprimorada (detecção de ameaças)
- Maior acessibilidade de IA
- Soluções específicas por indústria

### 5.5 Segurança, Conformidade e Resiliência

**Investimento:**
- Segurança na nuvem = **20% dos orçamentos** de cibersegurança até 2025

**Soluções:**
- Detecção e resposta impulsionadas por IA
- Arquitetura Zero Trust (ZTA)
- Técnicas aprimoradas de criptografia
- Compliance automatizado

### 5.6 Aplicações Cloud-Native

**Adoção:**
- **80% dos novos projetos** de software até 2025

**Tecnologias:**
- Microsserviços
- Conteinerização (Kubernetes)
- DevOps
- Service Meshes
- Integração IA/ML

### 5.7 Computação Quântica na Nuvem

**Mercado:**
- Projetado para **US$ 10 bilhões até 2025**

**Ofertas:**
- IBM Quantum
- Azure Quantum
- Amazon Braket
- Plataformas de experimentação

### 5.8 DevSecOps e Automação

**Adoção:**
- **70% das empresas** com práticas DevOps cloud integradas até 2025

**Benefícios:**
- Otimização de desenvolvimento
- Operações de TI eficientes
- Segurança integrada desde o início
- Entrega contínua

### 5.9 Green Cloud Computing (Sustentabilidade)

**Compromisso:**
- **60% dos provedores** comprometidos com neutralidade de carbono até 2025

**Foco:**
- Data centers eficientes energeticamente
- Energia renovável
- Redução de pegada de carbono
- Métricas de sustentabilidade

### 5.10 Sovereign Cloud (Nuvem Soberana)

**Conceito:**
- Segurança e privacidade em conformidade com leis locais
- Dados, infraestrutura e tecnologia livres de controle externo
- Soberania digital

**Drivers:**
- LGPD e regulamentações similares
- Segurança nacional
- Proteção de dados sensíveis

### 5.11 Nuvem Industrial

**Características:**
- Soluções verticais por indústria
- Serviços centrados no domínio
- Dados de referência pré-carregados
- Casos de uso específicos

**Setores:**
- Manufatura
- Saúde
- Finanças
- Varejo
- Energia

---

## 🎯 6. LIÇÕES DE CONTENÇÃO DE VIOLAÇÕES

### 6.1 Mudança de Paradigma

**De:** Prevenção total
**Para:** Contenção rápida + Resiliência

**Reconhecimento:**
- Violações são, em certa medida, inevitáveis
- Capacidade de resposta é tão importante quanto prevenção

### 6.2 Frameworks Regulatórios Modernos

**DORA (Digital Operational Resilience Act):**
- Foco em resiliência cibernética
- Capacidade de resposta a incidentes

**NIS2 (Network and Information Security Directive):**
- Segurança de redes e informações
- Requisitos de resposta

### 6.3 Gráficos de Segurança

**Função:**
- Mapas contextuais em tempo real
- Visualização de ambientes digitais
- Compreensão de relações entre componentes
- Priorização de segurança

**Benefícios:**
- Detecção mais rápida
- Compreensão de impacto
- Contenção eficaz

### 6.4 Detecção Baseada em Contexto

**Evolução:**
- De: Detecção tradicional (algo está errado)
- Para: Detecção contextual (onde estão os riscos reais)

**Soluções como Illumio Insights:**
- Entendimento de riscos reais
- Priorização de atenção
- Impedimento de movimento lateral
- Proteção antes do impacto em sistemas críticos

### 6.5 Excelência em Serviço ao Cliente (Segurança)

**Importância:**
- Respostas rápidas impactam resultados
- Orientação especializada é crítica

**Componentes:**
- Integração prática
- Treinamento especializado
- Suporte contínuo de consultores
- Implantação eficaz
- Contenção de violações
- Interrupção de movimento lateral

---

## ✅ 7. CHECKLIST DE BOAS PRÁTICAS

### Segurança

- [ ] Compreender e aplicar Modelo de Responsabilidade Compartilhada
- [ ] Implementar IAM com princípio do privilégio mínimo
- [ ] Configurar MFA para todos os usuários privilegiados
- [ ] Ativar criptografia em trânsito (TLS/SSL)
- [ ] Ativar criptografia em repouso (KMS)
- [ ] Configurar VPC com segmentação adequada
- [ ] Aplicar Security Groups restritivos
- [ ] Configurar NACLs para controle adicional
- [ ] Implementar WAF para aplicações web
- [ ] Estabelecer conexão segura (VPN ou Direct Connect)
- [ ] Realizar auditorias de segurança regularmente
- [ ] Implementar logging e monitoramento

### Custos

- [ ] Estabelecer visibilidade completa de gastos
- [ ] Configurar budgets e alertas
- [ ] Implementar política de tagging consistente
- [ ] Automatizar aplicação de tags
- [ ] Identificar recursos ociosos ou subutilizados
- [ ] Avaliar uso de instâncias Spot para cargas apropriadas
- [ ] Considerar Reserved Instances para cargas previsíveis
- [ ] Configurar autoscaling onde aplicável
- [ ] Implementar lifecycle policies para armazenamento
- [ ] Otimizar transferência de dados
- [ ] Agendar recursos não-produção
- [ ] Utilizar ferramentas nativas de otimização
- [ ] Revisar arquitetura para serverless onde possível
- [ ] Implementar FinOps como prática

### Governança

- [ ] Definir políticas claras de governança
- [ ] Estabelecer convenções de nomenclatura
- [ ] Implementar tagging obrigatório
- [ ] Automatizar aplicação de políticas
- [ ] Configurar relatórios contínuos
- [ ] Estabelecer alertas para não-conformidade
- [ ] Realizar auditorias periódicas
- [ ] Documentar responsabilidades
- [ ] Treinar equipes sobre políticas
- [ ] Implementar processo de revisão

### Well-Architected

- [ ] Avaliar arquitetura contra os 6 pilares
- [ ] Documentar decisões de trade-offs
- [ ] Implementar automação operacional
- [ ] Estabelecer práticas de segurança em todas as camadas
- [ ] Projetar para falhas
- [ ] Otimizar performance continuamente
- [ ] Monitorar custos ativamente
- [ ] Considerar impacto ambiental
- [ ] Realizar reviews periódicos
- [ ] Iterar e melhorar continuamente

---

## 📚 8. GLOSSÁRIO

**AES (Advanced Encryption Standard):** Padrão de criptografia simétrica amplamente utilizado.

**AZ (Availability Zone):** Zona de disponibilidade isolada dentro de uma região cloud.

**CASB (Cloud Access Security Broker):** Ponto de controle de segurança entre usuários e provedores cloud.

**CDN (Content Delivery Network):** Rede de distribuição de conteúdo para melhorar performance.

**CSPM (Cloud Security Posture Management):** Gerenciamento da postura de segurança na nuvem.

**CWPP (Cloud Workload Protection Platform):** Plataforma de proteção de cargas de trabalho na nuvem.

**DEK (Data Encryption Key):** Chave de criptografia de dados.

**DevSecOps:** Integração de segurança em práticas DevOps.

**DORA (Digital Operational Resilience Act):** Regulamentação europeia sobre resiliência operacional digital.

**EBS (Elastic Block Store):** Armazenamento em bloco da AWS.

**ENI (Elastic Network Interface):** Interface de rede elástica da AWS.

**FaaS (Function as a Service):** Computação serverless baseada em funções.

**FIDO2:** Padrão de autenticação forte sem senha.

**FinOps:** Práticas de otimização financeira em cloud.

**GreenOps:** Práticas de otimização ambiental em cloud.

**IaaS (Infrastructure as a Service):** Infraestrutura como serviço.

**IAM (Identity and Access Management):** Gerenciamento de identidade e acesso.

**IaC (Infrastructure as Code):** Infraestrutura como código.

**KEK (Key Encryption Key):** Chave de criptografia de chaves.

**KMS (Key Management Service):** Serviço de gerenciamento de chaves.

**LGPD:** Lei Geral de Proteção de Dados (Brasil).

**MFA (Multi-Factor Authentication):** Autenticação multifator.

**NACL (Network Access Control List):** Lista de controle de acesso de rede.

**NIS2 (Network and Information Security Directive):** Diretiva europeia de segurança de redes.

**NVA (Network Virtual Appliance):** Appliance virtual de rede.

**PaaS (Platform as a Service):** Plataforma como serviço.

**RI (Reserved Instance):** Instância reservada.

**SaaS (Software as a Service):** Software como serviço.

**SIEM (Security Information and Event Management):** Gerenciamento de informações e eventos de segurança.

**SRM (Shared Responsibility Model):** Modelo de responsabilidade compartilhada.

**SSO (Single Sign-On):** Autenticação única.

**TLS/SSL (Transport Layer Security/Secure Sockets Layer):** Protocolos de segurança de transporte.

**VPC (Virtual Private Cloud):** Nuvem privada virtual.

**VPN (Virtual Private Network):** Rede privada virtual.

**WAF (Web Application Firewall):** Firewall de aplicação web.

**Zero Trust (ZTA - Zero Trust Architecture):** Arquitetura de confiança zero.

---

## 🔗 9. RECURSOS ADICIONAIS

### Documentação Oficial

**AWS:**
- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Security Best Practices](https://aws.amazon.com/security/best-practices/)
- [AWS Cost Optimization](https://aws.amazon.com/aws-cost-management/)

**Azure:**
- [Azure Architecture Center](https://docs.microsoft.com/azure/architecture/)
- [Azure Security Documentation](https://docs.microsoft.com/azure/security/)
- [Azure Cost Management](https://azure.microsoft.com/services/cost-management/)

**Google Cloud:**
- [GCP Architecture Framework](https://cloud.google.com/architecture/framework)
- [GCP Security Best Practices](https://cloud.google.com/security/best-practices)
- [GCP Cost Optimization](https://cloud.google.com/cost-management)

### Certificações Relevantes

- AWS Certified Solutions Architect
- AWS Certified Security Specialty
- Azure Solutions Architect Expert
- Azure Security Engineer Associate
- Google Professional Cloud Architect
- Google Professional Cloud Security Engineer
- FinOps Certified Practitioner

### Comunidades

- FinOps Foundation
- Cloud Security Alliance
- CNCF (Cloud Native Computing Foundation)
- DevSecOps Community

---

## 📝 NOTAS FINAIS

Este resumo consolida os conceitos fundamentais da Aula 06 sobre Segurança, Custos e Boas Práticas em Arquitetura Cloud. Os tópicos abordados são interdependentes e devem ser considerados de forma holística ao projetar e operar ambientes cloud.

**Pontos-chave para lembrar:**

1. **Segurança é responsabilidade compartilhada** - compreenda claramente o que é de sua responsabilidade
2. **Otimização de custos é processo contínuo** - não é ação pontual
3. **Governança eficaz requer automação** - erro humano é um dos maiores riscos
4. **Trade-offs são inevitáveis** - documente decisões conscientemente
5. **Aprendizado contínuo é essencial** - o cenário cloud evolui rapidamente

**Próximos passos sugeridos:**

- Revisar as videoaulas para demonstrações práticas
- Praticar configurações em ambiente de laboratório
- Explorar ferramentas nativas de cada provedor
- Participar de comunidades e fóruns
- Buscar certificações relevantes
- Manter-se atualizado com tendências de mercado

---

**Gerado para:** Jamal - DevOps & Arquitetura Cloud (360h)  
**Programa:** FIAP + Alura POSTECH  
**Fase:** 1 - Arquitetura Cloud  
**Data:** Dezembro 2025