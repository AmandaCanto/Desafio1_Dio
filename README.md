# Desafio1_Dio
# Relatório de Estudo – Arquitetura AWS

## 1. Introdução
Este projeto demonstra uma arquitetura simples utilizando **EC2, EBS, Lambda e S3**, com envio automático de arquivos antigos para o S3.

## 2. EC2 (Elastic Compute Cloud)
- Instância de computação escalável.
- Usei como servidor principal do sistema.
- Permite anexar múltiplos volumes EBS.

## 3. EBS (Elastic Block Store)
- 2 volumes criados:
  - Volume 1: dados ativos
  - Volume 2: arquivos inativos (>90 dias)

## 4. Lambda
- Função que verifica os arquivos antigos e move para o S3.
- Pode ser disparada via CloudWatch Events.

## 5. S3 (Simple Storage Service)
- Bucket para armazenar arquivos antigos.
- Permite integração futura com Glacier.

## 6. Diagrama Arquitetural
O diagrama do projeto está na pasta `/diagrama`.

---
✍️ *Autor: [Amanda Canto](https://github.com/AmandaCanto)*
