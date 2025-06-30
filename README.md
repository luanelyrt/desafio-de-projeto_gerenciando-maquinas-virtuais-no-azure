# Entrega do Desafio:

<br>

## 1. Configurando Máquinas Virtuais no Azure  
Resumo abrangente dos principais conceitos e práticas para criar, gerenciar e otimizar VMs no Microsoft Azure.

### Introdução  
Apresentação dos fundamentos de IaaS no Azure e vantagens de usar VMs na nuvem: elasticidade, pagamento sob demanda, variedade de OS e integração nativa com outros serviços.

### Pré-requisitos  
- Conta Azure ativa (assinatura)  
- Permissões para criar recursos (RBAC: Contributor ou Owner)  
- Conhecimento básico de redes, sistemas operacionais e linha de comando  

### Conceitos Fundamentais  
- **Regiões e Zonas de Disponibilidade**  
  • Localizações físicas (ex.: East US, Brazil South)  
  • Zonas: falhas independentes para alta disponibilidade  
- **Tipos de VM e Séries**  
  • Séries B (burstable), Dsv5 (propósito geral), Esv4 (otimizada para memória), Nv (GPU), etc.  
  • Vantagens e casos de uso para cada série  

### Provisionamento de VMs  
1. **Portal Azure**  
   - Assistente passo a passo: escolha de imagem, tamanho (vCPU/RAM), recurso de rede, conta de disco  
2. **Azure CLI** (`az vm create`)  
3. **PowerShell** (`New-AzVM`)  
4. **ARM Templates**  
   - Infraestrutura como código (IaC) para deploy consistente  

### Parâmetros chave:  
- Imagem (Marketplace ou customizada)  
- Tamanho da VM  
- Chave SSH ou senha  
- Grupo de recursos  

### Rede e Segurança  
- **Rede Virtual (VNet)**  
  • Segmentação de IPs, sub-redes, peering  
- **Grupo de Segurança de Rede (NSG)**  
  • Regras de entrada e saída (permitir RDP/SSH, bloquear portas não usadas)  
- **Azure Firewall e WAF**  
  • Proteção avançada a nível de aplicação  
- **Endereços IP Públicos/DNS**  
  • Atribuição dinâmica ou estática  

### Armazenamento  
- **Discos Gerenciados**  
  • SSD Premium, Standard SSD, Standard HDD  
- **Tipos de Disco**  
  • OS Disk, Data Disk, Ultra Disk  
- **Snapshot e Imagem**  
  • Criar backup ponto-no-tempo; replicar VMs idênticas  

### Gerenciamento e Monitoramento  
- **Azure Monitor**  
  • Métricas (CPU, memória, IOPS), Logs de diagnóstico  
- **Azure Advisor**  
  • Recomendações de performance, segurança e custo  
- **Alertas e Action Groups**  
  • Notificações via e-mail, SMS, webhook  
- **Update Management**  
  • Patch automático de SO e extensões  

### Alta Disponibilidade e Escalabilidade  
- **Conjuntos de Disponibilidade (Availability Sets)**  
  • Domínios de falha e atualização para tolerância a quedas de hardware  
- **Escala Automática (VMSS)**  
  • Scale-out/in baseado em métricas (CPU, fila de mensagens, schedule)  
- **Zones Redundancy**  
  • Deploy entre Zonas de Disponibilidade  

### Extensões e Customizações  
- **Custom Script Extension**  
  • Execução de scripts PowerShell/Bash pós-provisionamento  
- **Desired State Configuration (DSC)**  
  • Garantir configuração idempotente do SO e apps  
- **Azure Automation**  
  • Runbooks para tarefas recorrentes  

### Backup e Recuperação  
- **Azure Backup**  
  • Política de retenção, backup incremental, instant restore  
- **Azure Site Recovery (ASR)**  
  • Replicação entre regiões para DR (Disaster Recovery)  

### Otimização de Custos  
- **Reserved Instances (1 ou 3 anos)**  
- **Spot VMs**  
- **Right-Sizing**  
  • Ajustar tamanhos com base em uso real  
- **Azure Cost Management + Billing**  
  • Orçamentos, alertas de gasto, relatórios de utilização  

### Boas Práticas  
- Use tags para organização e chargeback  
- Segregue ambientes (dev/test/prod) em diferentes grupos de recursos ou assinaturas  
- Implemente políticas do Azure Policy para governança  
- Habilite logs de auditoria e diagnóstico  
- Realize testes de failover regulares  

### Referências e Leituras Complementares  
- Documentação oficial Azure VMs: https://docs.microsoft.com/azure/virtual-machines  
- Azure CLI Reference: https://docs.microsoft.com/cli/azure/vm  
- Guia de práticas recomendadas: https://docs.microsoft.com/azure/architecture/best-practices  

<br>

## 2. Configurando a Disponibilidade da Máquina Virtual no Azure  
Resumo conciso e abrangente dos recursos e práticas para garantir alta disponibilidade e recuperação de falhas de VMs no Microsoft Azure. 

