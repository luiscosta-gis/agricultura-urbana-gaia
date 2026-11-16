# Estudo do Potencial dos Solos para Agricultura Urbana — Vila Nova de Gaia

Análise espacial e modelação multicritério em ambiente SIG para identificar áreas prioritárias destinadas à implementação de hortas/agricultura urbana no concelho de Vila Nova de Gaia, integrando variáveis biofísicas e socioeconómicas.

* **Contexto:** Mestrado em Sistemas de Informação Geográfica e Ordenamento do Território (FLUP)
* **Autores:** Luís Costa e Diogo Nogueira | **Docente:** Prof.ª Patrícia Abrantes
* **Ferramentas:** ArcGIS (Spatial Analyst, Fuzzy Logic, Cost Distance / Cost Path)
* **Sistema de Coordenadas:** `ETRS_1989_Portugal_TM06`

---

## 🎯 Objetivos
1. Avaliar a aptidão do território de V. N. de Gaia para a prática de agricultura urbana através de uma **Análise Multicritério (AMC)**.
2. Priorizar a proximidade a populações socialmente mais vulneráveis (desempregados e reformados).
3. Propor **5 localizações prioritárias** e calcular os **caminhos ótimos (menor custo)** de ligação entre esses locais e o **Parque Biológico de Vila Nova de Gaia**.

---

## 🗺️ Resultado Final
![Mapa de Potencial e Caminhos Ótimos](mapas/mapa-final-potencial-caminhos.png)

---

## ⚙️ Metodologia e Critérios

### 1. Dados de Base e Normalização (Resolução: 10m)
* **Variável Social (BGRI):** Seleção de subsecções estatísticas com valores de reformados e desempregados acima da média concelhia, seguida de distância euclidiana e normalização *Fuzzy Small*.
* **Ocupação do Solo (COS 2007):** Atribuição de probabilidade condicional de transição urbano-agrícola (0.2 culturas temporárias; 0.1 culturas permanentes e agrícolas heterogéneas).
* **Declives (%):** Derivados de MDT (interpolação TIN de curvas de nível e pontos cotados) e normalizados via *Fuzzy Small*.
* **Rede Hidrográfica:** Distância euclidiana normalizada via *Fuzzy Small*.
* **Condicionante Legal (RAN):** Áreas da Reserva Agrícola Nacional reclassificadas como fator de exclusão (`0` = RAN; `1` = Restante território).

### 2. Ponderação Multicritério (Escala de Osgood, 1 a 7)

| Critério | Pontuação | Peso Final |
| :--- | :---: | :---: |
| **Fator Social (BGRI)** | 7 | **0.41** |
| **Uso e Ocupação do Solo (COS)** | 5 | **0.29** |
| **Declives (%)** | 3 | **0.18** |
| **Proximidade à Hidrografia** | 2 | **0.12** |
| **Total** | **17** | **1.00** |

### 3. Superfície de Custo e Caminhos Ótimos
* Criação de superfície de atrito combinando **Declives (0.66)** e **Uso do Solo (0.34)**.
* Aplicação das ferramentas *Cost Distance* e *Cost Path* tendo como destino o **Parque Biológico de V. N. de Gaia**.

---

## 📂 Estrutura do Repositório
* 📄 **[Relatório Completo (PDF)](docs/relatorio-agricultura-urbana-gaia.pdf)** — Documento técnico detalhado com todos os passos, parâmetros e layouts.
* 🖼️ **[Mapas](mapas/)** — Cartografia final em alta resolução.
* 📍 **[Resultados Vetoriais](resultados/)** — Camadas leves (Locais Prioritários e Caminhos Ótimos) prontas a visualizar.