# Guia-para-deletar-recursos-AWS
Deletar um stack de aplicação tradicional de 3 camadas + mensageria + monitoramento

## Por que este guia?  
Quando começamos a usar a AWS, é comum aprender a **criar recursos**, mas poucos sabem a ordem correta de **deletar**.  
Se apagar na ordem errada, alguns recursos ficam **órfãos** ou geram **custos desnecessários**.  

Este repositório mostra um **exemplo de stack** e a **ordem correta para remover** cada recurso.  

---

## Exemplo de Stack  
Criação da aplicação

* Essa aplicação foi construída sobre uma arquitetura clássica na AWS, cobrindo rede, computação, armazenamento, monitoramento e mensageria:

Rede (VPC, Subnets, NAT Gateway, Security Groups): Criei uma VPC isolada para hospedar a aplicação, com subnets públicas (para o Load Balancer) e privadas (para banco de dados e instâncias). O NAT Gateway permite que recursos privados acessem a internet sem ficarem expostos.

Balanceamento e Auto Scaling (ALB + ASG): Configurei um Application Load Balancer para distribuir requisições entre as instâncias. As instâncias EC2 foram agrupadas em um Auto Scaling Group, garantindo alta disponibilidade e escalabilidade automática.

Banco de dados (RDS): Utilizei o Amazon RDS para persistência de dados, rodando em subnets privadas para segurança.

Armazenamento (S3): Buckets do Amazon S3 foram usados tanto para armazenar arquivos estáticos quanto como ponto de integração com outros serviços.

Serverless e Mensageria (Lambda + SNS): Funções Lambda processam eventos disparados a partir do S3 ou de aplicações internas. O SNS foi usado para envio de notificações ou integração com outros consumidores.

Monitoramento (CloudWatch): O CloudWatch coleta logs e métricas de toda a stack, permitindo configurar alarmes para eventos críticos."

### Uso da aplicação

* Esse tipo de aplicação é usada como base de workloads web ou APIs em nuvem.
Ela é segura, escalável e resiliente, típica de sistemas corporativos que precisam atender múltiplos usuários, com dados persistidos, alta disponibilidade e monitoramento centralizado.
Serve de exemplo para arquitetura de 3 camadas na AWS:

Frontend → acessado via Load Balancer,

Backend → rodando em EC2/Lambda,

Banco de dados → RDS."

Deleção da aplicação

* Para deletar, é importante seguir a ordem correta para evitar erros de dependência. Primeiro removo os recursos que dependem de outros, depois os de infraestrutura:

1. **Aplicações/Triggers**
   - Lambda Functions  
   - SNS Topics + Subscriptions  

2. **Armazenamento de Dados**
   - Esvaziar e deletar Buckets S3  
   - Encerrar instâncias do RDS  

3. **Monitoramento**
   - CloudWatch Alarms  
   - CloudWatch Log Groups  

4. **Compute/Balanceamento**
   - Auto Scaling Group  
   - Target Groups e Load Balancer  
   - Instâncias EC2  

5. **Rede**
   - Elastic IPs + NAT Gateway  
   - Route Tables personalizadas  
   - Security Groups  
   - Subnets  
   - Internet Gateway (detach da VPC antes)  
   - VPC  
 
## Objetivo do Repositório

* Ser uma fonte de pesquisa rápida para iniciantes

* Ensinar boas práticas de ciclo de vida (criação → uso → deleção)

* Evitar custos inesperados por recursos esquecidos
