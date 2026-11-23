# Requisitos — Classificador de Documentos com a Tabela de Temporalidade do CONARQ

> Documento vivo da Fase 1 (Concepção). Atualizado incrementalmente durante a entrevista.

## 1. Problema
Classificar documentos de arquivo da administração pública federal e definir seus prazos de guarda e destinação final é um trabalho manual, repetitivo e sujeito a erro. Exige consultar o código de classificação e a tabela de temporalidade do CONARQ para cada documento.

## 2. Objetivos
- Prototipar um sistema híbrido: um modelo de ML prevê a classe do documento e uma base de regras (sistema especialista) aplica a tabela de temporalidade.
- Para cada documento, o sistema devolve: (a) a classe prevista, (b) o prazo de guarda nas fases corrente e intermediária e a destinação final, e (c) uma explicação do resultado (termos que pesaram no ML e a regra da tabela aplicada).
- Atingir, em um conjunto de teste rotulado à mão pelo grupo, acurácia ≥ 80% e F1 macro ≥ 0,75.

## 3. Stakeholders
| Nome/Papel | Interesse no projeto |
|---|---|
| Grupo de alunos | Desenvolver e entregar o trabalho |
| Professor da disciplina | Avaliar o relatório e o protótipo |

## 4. Escopo
- Cobre apenas um recorte (10 a 20 classes) do código de classificação e da tabela de temporalidade das **atividades-meio** do Poder Executivo federal. Versão do instrumento (Res. CONARQ nº 14/2001 ou atualização posterior): [a confirmar].
- Recorte escolhido: classes de **Pessoal** e **Material** (e **Patrimônio**, se couber). Códigos e nomes exatos: [a confirmar no documento oficial].
- Entrada do sistema: texto puro (colado ou carregado de um `.txt`).
- Pendência: confirmar a versão em vigor do instrumento. A Portaria AN/MGI nº 174/2024 revogou a Portaria AN nº 47/2020.

## 5. Fora do escopo
- Leitura de PDF e de documentos escaneados (OCR).
- Classes além de Pessoal e Material (e Patrimônio, se couber), incluindo atividades-fim.
- Interface web ou aplicativo: o protótipo é um notebook Python.
- Integração com sistemas reais de gestão documental (SEI e semelhantes).
- Documentos sigilosos ou com restrição de acesso: só dados públicos.
- Treinamento com deep learning pesado (ajuste fino de BERT). Modelos clássicos e embeddings prontos continuam no escopo.

## 6. Personas
| Persona | Necessidade principal |
|---|---|
| Servidor de arquivo/protocolo | Classificar documentos de Pessoal e Material e saber o prazo de guarda e a destinação final. Na demonstração, quem usa o notebook é o próprio grupo. |

## 7. Casos de uso
### UC-01 — Classificar um documento
- **Ator**: servidor de arquivo/protocolo (na demonstração, o grupo).
- **Pré-condição**: modelo treinado e base de regras carregadas.
- **Fluxo principal**: o usuário fornece o texto do documento, o sistema prevê a classe, consulta a tabela de temporalidade e exibe classe, prazos, destinação final e explicação.
- **Fluxo alternativo/erro**: texto vazio ou muito curto gera mensagem de erro. Confiança abaixo do limiar gera aviso de "revisão humana recomendada".

## 8. Requisitos funcionais
| ID | Descrição | Critério de aceite | Prioridade |
|---|---|---|---|
| RF-01 | Montar o corpus de documentos públicos de Pessoal e Material (e Patrimônio) | Corpus salvo em disco, com fonte e data de cada documento | Alta |
| RF-02 | Rotular o corpus: rótulos automáticos por regras para treino e conjunto de teste rotulado à mão | Conjunto de teste de ouro de 150 a 300 documentos, separado do treino | Alta |
| RF-03 | Treinar o classificador de classes | Modelo treinado e salvo, com métricas reportadas | Alta |
| RF-04 | Prever a classe de um documento a partir de texto puro | Devolve classe e grau de confiança | Alta |
| RF-05 | Consultar a tabela de temporalidade a partir da classe | Devolve prazo na fase corrente, na intermediária e destinação final conforme o instrumento oficial | Alta |
| RF-06 | Explicar o resultado | Mostra os termos que mais pesaram e a regra da tabela aplicada | Alta |
| RF-07 | Avaliar o sistema | Acurácia, F1 macro e matriz de confusão no conjunto de teste | Alta |
| RF-08 | Sinalizar baixa confiança | Abaixo do limiar definido pelo grupo, exibe aviso de revisão humana | Média |

## 9. Requisitos não funcionais
| ID | Categoria | Descrição |
|---|---|---|
| RNF-01 | Portabilidade | O notebook roda de ponta a ponta em Python, de forma reproduzível, com dependências listadas |
| RNF-02 | Reprodutibilidade | Sementes fixas; os mesmos dados geram as mesmas métricas |
| RNF-03 | Privacidade | Apenas dados públicos; nenhum documento sigiloso |
| RNF-04 | Desempenho | Classificar um documento em poucos segundos em computador comum |

## 10. Regras de negócio
| ID | Regra |
|---|---|
| RN-01 | Os prazos e a destinação seguem exclusivamente a tabela de temporalidade oficial em vigor; a versão adotada é citada no relatório |
| RN-02 | Cada classe prevista tem exatamente uma entrada correspondente na tabela de temporalidade |
| RN-03 | Nenhum documento do conjunto de teste pode ser usado no treino |

## 11. Integrações
N/A — confirmado com o usuário. O sistema roda isolado, sem conexão externa. O corpus é montado manualmente a partir de fontes públicas.

## 12. Restrições
- Prazo: entrega até domingo, 22/11/2026.
- Equipe: trabalho em grupo.
- Disciplina: Sistemas Especializados e Machine Learning.
- Entrega: relatório + protótipo funcional.
- Formato do protótipo: notebook/script Python.
- Dados: documentos públicos reais.

## 13. Critérios de aceite gerais
- Acurácia ≥ 80% e F1 macro ≥ 0,75 no conjunto de teste rotulado à mão.
- Notebook executa do início ao fim sem erro em ambiente limpo.
- Relatório entregue até 22/11/2026 descreve dados, métodos, resultados e limitações.

## 14. Glossário
| Termo | Definição |
|---|---|
| CONARQ | Conselho Nacional de Arquivos |
| Tabela de temporalidade | Instrumento que define prazos de guarda e destinação final (eliminação ou guarda permanente) dos documentos |
| Código de classificação | Esquema hierárquico de classes que organiza os documentos por assunto/função |
| Atividades-meio | Atividades de suporte e administração comuns a todos os órgãos, em oposição às atividades-fim |
| Fase corrente / intermediária | Fases do ciclo de vida do documento, com prazos de guarda distintos |
| Destinação final | Eliminação ou guarda permanente, ao fim dos prazos |
| Conjunto de teste de ouro | Documentos rotulados à mão pelo grupo, usados só para avaliar |
| F1 macro | Média do F1 de cada classe, sem ponderar pelo tamanho da classe |

---
**Status da fase**: [x] Em andamento  [ ] Aguardando aprovação  [ ] Aprovado em: ___
