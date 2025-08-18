# Principais Recursos de Segurança AWS

### **AWS Identity and Access Management (IAM)**
O AWS IAM permite o gerenciamento de identidades (usuários, grupos, funções) e o controle de quem pode acessar quais recursos na AWS.  

**Como isso pode ser feito?**
- Com a criação de políticas de acesso granulares baseada em permissões mínimas (princípio do menor privilégio), revisadas e auditadas regularmente.
- O uso de roles de IAM é recomendado para serviços e aplicações, pois permite delegação segura sem expor credenciais. Para usuários humanos, recomenda-se aplicar políticas gerenciadas e ativar MFA, especialmente para contas com privilégios elevados.
- Autenticação multifator (MFA) para usuários com privilégios elevados.

## **AWS Key Management Service (KMS)**
O KMS é utilizado como um gerenciador de chaves criptográficas usadas para proteger dados. Os dados mantém-se criptografados em transito e em repouso. Com isso pode haver integração com serviços como S3, EBS, RDS para criptografia automática e um controle de acesso às chaves com políticas IAM e logs de uso via CloudTrail. O KMS permite o uso de Customer Managed Keys (CMKs) para controle granular, incluindo rotação automática, permissões específicas e auditoria via CloudTrail. Também é possível usar chaves gerenciadas pela AWS para simplicidade em serviços como S3 e RDS  

## **AWS Security Groups**
Os Security Groups funcionam como firewalls virtuais para controle de tráfego de rede para instâncias EC2. Com ele podem ser definidas regras de entrada e saída baseadas em IP, porta e protocolo e criar um isolamento de ambientes como produção, desenvolvimento e testes. Os Security Groups por padrão são Statefull, isto é, se uma solicitação de saída for permitida, a resposta de entrada correspondente também será permitida automaticamente. Isso simplifica a configuração, pois não há necessidade de criar regras duplicadas para permitir o retorno de pacotes. Mas uma boa prática seria criar regras para este tipo de fluxo e criar grupos separados para diferentes camadas (web, app, banco de dados) e usar apenas os protocolos necessários permitindo SSH apenas de IPs especificos.  

## **AWS CloudTrail**
O CloudTrail é um serviço que registra todas as chamadas de API feitas na conta AWS.   
Com ele é possível fazer auditoria de ações de usuários e serviços e também detectar atividades suspeitas ou não autorizadas. Com ele também é possivel configurar alertas para eventos críticos como criação de usuários, alteração de políticas, Armazenar logs em buckets S3 com criptografia via KMS e analisados com ferramentas como Athena ou CloudWatch Logs para detectar padrões suspeitos e gerar alertas.

## **AWS GuardDuty**
O GuardDuty é um serviço gerenciado da AWS que oferece detecção inteligente de ameaças baseado em machine learning, IA e inteligência de ameaças para proteger contas, workloads e dados na nuvem, identifica comportamentos anômalos e correlaciona eventos para detectar padrões de ataque usando fontes da AWS e de terceiros para identificar IPs maliciosos, domínios suspeitos e atividades de invasores conhecidos. Oferecendo um monitoramento contínuo ele analisa logs de atividades com VPC Flow Logs, CloudTrail e DNS logs.  
Ativando o GuardDuty em todas as regiões é possivel configurar notificações automáticas via SNS ou integração com sistemas de respostas a incidentes.

## Recomendações de Melhores Práticas de Segurança na Migração para AWS
Recomenda-se utilizar a criptografia de dados em transito e repouso usando TLS e KMS e criptografia nativa dos serviços(ex: S3, RDS, EBS) e gerenciar chaves com KMS aplicando políticas de acesso restritivas.  
Ativar o CloudTrail e GuardDuty em todas as regiões desde o início para monitoramento e segurança. Outras opções complementares seria usar AWS Config para rastrear alterações de configuração e integrar logs com ferramentas de SIEM para analise centralizada.  
Implementar IAM para controle de acesso com políticas baseadas em funções, usar AWS Orgaizations para segmentar contas por ambiente ou equipe, aplicar MFA e revisar acessos regularmante.