### Introdução  
Compreender os SLAs do Azure e adotar padrões de alta disponibilidade (HA) e recuperação de desastre (DR) são fundamentais para manter aplicações críticas online mesmo diante de falhas de hardware, data center ou região.

### Pré-requisitos  
- Assinatura Azure com quotas suficientes.  
- Permissões de Owner/Contributor para criar recursos de rede, VMs, Load Balancer e ASR.  
- Familiaridade com console Azure Portal, CLI/PowerShell e noções de redes e scripts.

### Conceitos-Chave de Disponibilidade  
- **Fault Domain (FD):** agrupamento físico de hardware (rack) isolado de falhas.  
- **Update Domain (UD):** grupo lógico para atualização sequencial de SO sem tirar todo o serviço offline.  
- **SLA do Azure:** percentual mínimo de uptime garantido (ex.: 99,9% para Availability Set com duas VMs).  

### Conjuntos de Disponibilidade (Availability Sets)  
- Distribuem VMs entre múltiplos FDs e UDs.  
- Minimiza impacto de falha de hardware ou patch de manutenção.  
- Requisitos: pelo menos duas VMs idênticas configuradas no mesmo conjunto.  
- SLA: 99,95% de disponibilidade quando configurado corretamente.

### Zonas de Disponibilidade (Availability Zones)  
- Três áreas físicas independentes em cada região (ex.: Brazil South).  
- Suportam Tier-1 de robustez: deploy de VMs em Z1, Z2 e Z3.  
- Failover quase instantâneo em caso de deslocamento de energia ou falha de rede regional.  
- Use Load Balancer ou Traffic Manager para rotear tráfego entre zonas/regiões.

### Escalonamento Automático e Escala de Conjunto de VMs (VMSS)  
- **Máquinas Virtuais em Conjunto (VM Scale Sets):** replicação automática de instâncias idênticas.  
- **Auto-scale:** regras baseadas em métricas (CPU, memória, latência) ou schedule.  
- Garante elasticidade e redundância horizontal.  
- Integrável a Load Balancer interno/externo.

### Balanceamento de Carga e Topologias Ativas-Ativas  
- **Azure Load Balancer (Layer 4):** distribui tráfego TCP/UDP entre VMs em mesma região.  
- **Application Gateway (Layer 7):** roteamento baseado em URL, WAF e SSL offload.  
- **Traffic Manager:** DNS-level routing para failover entre regiões (latency-based, priority-based).  
- **Front Door:** CDN + roteamento global com aceleração de aplicações.

### Replicação e Recuperação de Desastre  
- **Azure Site Recovery (ASR):** 
  • Replicação contínua das VMs para outra região ou zona.  
  • Plans de failover e failback orquestrados.  
- **Snapshot e Backup de discos:** Snapshots periódicos de OS/Data disks via Azure Backup.  
- **Teste de DR:** failover de teste isolado sem impactar o ambiente de produção.

### Monitoramento, Teste e Failover  
- **Azure Monitor & Log Analytics:** alertas para quedas de instância, latência ou utilização anômala.  
- **Action Groups & Runbooks:** automatize recovery tasks (ex.: reprovisionar VM, executar script).  
- **Failover Drills:** simular falhas de FD, UD ou zona inteira para validar RTO/RPO.

### Otimização de Custos  
- Use **VM de spot** para cargas não críticas em DR.  
- Prefira **Reserved Instances** para VMs primárias de longa duração.  
- Avalie diferenças de preço entre Availability Sets e Zonas de Disponibilidade.  
- Elimine recursos órfãos (endereços IP públicos, discos não anexados).

### Boas Práticas  
- Planeje arquitetura ativa-ativa sempre que possível.  
- Separe redes e políticas de segurança por camadas (DMZ, aplicação, dados).  
- Aplique tags em recursos de DR para gestão e cobrança.  
- Documente e version control seus runbooks e templates (ARM, Bicep, Terraform).  
- Revise e teste planos de failover/recuperação pelo menos semestralmente.

### Referências e Leituras Complementares  
- Alta Disponibilidade de VM no Azure: https://docs.microsoft.com/azure/virtual-machines/high-availability  
- Azure Site Recovery Overview: https://docs.microsoft.com/azure/site-recovery/overview  
- Patterns for Cloud Resilience: https://docs.microsoft.com/azure/architecture/patterns  

<br>

## 3. Gerenciando Máquinas Virtuais no Azure  
Guia resumido e abrangente para administrar o ciclo de vida, a operação diária e a governança de VMs em Azure.

### Introdução  
Administrar VMs no Azure vai além do provisionamento: envolve manter disponibilidade, performance, segurança, conformidade e eficiência de custos ao longo de todo o ciclo de vida.

### Pré-requisitos  
- Conta e assinatura Azure com permissões (Owner/Contributor).  
- Conhecimento em CLI/PowerShell e ARM/Bicep.  
- Noções de redes (VNet/NSG), armazenamento de discos e monitoramento.

