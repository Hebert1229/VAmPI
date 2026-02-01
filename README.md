🔐 Security Pipeline – GitHub Actions

Este repositório contém um pipeline de segurança automatizado implementado com GitHub Actions, projetado para executar análises de segurança estática, de dependências e dinâmica, além de centralizar os resultados no DefectDojo. O pipeline é executado em um runner self-hosted e segue uma abordagem de defesa em profundidade no contexto de CI/CD.

📌 Visão Geral do Pipeline

O pipeline é acionado automaticamente a cada push na branch main e executa as seguintes etapas:

Análise estática de código (SAST) com Horusec

Build da aplicação Java utilizando Maven

Análise de dependências (SCA) com Snyk

Análise dinâmica (DAST) com OWASP ZAP (Baseline Scan)

Upload centralizado dos resultados no DefectDojo

Limpeza do workspace do runner

O pipeline foi desenhado para não bloquear o build, permitindo a coleta contínua de evidências de segurança mesmo em ambientes de baixa maturidade.

🧱 Estrutura dos Jobs
🧪 run-horusec – Execução dos testes de segurança

Responsável por executar todas as ferramentas de análise de segurança.

Ferramentas utilizadas:

Horusec – Análise estática de código (SAST)

Maven – Build da aplicação (com testes ignorados)

Snyk – Análise de dependências (SCA)

OWASP ZAP – Análise dinâmica (DAST – baseline)

Principais características:

Execução em runner self-hosted

Geração de relatórios em formato JSON e HTML

Relatórios versionados por nome do projeto e número do build

Uso de || true para evitar falha do pipeline em caso de vulnerabilidades

📤 upload-defectdojo – Upload dos relatórios

Responsável por enviar os relatórios de segurança gerados para o DefectDojo, centralizando a gestão de vulnerabilidades.

Relatórios enviados:

Horusec (SAST)

Snyk (SCA)

Tecnologia utilizada:

Scripts Python utilizando a API REST do DefectDojo (/api/v2/import-scan/)

🧹 clean-up – Limpeza do ambiente

Executado sempre, independentemente do status dos jobs anteriores.

Ação:

Remove o diretório de trabalho do runner para evitar acúmulo de artefatos e vazamento de dados sensíveis.

🔐 Variáveis de Ambiente e Secrets
Variáveis de ambiente configuradas no workflow

PROJECT_NAME – Nome do projeto analisado

BUILD_NUMBER – Número do build (GitHub Run Number)

REPORT_PATH – Diretório onde os relatórios são armazenados

ZAP_TARGET_URL – URL alvo para o scan do OWASP ZAP

DEFECTDOJO_HOST – Endereço do DefectDojo

Secrets obrigatórios (GitHub Secrets)

DEFECTDOJO_TOKEN – Token de autenticação da API do DefectDojo

DEFECTDOJO_PRODUCT_ID – ID do produto no DefectDojo

DEFECTDOJO_ENGAGEMENT_ID – ID do engagement no DefectDojo

⚠️ Nunca versionar tokens ou credenciais diretamente no código.

🖥️ Requisitos do Runner Self-Hosted

O runner deve possuir as seguintes dependências instaladas:

Docker

Horusec CLI

Maven

Snyk CLI (autenticado previamente)

Python 3 + biblioteca requests

Permissão para execução de containers Docker

Acesso ao DefectDojo

🛡️ Boas Práticas Aplicadas

Defesa em profundidade (SAST, SCA e DAST)

Centralização de vulnerabilidades no DefectDojo

Versionamento e rastreabilidade de relatórios

Execução isolada em runner self-hosted

Limpeza automática do ambiente após execução

🎯 Objetivo do Pipeline

Este pipeline tem como objetivo aumentar a visibilidade de riscos de segurança, promover shift left e fornecer insumos contínuos para o processo de gestão de vulnerabilidades, sem comprometer a velocidade de entrega do time de desenvolvimento.
