# computa-o-_e_rede_lab

# Máquinas Virtuais no Azure — Resumo da Aula (AZ-900)

## O que é uma Máquina Virtual (VM)

Uma Máquina Virtual no Azure é um recurso de **IaaS (Infrastructure as a Service)**, ou seja, a Microsoft fornece o hardware virtualizado (processamento, memória, disco e rede) e o cliente é responsável por instalar e gerenciar o sistema operacional, os aplicativos, as atualizações e os dados. Esse é um ponto muito cobrado na prova AZ-900: no modelo de responsabilidade compartilhada, quanto mais "as a Service" (de IaaS para PaaS e depois para SaaS), menos responsabilidade técnica fica com o cliente e mais fica com a Microsoft. Em uma VM (IaaS), o cliente ainda cuida do sistema operacional e de tudo que roda dentro dele.

## As três opções ao clicar em "Criar" em Máquinas Virtuais

Quando se acessa o serviço "Máquinas Virtuais" no portal e clica em "Criar", o Azure apresenta três caminhos diferentes:

**Máquina virtual**: é a opção voltada para cargas de trabalho com pouco tráfego, para testes, ou para quando se quer controlar e personalizar profundamente o sistema operacional e os aplicativos. É a criação de uma única VM isolada. O próprio portal já avisa que, se a carga de trabalho crescer, essa VM pode futuramente ser anexada a um Conjunto de Dimensionamento de Máquinas Virtuais.

**Conjunto de Dimensionamento de Máquinas Virtuais (VMSS — Virtual Machine Scale Set)**: permite criar um grupo de VMs idênticas que escalam automaticamente, com balanceamento de carga, otimização de desempenho e gerenciamento em lote. Suporta de 1 a 1.000 VMs sem custo adicional pelo próprio recurso de escala (paga-se apenas pelas VMs em si), e permite combinar diferentes tamanhos de VM, zonas de disponibilidade, regiões, domínios de falha e até VMs Spot com desconto. Esse é o recurso indicado quando a aplicação precisa crescer ou diminuir de forma automática conforme a demanda (auto scaling).

**Soluções híbridas, pré-configuradas e de alto volume**: reúne kits de inicialização já prontos para Linux e Windows, além de soluções de infraestrutura híbrida usando o Azure Arc (que estende o gerenciamento do Azure para servidores fora da nuvem, inclusive on-premises ou em outras nuvens). É voltada para cenários corporativos maiores, de implantação em volume ou de integração entre ambiente local e nuvem.

Ponto de atenção para a prova: a diferença entre **scale up** (aumentar a capacidade de uma única VM, trocando para um tamanho maior) e **scale out** (adicionar mais VMs, o que é exatamente o papel do VMSS) costuma aparecer em questões.

## Etapas da criação de uma Máquina Virtual

O assistente de criação de VM no portal é dividido em guias sequenciais. Cada uma delas configura um aspecto diferente do recurso.

### 1. Básico

É onde ficam as definições fundamentais da VM. Primeiro, os detalhes do projeto: a **assinatura** (subscription, que é a unidade de cobrança e controle de acesso) e o **grupo de recursos** (resource group, um contêiner lógico usado para organizar e gerenciar recursos relacionados como se fossem uma pasta — recursos de um grupo geralmente são criados, atualizados e excluídos juntos). Depois vêm os detalhes da instância: nome da VM, região (localização física dos datacenters onde a VM será hospedada — importante para latência, custo e conformidade legal) e a opção de zona de disponibilidade. Nessa guia também se escolhe a imagem do sistema operacional (Windows Server, várias distribuições Linux, ou uma imagem personalizada), o tamanho da VM (que define quantidade de vCPUs, memória RAM e capacidade de disco/rede), a conta de administrador (usuário e senha ou chave SSH) e as regras de porta de entrada (inbound), além do tipo de licenciamento.

Ponto de prova: a **região** escolhida impacta diretamente em preço (cada região tem sua própria tabela de custos) e em requisitos de residência de dados; e as **zonas de disponibilidade** são datacenters fisicamente separados dentro de uma mesma região, usadas para proteger contra falha de um datacenter inteiro.

### 2. Discos