### Ciclo de Vida da VM  
- **Start/Stop/Restart/Deallocate**  
  • `az vm start|stop|restart|deallocate` ou PowerShell.  
- **Captura de estado**  
  • Status (ProvisioningState, PowerState) em Azure Portal, CLI ou API REST.  
- **Redimensionamento**  
  • Alterar tamanho via `az vm resize` ou Portal, considerando quotas e dependências de disco.  
- **Upgrade de SKU**  
  • Migrate OS disk para tipo Premium/Ultra sem reprovisionar.

### Atualização e Patching  
- **Update Management (Automation)**  
  • Grupos de atualização, agendamento, relatórios de compliance.  
- **Extensão de VM**  
  • `Microsoft.Compute/VMAccessAgent` para correções emergenciais.  
- **Windows Update/apt-get**  
  • Integre com Automation Hybrid Workers para VMs on-premises ou em outras clouds.

### Monitoramento e Diagnóstico  
- **Azure Monitor Metrics & Logs**  
  • CPU, memória, disco, rede; Log Analytics para consultas Kusto.  
- **Insights para VM**  
  • Dependências de serviço, mapeamento de processos, análise de performance.  
- **Alertas e Action Groups**  
  • Condições (ex.: alta latência, erro de guest agent) com notificações por e-mail/SMS/webhook.

### Automação e Scripting  
- **Azure CLI & PowerShell**  
  • Scripts idempotentes para tasks repetitivas (start/stop, snapshots).  
- **ARM Templates / Bicep**  
  • Declaração de configurações VM + extensões + rede + disco.  
- **Azure DevOps / GitHub Actions**  
  • Pipelines para CI/CD de infraestrutura (IaC).  
- **Runbooks no Azure Automation**  
  • Tarefas agendadas: limpeza de discos órfãos, resize, failover.

### Imagens, Snapshots e Galeria  
- **Snapshot de discos**  
  • Backup ponto-no-tempo para OS e Data Disks.  
- **Captura de Imagem**  
  • `az vm capture` cria imagem generalizada (Windows Sysprep/Linux waagent).  
- **Shared Image Gallery**  
  • Gerencie versões, regiões, e replicação de imagens para escala e consistência.

### Escalabilidade e Scale Sets  
- **VM Scale Sets (VMSS)**  
  • Instâncias idênticas, autoscale por métrica ou agendamento.  
- **Mode Flexível vs Uniforme**  
  • Flexível para cargas heterogêneas; Uniforme para pools homogêneos.  
- **Custom Scripts & Extensões**  
  • Provisionamento automatizado de software em cada instância.

### Rede e Balanceamento de Carga  
- **NSG e ASG**  
  • Filtragem de tráfego por grupo de IPs/VMs.  
- **Load Balancer**  
  • Interno/externo, health probes, NAT rules.  
- **Application Gateway & Traffic Manager**  
  • Roteamento L7, SSL offload, WAF e failover global.

### Backup e Recuperação  
- **Azure Backup**  
  • Política de retenção, backups incrementais, instant restore.  
- **Azure Site Recovery**  
  • Replicação contínua, planos orquestrados de failover/failback.  
- **Testes de DR**  
  • Execuções isoladas de failover sem impacto à produção.

### Governança e Políticas  
- **Azure Policy / Initiative**  
  • Enforce naming, SKU máximo, tipos de disco, localização.  
- **Blueprints**  
  • Conjuntos pré-configurados de políticas, RBAC, recursos.  
- **Tags**  
  • Projeto, ambiente, dono, custo-center para relatórios e chargeback.

### Segurança e Compliance  
- **Azure Security Center**  
  • Recomendação para VMs (JIT, antivírus, atualizações).  
- **Vulnerability Assessment**  
  • Integração com Qualys/Microsoft Defender para varreduras.  
- **Just-In-Time VM Access**  
  • Abrir portas SSH/RDP somente quando necessário, com auditoria.

### Otimização de Custos  
- **Start/Stop Schedule** 
  • Desligue VMs em horários ociosos via Automation.
- **Right-Sizing**
  • Ajuste tipos/séries baseado no uso real.
- **Reserved Instances e Savings Plans**
  • Descontos em comprometimento de 1–3 anos.
- **Spot VMs**
  • Para cargas tolerantes a interrupção (batch jobs, testes).

### Boas Práticas
- Automatize tudo (IaC + pipelines).
- Monitore saúde e performance continuamente.
- Aplique controle de acesso granular (RBAC mínimo).
- Documente runbooks e procedimentos de recovery.
- Reserve ambientes de teste para validar alterações antes da produção.

### Referências e Leituras Complementares  
- Gerenciamento de VMs no Azure:  
  https://docs.microsoft.com/azure/virtual-machines/management
- Azure Update Management:
  https://docs.microsoft.com/azure/automation/update-management
- Azure Monitor para VMs:
  https://docs.microsoft.com/azure/azure-monitor/vm-insights
- Automation Runbooks:
  https://docs.microsoft.com/azure/automation/automation-runbook-types
