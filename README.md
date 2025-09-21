# Desafio1_Dio
## 📑 Relatório de Estudo

# ☁️ Arquitetura AWS – EC2, EBS, Lambda e S3

Este repositório apresenta uma arquitetura prática utilizando serviços da **Amazon Web Services (AWS)**, construída como parte dos meus estudos sobre **EC2, EBS, Lambda e S3**.  
O projeto inclui um **diagrama arquitetônico**, um **relatório detalhado de aprendizado** e explicações sobre boas práticas de uso dos recursos.

---

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

### 6. Snapshots de EBS
- Os Snapshots são backups pontuais dos volumes EBS.
-Eles armazenam o estado do disco em um momento específico, permitindo restauração em caso de falhas ou desastres.
-Esses snapshots são enviados e mantidos no Amazon S3 de forma gerenciada pela AWS (não aparecem no mesmo bucket criado pelo usuário).
- Possibilitam:
  - Restaurar volumes EBS a partir de um backup.
  - Clonar volumes para outras zonas de disponibilidade ou regiões.
  - Garantir maior resiliência e segurança dos dados.

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

## 📊 Diagrama Arquitetural
O diagrama representa o fluxo do sistema com:
- 1 instância **EC2**  
- 2 volumes **EBS**  
- Monitoramento com **CloudWatch**  
- Função **Lambda** para mover arquivos  
- Buckets **S3 Standard** e **S3 Glacier** para diferentes classes de armazenamento  

---

## 📖 Descrição do Diagrama

- **Usuários/Sistema**: acessam a aplicação principal hospedada em uma instância **Amazon EC2**.  
- **EC2 + EBS**: a instância utiliza dois volumes de **Amazon EBS** para armazenamento, sendo um destinado a dados ativos (**EBS-1**) e outro a dados que podem ficar inativos (**EBS-2**).  
- **Amazon CloudWatch**: responsável por monitorar e disparar eventos agendados. Ele ativa a função Lambda periodicamente para verificar arquivos armazenados.  
- **AWS Lambda**: função serverless que processa os arquivos e decide o destino de cada um, de acordo com o tempo de inatividade.  
- **Amazon S3**: serviço de armazenamento em nuvem utilizado em diferentes classes:  
  - **S3 Standard**: para arquivos com menos de 90 dias.  
  - **S3 Glacier**: para arquivos com mais de 90 dias, reduzindo custos de armazenamento de longo prazo.  


![Diagrama AWS](diagrama/aws_diagrama.png)

