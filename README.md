# 🌾 Identificação de doenças em folhas de cana-de-açúcar com Inteligência Artificial

**Estudo acadêmico de classificação de imagens com redes neurais convolucionais (CNN) e transferência de aprendizado (transfer learning).**

Projeto desenvolvido por **Claudecir Menezes Francisco**, em novembro de 2026, na disciplina de **Redes Neurais**, da pós-graduação em **Inteligência Artificial Aplicada a Processos Industriais — PUC Minas**, ministrada pela **Dra. Izabela Batista da Silva**.

> **Resultado principal:** a MobileNetV2 com ajuste fino atingiu **88,3% de acurácia** em **343 fotos de teste** não utilizadas no treinamento (intervalo aproximado de **±3,4 pontos percentuais**, conforme a apresentação).

## 🎯 Qual foi a ideia?

Doenças como mosaico, ferrugem, podridão vermelha e amarelinho podem deixar sinais visíveis nas folhas de cana-de-açúcar. Este estudo investigou se uma rede neural conseguiria reconhecer esses padrões em fotografias e, futuramente, apoiar uma triagem feita diretamente no campo.

**O projeto é experimental.** Não é um aplicativo pronto nem substitui a avaliação de um profissional especializado.

## 🖼️ Base de imagens

Foram utilizadas **2.521 fotografias** de folhas de cana-de-açúcar, coletadas em Maharashtra, na **Índia**, e disponibilizadas por **Daphal e Koli (2022)** no *Mendeley Data*, sob licença **CC BY 4.0**. O estudo dos autores foi publicado em 2023.

As imagens pertencem a cinco classes:

| Classe | Imagens originais |
| --- | ---: |
| Folha sadia | 522 |
| Mosaico | 462 |
| Podridão vermelha | 518 |
| Ferrugem | 514 |
| Amarelinho | 505 |
| **Total** | **2.521** |

## 🧹 Preparação e cuidados com os dados

Uma das descobertas mais importantes foi que **um modelo simples acertava 56% das classes usando apenas a largura e a altura das fotografias**, sem analisar as folhas. Isso revelou uma possível pista indevida relacionada à forma como as imagens haviam sido coletadas.

Para reduzir esse problema e evitar resultados artificiais:

- As imagens foram **recortadas pelo centro** e redimensionadas para **224 × 224 pixels**.
- Foram identificadas **210 cópias exatas** (MD5) e **26 imagens muito semelhantes** (pHash), deixando **2.285 imagens únicas**.
- A separação ocorreu **depois da limpeza e antes do aumento de dados**.
- O aumento de dados, com variações de posição, brilho e contraste, foi aplicado **somente ao conjunto de treinamento**.

**Divisão final:** 1.599 imagens para treinamento, 343 para validação e 343 para teste.

## 🧠 Modelos comparados

Foram avaliadas três estratégias:

1. **CNN construída do zero:** quatro blocos convolucionais para aprender padrões diretamente nas imagens de cana.
2. **MobileNetV2 com base congelada:** aproveitamento dos padrões visuais aprendidos previamente em outra base de imagens, treinando apenas o classificador final.
3. **MobileNetV2 com ajuste fino:** adaptação de parte da rede pré-treinada às imagens das folhas de cana.

### 📊 Resultados nas imagens de teste

| Modelo | Acurácia |
| --- | ---: |
| CNN construída do zero | 85,4% |
| MobileNetV2 com base congelada | 84,5% |
| **MobileNetV2 com ajuste fino** | **88,3%** |

Os resultados se referem ao mesmo conjunto de **343 imagens de teste**. A MobileNetV2 com ajuste fino apresentou o melhor desempenho nesse experimento.

### Resultados por classe no melhor modelo

| Classe | Imagens corretamente identificadas na classe* |
| --- | ---: |
| Folha sadia | 94% |
| Ferrugem | 94% |
| Podridão vermelha | 91% |
| Mosaico | 88% |
| Amarelinho | 76% |

