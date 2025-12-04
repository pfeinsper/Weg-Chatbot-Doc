# Chatbot RAG WEG

## Introdução e motivação do projeto

A WEG é uma multinacional brasileira de referência global em equipamentos eletroeletrônicos, com presença em mais de quarenta países e um portfólio amplo que vai de motores e sistemas de automação a soluções digitais. Essa diversidade, somada à heterogeneidade dos ambientes de uso, faz com que o suporte a ocorrências dependa fortemente de especialistas internos, o que aumenta o tempo entre solicitação, diagnóstico e solução.

No chão de fábrica, a ausência de estações de trabalho disponíveis para consulta a manuais ou sistemas de suporte torna o processo ainda mais fragmentado. Surge, portanto, a necessidade de um canal de atendimento acessível e escalável, capaz de reduzir a latência no diagnóstico e oferecer orientação prática de forma segura.

O objetivo deste projeto é desenvolver um **assistente conversacional**, que utiliza **RAG (Retrieval-Augmented Generation)** para consultar manuais e procedimentos técnicos, apoiar o troubleshooting e, quando necessário, realizar a triagem estruturada para encaminhamento ao especialista com o contexto consolidado.

A solução aumenta a eficiência do atendimento ao dar mais autonomia aos usuários e, ao mesmo tempo, otimiza o trabalho das equipes técnicas, que passam a focar nos casos críticos e complexos. Comprovada sua eficácia, a abordagem pode ser replicada em outros departamentos da empresa que enfrentam desafios operacionais semelhantes.

---

## Features

- API com deploy na AWS e com um front-end que simula o ambiente do **Microsoft Teams**.
- Recuperação de informações via RAG, utilizando **Azure OpenAI + ChromaDB**.
- Suporte a **PDFs e CSVs** como fontes de dados.
- Fluxo de decisão e refinamento de perguntas via **LangGraph**.
- Handoff automático para suporte humano quando necessário via e-mail.
- Utilização do modelo **gpt-oss-120b** da Azure OpenAI.

---

## Getting Started

> ⚡️ **Comece em minutos**  
> Instale as dependências, configure suas credenciais no `.env` e rode o servidor local.  
> Veja instruções detalhadas em [Getting Started](getting-started.md).

---

## Membros do grupo

- Henrique Badin
- Luca Caruso
- Eduardo Castanho
- Felipe Maia