🔗 [Abrir diagrama editável no draw.io](https://viewer.diagrams.net/?tags=%7B%7D&lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=aws_diagrama.drawio&dark=auto#R%3Cmxfile%3E%3Cdiagram%20id%3D%22AWS%22%20name%3D%22AWS%20Architecture%22%3E7Vtbc9o6EP41PIaxJN94NJe0mUmanNC0PU8ZxRagU4OoLQL011eyZbAtkdIJxOGUXL2r%2B%2B5%2B0mq9tFBvuvqQ4PnkhkUkbkErWrVQvwUhRJ4n%2FknOOucAiPycM05opHhbxpD%2BJIppKe6CRiStVOSMxZzOq8yQzWYk5BUeThK2rFYbsbg66hyPicYYhjjWuV9pxCc514felv%2BR0PGkGBm4nbxkiovKaiXpBEdsWWHhCM85fSY9FrNETXTGZkSWokEL9RLGeP40XfVILGVbSC3v4nJH6WbaCZnxfRqg%2B1F38BOuOn48mg9%2BXPv0W%2BcCorybZxwvlDzUWvi6ENByQjkZznEo6aWwghbqTvg0FhQQj2LVc1kyXY2lhbSfcErDdhizRfSYSIXlA5CEk9XOqYONQIShETYlPFmLKqqB14ZO3kYZmV0Yz3KrMc%2Bx256qNikpzN3oQsl%2FvOl%2FKy3xoAT2J8Kzfi88EgljUyRL%2BISN2QzHgy23m7DFLCJyHEtQ2zrXjM2ViP8jnK8VcvCCs6oC8jHlQC%2BLV8yLLZKQvLCiAn84GRP%2B0srN6kpIjKW1VzF6aLEDTeoPw4fg%2FipoQTcWs%2B5G9Fk8jnkmm5z1lNQ5YuhKvZrepL1SsUlc4ycS37GUcspmouiJcc6m0uy%2FEx5OlNZK2hjROM7QnvWDAuCgQFYpOgxiOpYdcandLlZUKFRFRJPunFH5NHgWjHQHwkKahuwxxSPSFjun6KCNQ86Sx60xvApttlXBGkA61oDltg1QA%2FaxoOacOLDgnsCymwQW1IA1vBp%2BHtwEf44OnM7zs3pEV1IFLx0YqTiK0xETAmwvyVMZQpkm06wXxRHz6G0IVCbsckOYg89CMHKtcgFSBVEninC5wM4LRtlXDcbZgS3mzRP2ndSYB8AbtEAFcBA5GuB82Ia%2B4WxzjmQK%2Fonjzd4Tb16TeOv8JUJ2mxSyrW1qgx40eApfbnZ5BodzKtiCx3QmtpDiJiN1GOF0slHobzfWPd2IF7ZbvExRm4RQ91UuHd9Bch8VFSMqujr8Xgeg3a658q7BlTd6Fx10LIcSaJo6LSC6ewIxvzU1hURXR2J3KBgX4teEyP8BzJ5ZvJgSHWkDy4E2OirSIKq68a7hyuxAE868YzkV8NS9eO8kcOa9gDN9pzuDRbrXfu1UegdoAXp06Tq46fZfF%2BbQml4%2BfGr1UMv3sr%2FoVox4F9yLQayb2y%2BD%2B737Ce7%2Febj6cjs8uT06xtOn6l3w7bwhu3rz8w2hlrf2hQxBADGW9eE66F1Jc6jpsRIGy4JX2YycrvgRi%2Bnlv46o2pOcLJCrMU08T2cCvZr4B0wj1JkmnqczgV5NUsWsq0wTz3P0GddbA0NrUGstfsRhZ8LIiG0tUVip%2BL6U6q1brxcEALha1KIc4jg22uRM1akMYEEry7FMMSIDPO12QvJj%2BSqU8%2BkKMn%2Bq1hrHOKTZBA4AS7fmOnmmtw2GaEzBOzwo9bc0GSiHn4NP%2FeC%2Bf0blGZXvEZUpOhQga%2Beko0dI3xaQdhM3GSGtZP1Ntc%2BIfyUhjFaR%2FVW5sL8uU3ckoWLx0hwy5gGvRcX2%2BPt7UbPvDRu5fu5Smnc6WkONaq2jnX1NqPGQgj%2BNcB3Q43W969uH%2Ftfgc%2B%2Fj2eN4Q4%2FjLM5348ANPOB43b%2FGgcuyuJZYIPsRHsaVq1%2BtvIavVsUKKsdLHsGagMoW5%2F5YyFS97lRs23R2kSlKhsms%2BSqPiKnyeoxM6sfYkSy4SDNNyn6As7sjPJVWoSirI2Rm9a%2BCcqwtH0WL%2Fck17IjACR3yqmWZEv2Y0PYozrIaJzSKyEw7TBPGsbJq6zA24tQykEz3b9%2FSjeRo2UeFy3EKRhL%2FpUbSsfyGjUTPNPhT7UJvh3aDr7vVWPeF45jOU1La97NNfIfmNA%2F3dTpxqzqxHUPqIDQoxT%2BaUnQn9sRvD3Dfl5B5vlhjuYT6W8hzlu5Bs3QRqr2zfBeJukWW4jmJtPWmSaQIVrfe95BEChtJcNwE3DZEHnCDG7qhgFsh%2B6MngWdNgyTB61IFFUTY9nwnGSUDcqsxdlT%2FOFG9vmW9qr7tWzUDy2e8NbfN0k0WKMjtR6fy6tvPp6HBLw%3D%3D%3C%2Fdiagram%3E%3C%2Fmxfile%3E)


✍️ *Autor: [Amanda Canto](https://github.com/AmandaCanto)*  
📅 *Projeto criado como parte do aprendizado em AWS.*
