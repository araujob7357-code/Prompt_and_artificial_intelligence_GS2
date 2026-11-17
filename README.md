# Citizen Assist AI — Assistente Digital de Orientação ao Cidadão

Protótipo educacional de assistente digital para orientação sobre documentos pessoais (RG, CPF, título de eleitor, carteira de trabalho, certidões e CNH), desenvolvido como parte do desafio **Global Solutions – Prompt and Artificial Intelligence**.

> ⚠️ **Obs:** este é um material didático. As fichas da "Central do Cidadão de Aurora" são fictícias. O assistente **não é um canal oficial** e não coleta nem armazena dados pessoais.

## Integrante

- Beatriz da Silva Araújo — RM 570619

## O que este projeto faz

- Recebe perguntas em linguagem natural sobre documentos pessoais
- Responde apenas com base nas 7 fichas de referência fornecidas (fonte de verdade única)
- Cita a ficha de origem (ex.: FC-01) e a data de verificação em todas as respostas
- Oculta automaticamente dados pessoais (CPF, telefone, e-mail, CEP) nas saídas
- Rejeita tentativas de *prompt injection*
- Inclui pipeline de **LLM as a Judge** para avaliar as respostas sobre um golden dataset de 26 mensagens

## Arquitetura

- **Framework:** LangGraph (grafo de estados com nós determinísticos e um único nó de LLM)
- **Modelo:** Groq `openai/gpt-oss-120b`
- **Onde a IA entra:** apenas na geração da resposta final e no juiz avaliador
- **Onde é lógica determinística:** validação de entrada, mascaramento de dados pessoais, detecção de injection, busca nas fichas, citação de fonte, auditoria

## Como executar

1. Abra o notebook `GS_PAI_Citizen_Assist_AI.ipynb` no Google Colab
2. Crie uma chave gratuita em [console.groq.com](https://console.groq.com)
3. No Colab, adicione o segredo `GROQ_API_KEY` (ícone de chave na barra lateral)
4. Execute todas as células (Ambiente de execução → Executar tudo)

O notebook cria automaticamente toda a estrutura do projeto, gera o zip e roda as avaliações.

## Estrutura do repositório
├── GS_PAI_Citizen_Assist_AI.ipynb # notebook principal (executado)
├── citizen_assist_ai.zip # projeto completo (fichas, módulos, testes)
├── golden_dataset.json # 26 mensagens de avaliação
├── relatorio.md (ou .pdf) # relatório do desafio
└── README.md

## Resultados (golden dataset)

| Versão | Acertos | Taxa |
|---|---|---|
| v1 (baseline) | 2/26 | 7,7% |
| v2 (com guardas) | 23/26 | 88,5% |

## Limitações

- Responde apenas com as 7 fichas fictícias; não consulta sistemas reais
- Detecção de dados pessoais e injection é baseada em regex (reduz risco, mas não é infalível)
- A busca é por palavras-chave; sinônimos fora da lista podem resultar em "não encontrei"
- O juiz é um LLM e pode errar; por isso há auditoria em código e conferência manual de amostra
