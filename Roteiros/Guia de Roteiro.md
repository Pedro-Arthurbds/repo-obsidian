---
tags:
  - guia
  - roteiro
  - producao-de-conteudo
data_criacao: 2026-11-19
status: permanente
---

# 🎬 Guia Definitivo de Produção de Roteiros

> [!tip] Objetivo do Guia
> Este guia estabelece o padrão de escrita, planejamento e execução de roteiros no Obsidian. Ele serve como referência para vídeos longos (YouTube/aulas), vídeos curtos (Reels/Shorts) e tutoriais práticos com gravação de tela no OBS.

---

## 📌 1. A Anatomia de um Roteiro Eficaz

Todo roteiro de alto impacto segue uma estrutura lógica de retenção de atenção:

```text
[ Gancho (0-15s) ] ➔ [ Promessa & Contexto ] ➔ [ Desenvolvimento em Blocos ] ➔ [ Clímax / Revelação ] ➔ [ CTA ]
```

### 1.1. O Gancho (Hook — primeiros 5 a 15 segundos)
- **Regra dos 5 segundos:** Apresente o problema central ou uma quebra de expectativa imediatamente.
- Evite vinhetas longas ou saudações demoradas ("Fala galera, tudo bem com vocês?").
- *Exemplo de gancho eficaz:* "Se o seu código trava em listas gigantes, o problema quase sempre não é o hardware, mas a complexidade do algoritmo. Hoje vamos resolver isso em 3 passos."

### 1.2. A Promessa & Contextualização
- Deixe claro o que o espectador vai aprender ou resolver se ficar até o final.
- Conecte o tema a um objetivo prático (ex: "Ao final deste vídeo, você terá sua API configurada com autenticação segura").

### 1.3. O Desenvolvimento (Corpo Principal)
- Divida o conteúdo em **blocos temáticos ou tópicos bem definidos**.
- Alterne entre explicação teórica, exemplo em tela e pausas de respiração/reflexão.
- Mantenha a regra de progressão: do conceito mais simples ao mais avançado.

### 1.4. Clímax & Conclusão
- O ápice da entrega (a resolução do problema, o teste do código funcionando ou o insight principal).
- Feche os loops abertos no início do vídeo.

### 1.5. CTA (Call to Action)
- Escolha **uma única ação prioritária** por vez (ex: comentar uma dúvida, conferir o repositório ou inscrever-se no canal).

---

## ⏱️ 2. Estimativa de Tempo e Ritmo

A velocidade média de fala em vídeos e tutoriais gira em torno de 130 a 150 palavras por minuto. 

Para calcular a duração estimada do roteiro, use a relação:

$$\text{Duração (min)} = \frac{\text{Total de Palavras}}{140}$$

- **Vídeo de 1 minuto (Shorts/Reels):** $\approx 120 \text{ a } 140 \text{ palavras}$.
- **Vídeo de 5 minutos:** $\approx 650 \text{ a } 750 \text{ palavras}$.
- **Vídeo de 10 minutos:** $\approx 1300 \text{ a } 1500 \text{ palavras}$.

---

## 📋 3. Formatos de Roteiro Recomendados

Escolha o formato que melhor se adapta à complexidade do conteúdo:

| Formato | Indicação | Vantagens | Desvantagens |
| :--- | :--- | :--- | :--- |
| **Duas Colunas (Áudio & Vídeo)** | Tutoriais com OBS, gameplays, vídeos editados com B-Roll | Alinhamento perfeito entre o que você fala e o que aparece na tela | Leva mais tempo para planejar |
| **Roteiro em Tópicos (Bullet Points)** | Aulas ao vivo, conteúdo espontâneo, revisões rápidas | Fala mais natural e fluida; menor sensação de teleprompter | Exige maior domínio do tema para não perder o foco |
| **Roteiro Palavra por Palavra** | Vídeos curtos (Reels/Shorts), introduções densas, anúncios | Controle milimétrico do tempo e da retenção | Pode soar engessado se for lido sem entonação |

---

## 🎙️ 4. Boas Práticas de Gravação com OBS

> [!important] Checklist Rápido de Pré-Gravação
> 1. **Áudio:** Verifique os níveis de volume no mixer do OBS (picos entre -12 dB e -6 dB para evitar distorção).
> 2. **Cena & Captura:** Teste se a captura de janela/tela está pegando a resolução correta e sem bordas cortadas.
> 3. **Ambiente:** Feche janelas, notificações do sistema e abafe ruídos de eco no ambiente.
> 4. **Teste de 10 segundos:** Grave 10 segundos antes de iniciar o roteiro real para testar o som do microfone e vídeo.

---

## 🗂️ 5. Fluxo de Trabalho no Obsidian

Organize seus roteiros na pasta `Second Brain/Roteiros/` utilizando as tags de estado para acompanhar o pipeline de produção:

- `#roteiro/ideia`: Tema anotado, aguardando estruturação.
- `#roteiro/em-escrita`: Roteiro sendo desenvolvido.
- `#roteiro/pronto`: Texto pronto para gravação.
- `#roteiro/gravado`: Arquivos brutos gerados no OBS.
- `#roteiro/editado`: Vídeo finalizado.
- `#roteiro/publicado`: Conteúdo no ar.

### Modelo Reutilizável
Para iniciar um novo roteiro, utilize o modelo padronizado:
- [[Template - Roteiro]]

---

## 📊 Acompanhamento de Produção (Dataview)

```dataview
TABLE status, plataforma, duracao_estimada AS "Duração"
FROM #roteiro
WHERE file.name != "Template - Roteiro" AND file.name != "Guia de Roteiro"
SORT file.mtime DESC
```