\* Percentuais por classe apresentados na matriz de confusão da apresentação (sensibilidade/recall, aproximadamente).

O **amarelinho** foi a classe mais difícil de reconhecer. O estudo também identificou casos de **mosaico confundido com folha sadia**.

## 🔎 Investigando os erros

Além de calcular a acurácia, foram analisadas **40 classificações incorretas** entre as 343 fotos de teste. Entre as dificuldades observadas estavam sintomas discretos, imagens com muitas folhas e casos de baixa confiança na previsão.

Foi utilizado o **Grad-CAM**, uma técnica que ajuda a visualizar as regiões da imagem que mais influenciaram determinadas classificações. Os exemplos apresentados sugerem atenção relevante às folhas, mas esse recurso **não prova, por si só, que o modelo está livre de vieses**.

## ⚠️ Limitações

- As fotos vieram de **uma região da Índia**; ainda não há validação com folhas de lavouras brasileiras.
- O modelo reconhece **apenas cinco classes**; uma doença diferente pode ser classificada incorretamente como uma das classes conhecidas.
- O desempenho foi estimado em um conjunto de **343 imagens de teste**.
- A execução **diretamente em celular e sem internet ainda não foi testada**.
- Não foi feita uma comparação direta, com a mesma base de cana-de-açúcar, entre este modelo e assistentes gerais de IA.

## 🚀 Próximos passos

- Testar com fotos de lavouras brasileiras, com avaliação de um especialista da área.
- Ajustar o modelo com novas imagens, caso necessário.
- Investigar sua execução em **celular, sem depender de internet**.
- Explorar possíveis integrações futuras com outras ferramentas de monitoramento agrícola.

## 📄 Apresentação do projeto

A apresentação de **21 páginas** reúne a motivação do estudo, os cuidados com a base, a arquitetura das redes, os gráficos de treinamento, a matriz de confusão, exemplos de erros e mapas Grad-CAM.

📎 **[Abrir a apresentação técnica completa (PDF)](apresentacao.pdf)**

> Para que o link acima funcione, envie o PDF ao repositório com o nome `apresentacao.pdf`, na mesma pasta deste README.

## 💻 Código e reprodução

O notebook reúne todas as etapas do experimento, desde a preparação das imagens até o treinamento, a comparação e a avaliação das três redes neurais.

O estudo inclui:

- Preparação e limpeza da base de imagens.
- Identificação e remoção de imagens duplicadas.
- Desenvolvimento de uma CNN do zero.
- Treinamento da MobileNetV2 com transferência de aprendizado.
- Ajuste fino da MobileNetV2.
- Comparação dos resultados e análise dos erros.
- Visualização das regiões das imagens com Grad-CAM.

📓 **[Acessar o notebook completo](classificacao_doencas_cana_portfolio_revisado.ipynb)**

**Ambiente utilizado:** Google Colab com GPU NVIDIA T4.

Para reproduzir o experimento, é necessário obter a base de imagens original e ajustar os caminhos de carregamento dos dados.

**Melhor resultado:** 88,3% de acurácia em 343 imagens de teste.


## 📚 Referências e créditos

- **Daphal, P.; Koli, S.** Base de imagens de doenças em folhas de cana-de-açúcar, Mendeley Data (2022), licença CC BY 4.0; estudo relacionado publicado em *Heliyon* (2023).
- **MobileNetV2:** arquitetura utilizada nos experimentos de transferência de aprendizado.
- **PUC Minas:** contexto acadêmico do projeto, desenvolvido na disciplina de Redes Neurais.

**Autoria do projeto:** Claudecir Menezes Francisco.

*Projeto acadêmico pessoal e independente, sem vínculo com a empresa em que o autor trabalha. A menção à PUC Minas identifica o contexto de formação, não representa endosso institucional.*