Define o armazenamento da VM. É possível escolher o tipo do disco do sistema operacional entre HDD Standard (mais barato, menor desempenho), SSD Standard (custo-benefício intermediário), SSD Premium (alto desempenho, indicado para cargas de produção) e Disco Ultra (latência extremamente baixa, para bancos de dados e cargas exigentes). Também é possível configurar discos de dados adicionais, o tipo de controlador de disco e a criptografia dos discos.

Ponto de prova: a diferença entre os tipos de disco (HDD vs. SSD Standard vs. SSD Premium vs. Ultra Disk) é um tema clássico, assim como o fato de que discos gerenciados (managed disks) são a forma recomendada pela Microsoft de administrar o armazenamento das VMs, pois o Azure cuida da criação e do gerenciamento das contas de armazenamento por trás dos discos.

### 3. Rede

Configura a conectividade da VM: a rede virtual (VNet) e a sub-rede em que ela será colocada, se terá ou não um IP público, o grupo de segurança de rede (NSG — Network Security Group, que funciona como um firewall controlando tráfego de entrada e saída) e a opção de colocar a VM atrás de um balanceador de carga.

Ponto de prova: a rede virtual é o que permite que os recursos do Azure se comuniquem entre si e com a internet de forma segura, e o NSG é o principal mecanismo de controle de tráfego citado na prova.

### 4. Gerenciamento

Reúne opções relacionadas à administração contínua da VM, como identidade gerenciada (managed identity, que permite que a VM se autentique em outros serviços do Azure sem precisar guardar credenciais no código), login via Microsoft Entra ID, desligamento automático (auto-shutdown, útil para economizar custos em ambientes de teste) e backup.

### 5. Monitoramento

Ativa o diagnóstico de inicialização (boot diagnostics), que ajuda a identificar problemas caso a VM não inicialize corretamente, além de diagnósticos do sistema convidado e a configuração de alertas de monitoramento, normalmente integrados ao Azure Monitor.

### 6. Avançado

Permite adicionar extensões (scripts e ferramentas que rodam automaticamente após o provisionamento), configurar cloud-init, instalar aplicativos de VM, definir grupos de posicionamento por proximidade (para reduzir latência entre recursos) e passar dados personalizados (user data) para a inicialização.

### 7. Marcas (Tags)

Permite adicionar pares de nome e valor (por exemplo, "Ambiente: Produção" ou "Departamento: Financeiro") aos recursos. As tags não afetam o funcionamento da VM, mas são fundamentais para organização, controle de custos e geração de relatórios financeiros.

Ponto de prova: tags são um dos temas mais cobrados quando o assunto é gerenciamento de custos e organização de recursos no Azure.

### 8. Revisar + criar

Última etapa, onde o Azure valida automaticamente as configurações escolhidas, mostra um resumo de tudo o que foi definido nas guias anteriores e apresenta uma estimativa de custo mensal antes da implantação final. Só depois dessa validação o botão "Criar" efetivamente provisiona a VM.

## Outros pontos comuns na prova AZ-900 sobre VMs

Um tema recorrente é a comparação entre os **modelos de preço**: Pay-as-you-go (paga-se pelo uso, sem compromisso), Reserved Instances (desconto em troca de compromisso de uso por 1 ou 3 anos) e VMs Spot (grande desconto, mas a Microsoft pode desalocar a VM quando precisar da capacidade de volta — por isso são indicadas apenas para cargas tolerantes a interrupção, como processamento em lote).

Outro ponto é a diferença entre **conjunto de disponibilidade (availability set)** e **zona de disponibilidade (availability zone)**: o primeiro protege contra falhas de hardware dentro do mesmo datacenter, distribuindo as VMs em diferentes domínios de falha e de atualização; o segundo protege contra a falha de um datacenter inteiro, distribuindo as VMs entre datacenters fisicamente separados dentro da mesma região.

Também costuma ser cobrada a lógica de que o **grupo de recursos** não tem custo em si — ele é apenas uma forma de organização — mas os recursos dentro dele é que geram cobrança, e que excluir um grupo de recursos exclui todos os recursos contidos nele.

Por fim, é comum a prova pedir para reconhecer o Azure Site Recovery (para recuperação de desastres) e o Azure Backup (para cópias de segurança) como os serviços responsáveis pela continuidade de negócios e proteção de dados de VMs, temas ligados à etapa de Gerenciamento vista no assistente de criação.
