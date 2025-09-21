# Desafio1_Dio
## 📑 Relatório de Estudo

### 1. Introdução
Este projeto demonstra uma arquitetura simples utilizando **EC2, EBS, Lambda e S3**, com envio automático de arquivos antigos para o S3. Além disso, inclui aprendizados adquiridos em aula sobre gerenciamento de recursos AWS.

### 2. EC2 (Elastic Compute Cloud)
- Serviço de máquina virtual escalável.  
- Utilizado como servidor principal do sistema.  
- Permite anexar múltiplos volumes EBS.  

**Gestão de Perfis e Acessos**
- É possível criar **usuários administradores** e **visualizadores**.  
- Os usuários podem ser agrupados em **grupos de permissões**, facilitando a gestão de acessos.  

**Precificação**
- O custo do EC2 depende da configuração escolhida (tipo de instância, CPU, memória, disco).  
- É possível criar **gatilhos automáticos** para **ligar/desligar a instância** conforme a necessidade, reduzindo custos.  

### 3. EBS (Elastic Block Store)
- Volumes de armazenamento em bloco conectados ao EC2.  
- No projeto foram criados 2 volumes:
  - **Volume 1**: dados ativos.  
  - **Volume 2**: arquivos inativos (>90 dias), que depois são movidos para o S3.  

### 4. Lambda
- Função que verifica os arquivos antigos e move para o S3.  
- Pode ser disparada via **CloudWatch Events** para automatizar a rotina.  

### 5. S3 (Amazon Simple Storage Service)
- **Serviço de armazenamento de objetos em nuvem**.  
- Muito utilizado em aplicações reais (ex: sistemas de pedágio como o **Sem Parar**, que consultam o S3 como primeira camada).  
- Pode armazenar:
  - Arquivos e logs.  
  - Movimentações de sistemas.  
  - Dados que alimentam aplicações.  

**Custos**
- **Armazenamento é gratuito**.  
- **O acesso/uso dos arquivos gera cobrança**.  

**Classes de Storage**
- **S3 Standard, Intelligent-Tiering, Standard-IA**: acesso frequente.  
- **S3 Glacier**: arquivos raramente acessados ("arquivos mortos").  
  - Mais barato para armazenar.  
  - Maior tempo para recuperação dos dados.  
  - Garantia de disponibilidade de **99,999999%**.  

---

✍️ *Autor: [Amanda Canto](https://github.com/AmandaCanto)*  
📅 *Projeto criado como parte do aprendizado em AWS.*
