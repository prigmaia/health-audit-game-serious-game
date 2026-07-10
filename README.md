# 🏥 Simulador de Auditoria ONA: Serious Game Baseado em Inteligência Artificial

Este repositório contém o código-fonte de um **Serious Game (Jogo Educacional)** voltado para o ensino prático de **Auditoria Hospitalar e Gestão de Qualidade em Saúde**, desenvolvido originalmente no contexto acadêmico da **Universidade Federal de Minas Gerais (UFMG)**. 

O projeto utiliza uma arquitetura baseada em tecnologias web abertas (HTML5, CSS3, JavaScript) integrada de forma dinâmica a Modelos de Linguagem de Larga Escala (LLMs) via a plataforma WebSim, criando uma simulação interativa e imersiva para a detecção de **Não Conformidades (NCs)** segundo os critérios da **ONA (Organização Nacional de Acreditação)** e legislações da **ANVISA/Conselhos de Classe**.

---

## 🚀 Funcionalidades Principais

*   **Ambiente Multissetorial:** Simulação de 6 cenários críticos do ecossistema hospitalar:
    1.  Recepção / Admissão
    2.  Farmácia Central
    3.  UTI Adulto
    4.  Centro Cirúrgico
    5.  Central de Material e Esterilização (CME)
    6.  Engenharia Clínica / Manutenção
*   **NPC Interativa Baseada em Engenharia de Prompt:** A personagem **Elara (Coordenadora de Setor)** é gerada via IA. Inicialmente, ela adota uma postura defensiva, afirmando que todos os processos estão em conformidade. O usuário (Auditor) precisa investigar ativamente e solicitar as evidências corretas para que a IA confesse as falhas estruturais ou processuais.
*   **Mapeamento Legislações Vigentes:** O jogo embarca uma matriz de conformidade regulatória contendo normativas fundamentais (RDC 36/13, RDC 7/10, RDC 15/12, RDC 63/11, Resoluções CFM, COFEN, COFFITO e COSEMS).
*   **Prancheta do Auditor Dinâmica:** Um painel de consulta rápida dentro do jogo que ajuda o aluno a entender quais evidências (escalas, POPs, laudos, relatórios) devem ser cobradas em cada setor e quais leis dão suporte àquela exigência.
*   **Relatório Automatizado e Avaliação Continuada:** Ao encerrar o ciclo, o jogo processa o score do aluno (0 a 5 Não Conformidades detectadas) e exibe um feedback visual detalhado.
*   **Integração com Google Forms (Data Logging):** Os dados de identificação do aluno (Nome, Matrícula), o setor auditado e o status de cada Não Conformidade avaliada são disparados automaticamente em segundo plano via requisição assíncrona (`POST` no modo `no-cors`) para um formulário de controle do docente, viabilizando métricas de aprendizado da turma de forma centralizada.

---

## 🛠️ Detalhamento Técnico e Arquitetura

### Lógica de "Gatilhos" e Engenharia de Prompt
O grande diferencial pedagógico do simulador está na blindagem do prompt de sistema injetado na IA. Cada módulo possui uma diretiva estrita:
1.  **Postura Inicial:** Defender a perfeição dos fluxos.
2.  **Mecanismo de Confissão:** A IA só "quebra" a postura e revela a falha se palavras-chave de evidência física forem citadas pelo aluno (ex: *escala*, *inventário*, *in loco*, *prontuário*, *laudo*).
3.  **Strings Identificadoras:** Quando a falha é admitida, a IA obrigatoriamente injeta uma frase normalizada na resposta (ex: `"lacunas no final de semana"`, `"fisioterapeuta ausente no noturno"`). O motor em JavaScript captura essas expressões através de processamento de texto em tempo real (com normalização de acentos e caixa baixa), validando o score do usuário sem necessidade de um backend dedicado.

### Matriz de Setores e Evidências Regulatórias

| Setor | Item Avaliado | Evidência Exigível | Legislação de Lastro |
| :--- | :--- | :--- | :--- |
| **Farmácia Central** | Alta Vigilância, Termometria, Rastreabilidade | Escalas, Gráficos diários, POPs de Medicamentos | Lei 13.021, RDC 44/09, RDC 36/13, RDC 54/13 |
| **UTI Adulto** | Dimensionamento, Equipamentos Reserva, Notificações | Inventários, Contratos, Prontuários (Termos), Notivisa | RDC 7/10, RDC 36/13, Res. CFM 1/16, Portaria 529/13 |
| **Centro Cirúrgico** | Cirurgia Segura, Lateralidade, Alta da RPA | Checklist OMS, Registros de Contagem, Avaliação Pré | RDC 36/13, Res. CFM 1802, Res. CFM 2174, Res. CFM 1/16 |
| **Recepção/Admissão** | Classificação de Risco, Acessibilidade, Gestão do SAC | Escala Enfermeiro Triagem, Pulseiras In Loco, Relatórios | Res. Cofen 661, RDC 36/13, Dec. 5296/04, Dec. 11034 |
| **C.M.E.** | Testes Biológicos, Barreiras Físicas, Contingência | Livros de Registro, Planta Física, Planos Formais | RDC 15/12, RDC 50/02 |
| **Engenharia Clínica** | Manutenção Preventiva, Laudos de Água, Engenheiro RT | Cronogramas, Laudos de Laboratório, Registro CREA (ART) | RDC 63/11, Lei 5194/66 |

---

## 💻 Como Executar o Projeto

O simulador foi construído seguindo a filosofia de desenvolvimento ágil e portabilidade, rodando diretamente no navegador (**Client-Side**):

1.  Faça o clone deste repositório ou baixe o arquivo HTML.
2.  Certifique-se de ter um arquivo `styles.css` no mesmo diretório contendo as classes de estilização (modais, bolhas de chat, layout responsivo).
3.  Abra o arquivo `index.html` em qualquer navegador moderno.
4.  *Nota de Integração com IA:* Para o pleno funcionamento das rotas de chat (`websim.chat.completions`), este código deve ser carregado ou hospedado dentro de um ambiente que forneça a API de execução WebSim ou adaptado para o ecossistema de sua preferência (como OpenAI API, Anthropic, Gemini, etc.), alterando apenas o método receptor na função `sendToWebsim`.

---

## 📊 Coleta de Dados Acadêmicos (Dashboard do Professor)

O sistema conta com um endpoint que aponta para um formulário de submissão automática de notas. Caso queira replicar a infraestrutura para a sua turma:
1.  Crie um **Google Form** com campos para: Nome, Matrícula, Setor, NC1, NC2, NC3, NC4, NC5.
2.  Gere o link de preenchimento pré-preenchido para isolar os IDs de cada campo (`entry.XXXXXXXXX`).
3.  Substitua a variável `formURL` e as chaves de dicionário do `formData.append` no bloco JavaScript do código do jogo.

---

## 🗪 Como Contribuir

Fique à vontade para abrir **Issues** ou enviar **Pull Requests** com:
*   Novos setores hospitalares (ex: Pronto-Socorro, Banco de Sangue, Hemodiálise).
*   Atualizações e revisões das legislações da ANVISA baseadas em novas RDCs.
*   Melhorias na interface de usuário (UI) e responsividade.

---

## 📜 Licença

Este projeto está sob a licença MIT - consulte o arquivo de Licença para mais detalhes. O uso de caráter educacional e acadêmico é livre e incentivado, desde que citada a fonte e autoria.

***

**Desenvolvido como ferramenta de inovação pedagógica para Auditoria em Saúde - UFMG.**
