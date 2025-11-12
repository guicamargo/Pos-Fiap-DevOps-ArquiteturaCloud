# Cultura DevOps - Aula 2: Práticas Comuns e Ferramentas

## 📋 Resumo da Aula

Esta aula apresentou os fundamentos técnicos e culturais que sustentam o movimento DevOps, explorando os principais pilares que tornam possível a entrega moderna de software.

---

## 🔑 Conceitos Principais

### 1. **Infraestrutura como Código (IaC)**
- Transformação de servidores, redes e ambientes em arquivos versionáveis
- Ambientes reproduzíveis, auditáveis e imunes a desvios
- Tratamento da infraestrutura como código de aplicação
- **Ferramentas principais:**
  - **Terraform** - multi-cloud com linguagem HCL
  - **AWS CloudFormation, Azure Resource Manager, Google Deployment Manager** - soluções nativas
  - **Pulumi** - IaC em linguagens convencionais (TypeScript, Python, Go)
  - **Ansible, Chef, Puppet** - gerenciamento de configurações

---

### 2. **Integração Contínua (CI) e Entrega Contínua (CD)**

#### **Integração Contínua:**
- Cada commit aciona verificações automatizadas
- Compilação e testes unitários imediatos
- Feedback rápido ao desenvolvedor

#### **Entrega Contínua:**
- Pipeline automatizado de testes, builds e validações
- Testes de integração, simulações de carga e verificações de segurança
- Processo orquestrado por arquivos de configuração (geralmente YAML)

#### **Estratégias de Deploy:**
- **Blue-Green**: dois ambientes idênticos, troca controlada de tráfego
- **Canary Release**: lançamento gradual para percentual pequeno de usuários

#### **Ferramentas principais:**
- **Jenkins** - extensível via plugins
- **GitHub Actions e GitLab CI/CD** - integração direta com repositórios
- **Argo CD** - GitOps para Kubernetes

---

### 3. **Containerização e Orquestração**

#### **Evolução da Virtualização:**
- **VMs**: isolamento completo mas com sobrecarga
- **Containers**: compartilham kernel do host, reduzem recursos e tempo de inicialização

#### **Benefícios:**
- Microsserviços isolados em containers imutáveis
- Rastreabilidade e confiabilidade
- Mesmo artefato da homologação vai para produção

#### **Orquestração:**
- **Kubernetes**: automatiza escalonamento, balanceamento e recuperação
- Configurações declarativas mantêm estado desejado do cluster

#### **Segurança:**
- **AppArmor, SELinux, Falco** - políticas rígidas de isolamento
- Sandboxing para evitar processos maliciosos

#### **Virtualização Tradicional:**
- **VMware vSphere, Proxmox, Hyper-V** - ainda relevantes em data centers on-premises
- **Arquiteturas híbridas**: combinam VMs e containers

---

### 4. **Monitoramento e Feedback Contínuo**

#### **Os 3 Pilares da Observabilidade:**
1. **Logs** - centralização de eventos e erros
2. **Métricas** - CPU, memória, tempo de resposta, taxa de sucesso
3. **Traces** - rastreamento distribuído entre microsserviços

#### **SLOs e SLIs:**
- **Service Level Objectives (SLOs)** - acordos de nível de serviço
- **Service Level Indicators (SLIs)** - indicadores de medição
- Alertas automatizados quando limites são ultrapassados

#### **Ferramentas principais:**
- **Prometheus e Grafana** - métricas e dashboards
- **Elasticsearch e Kibana** - logs centralizados
- **Jaeger** - tracing distribuído
- **Datadog e New Relic** - soluções comerciais integradas

---

## 💻 Ferramentas e Tecnologias Citadas

| Categoria | Ferramentas |
|-----------|------------|
| **IaC** | Terraform, CloudFormation, Pulumi, Ansible, Chef, Puppet |
| **CI/CD** | Jenkins, GitHub Actions, GitLab CI/CD, Argo CD, Spinnaker |
| **Containers** | Docker, Kubernetes |
| **Monitoramento** | Prometheus, Grafana, Elasticsearch, Kibana, Jaeger, Datadog, New Relic |
| **Virtualização** | VMware vSphere, Proxmox, Hyper-V |
| **Segurança** | AppArmor, SELinux, Falco |

---

## 🎯 Case de Mercado: Netflix

**Como a Netflix faz milhares de alterações diárias globalmente sem colapso?**

- Infraestrutura definida como código (IaC)
- Pipelines CI/CD robustos e automatizados (Spinnaker)
- Aplicações em containers padronizados
- Consistência do ambiente dev até produção
- **Resultado:** eficiência e resiliência em escala global

---

## 🔮 Tendências Futuras

### **GitOps**
- Repositório Git como única fonte de verdade
- Ferramentas: Argo CD, Flux
- Estado da produção reflete o código

### **Policy as Code**
- **Open Policy Agent (OPA)**
- Infraestrutura criada já em conformidade com regras de segurança e custos
- Automação inteligente e declarativa

---

## ✅ Principais Aprendizados

1. **IaC** permite criar ambientes versionáveis e reproduzíveis
2. **Pipelines CI/CD** automatizam testes, builds e deploys
3. **Estratégias progressivas** (blue-green, canary) minimizam riscos
4. **Containers e Kubernetes** garantem escalabilidade e resiliência
5. **Feedback contínuo** fecha o ciclo DevOps com métricas, logs e traces
6. **Cultura colaborativa** entre dev e ops é essencial
7. **Automação** é a chave para velocidade e confiabilidade

---

## 📝 Palavras-chave

DevOps, Integração Contínua (CI), Entrega Contínua (CD), Automação de Deploy, Pipeline de Deploy, Monitoramento Contínuo, Feedback Contínuo, Cultura Colaborativa, Infraestrutura como Código (IaC), Provisionamento Automatizado, Gestão de Configuração, Entregas Ágeis, Colaboração entre Times, Redução de Riscos, Resiliência Organizacional, Entregas Frequentes, Alta Disponibilidade, Ferramentas DevOps, GitOps, Observabilidade, CI/CD Pipeline, Deploy Automatizado, Comunicação entre Equipes, Aprendizado Contínuo, Segurança Integrada (DevSecOps), Versionamento de Código, Qualidade de Software, Métricas de Performance, Escalabilidade, Orquestração de Containers.

---

## 📚 Referências

- GITHUB. About continuous integration with GitHub Actions. 2024.
- IBM. O que é DevOps?. 2024.
- PIWOSZ, P. 73 Most Useful DevOps Tools to Use in 2025. 2025.

---