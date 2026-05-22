<div align="center">

# Gerenciamento de Instâncias EC2 na AWS

[![AWS](https://img.shields.io/badge/Amazon_AWS-232F3E?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![EC2](https://img.shields.io/badge/Amazon_EC2-FF9900?style=for-the-badge&logo=amazon-ec2&logoColor=white)](https://aws.amazon.com/ec2/)
[![Lambda](https://img.shields.io/badge/AWS_Lambda-FF9900?style=for-the-badge&logo=aws-lambda&logoColor=white)](https://aws.amazon.com/lambda/)
[![S3](https://img.shields.io/badge/Amazon_S3-569A31?style=for-the-badge&logo=amazon-s3&logoColor=white)](https://aws.amazon.com/s3/)
[![Status](https://img.shields.io/badge/Status-Conclu%C3%ADdo-brightgreen?style=for-the-badge)](.)

**Bootcamp GFT — Fundamentos de Cloud com AWS** · [DIO](https://www.dio.me/)

</div>

---

## 🎯 Objetivo

Este laboratório tem como objetivo consolidar conhecimentos em gerenciamento de instâncias EC2 na AWS. O entregável é um repositório organizado contendo anotações e insights adquiridos durante a prática, servindo como material de apoio para estudos e futuras implementações.

---

## ⚙️ Tecnologias Utilizadas

| Serviço | Função na Arquitetura |
|---|---|
| **Amazon EC2** | Instância de computação para processamento pesado |
| **Amazon S3** | Armazenamento de objetos de entrada e saída |
| **AWS Lambda** | Orquestração serverless orientada a eventos |
| **Amazon EBS** | Volume de bloco persistente para a instância EC2 |
| **EBS Snapshot** | Backup incremental do volume para recuperação de dados |
| **Amazon VPC** | Rede virtual isolada para controle de tráfego |

---

## 🏗️ Arquitetura

A arquitetura proposta simula uma plataforma de processamento de imagens de produtos para e-commerce — redimensionamento, normalização de cores e remoção de fundo. O volume de uploads é imprevisível e o processamento exige bibliotecas pesadas de visão computacional.

![Diagrama de Arquitetura](images/arquitetura-aws.png)

> Arquivo editável disponível em: [`arquitetura-aws.drawio`](arquitetura-aws.drawio)

### Componentes da Arquitetura

| Elemento | Descrição |
|---|---|
| **Usuário** | Origem do fluxo — realiza o upload da imagem |
| **S3 — imagens-originais** | Bucket de entrada; armazena as imagens brutas enviadas |
| **AWS Lambda** | Função serverless que orquestra o fluxo; executa somente quando acionada por evento |
| **Amazon VPC** | Rede virtual isolada envolvendo a EC2 e o EBS |
| **EC2 Instance** | Instância que executa o processamento pesado (filtros, OCR, redimensionamento) |
| **EBS Volume** | Disco persistente da EC2; armazena SO, aplicação e arquivos temporários |
| **S3 — imagens-processadas** | Bucket de saída; armazena as imagens após o processamento |
| **EBS Snapshot** | Cópia de segurança periódica do volume EBS |

### Fluxo de Interações

| # | De → Para | Tipo | Descrição |
|---|---|---|---|
| 1 | Usuário → S3 (originais) | Upload | Envio da imagem para o bucket de entrada |
| 2 | S3 (originais) → Lambda | S3 Event Trigger | Chegada do arquivo dispara automaticamente a função Lambda |
| 3 | Lambda → EC2 | AWS SDK | Lambda aciona a EC2 via SDK para iniciar o processamento |
| 4 | EC2 ↔ EBS | Leitura / Escrita | EC2 lê a aplicação e grava arquivos temporários no volume EBS |
| 5 | EC2 → S3 (processadas) | Gravação | EC2 salva a imagem processada no bucket de saída |
| 6 | EBS → EBS Snapshot | Snapshot periódico | Backup automático do volume EBS |

---

## 💡 Aprendizados

### Amazon EC2 (Elastic Compute Cloud)

Máquina virtual na nuvem com controle total sobre SO, runtime e configurações. Ideal para workloads que exigem processamento contínuo ou customização de ambiente. Ao contrário do Lambda, permanece ativa enquanto ligada e cobra por tempo de uso.

### Amazon S3 (Simple Storage Service)

Armazenamento de objetos altamente durável e escalável. Cada objeto é acessado por uma chave única dentro de um bucket. Eventos nativos (como `s3:ObjectCreated`) permitem acionar outros serviços automaticamente, sem polling.

### Amazon EBS (Elastic Block Store)

Volume de bloco persistente acoplado à EC2, funcionando como um disco externo na nuvem. Os dados sobrevivem a reinicializações da instância, mas o volume fica vinculado a uma única EC2 e à mesma zona de disponibilidade.

### EBS Snapshot

Cópia incremental do volume EBS armazenada no S3 gerenciado pela AWS. Apenas os blocos alterados desde o último snapshot são gravados, tornando o processo eficiente. Útil para backup, migração entre regiões e criação de AMIs.

### AWS Lambda

Execução de código orientada a eventos sem necessidade de gerenciar servidores. Cobra apenas pelo tempo de execução e escala automaticamente. Ideal como orquestrador leve — recebe o evento do S3 e delega o processamento pesado para a EC2.

---

## 🗂️ Estrutura do Repositório

```
gft-aws-fundamentos/
├── README.md
├── arquitetura-aws.drawio       # Diagrama editável
└── images/
    └── arquitetura-aws.png      # Diagrama renderizado
```

---

## ✅ Conclusão

Este laboratório consolidou a compreensão sobre como os principais serviços de computação e armazenamento da AWS interagem em uma arquitetura real. A separação de responsabilidades entre Lambda (orquestração), EC2 (processamento pesado), S3 (armazenamento) e EBS (persistência) demonstra boas práticas de design de soluções em nuvem, com atenção especial à escalabilidade e à resiliência dos dados.

---

## 📚 Referências

- [Documentação Amazon EC2](https://docs.aws.amazon.com/ec2/)
- [Documentação Amazon S3](https://docs.aws.amazon.com/s3/)
- [Documentação AWS Lambda](https://docs.aws.amazon.com/lambda/)
- [Documentação Amazon EBS](https://docs.aws.amazon.com/ebs/)
- [DIO — GFT Fundamentos de Cloud com AWS](https://www.dio.me/)

---

<div align="center">

Desenvolvido como parte do bootcamp **GFT — Fundamentos de Cloud com AWS** na [DIO](https://www.dio.me/)

</div>
