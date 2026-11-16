# Fontes de Dados e Parâmetros de Processamento

Por motivos de dimensão e otimização de repositório, os ficheiros matriciais (*rasters* em formato GeoTIFF/GRID) e a *File Geodatabase* (`Trabalho.gdb`) não se encontram alojados diretamente aqui. 

Abaixo detalham-se todas as fontes oficiais e parâmetros utilizados para permitir a replicação integral da análise.

---

## 1. Dados Geográficos de Base

* **Limites Administrativos (CAOP):** Carta Administrativa Oficial Portuguesa — Direção-Geral do Território (DGT). Utilizada para extrair a máscara do concelho de Vila Nova de Gaia.
* **Ocupação do Solo (COS 2007):** Carta de Uso e Ocupação do Solo (Nível 2) — Direção-Geral do Território (DGT).
* **Condicionantes Legais (RAN):** Áreas da Reserva Agrícola Nacional — Direção-Geral de Agricultura e Desenvolvimento Rural (DGADR) / PDM de V. N. de Gaia.
* **Altimetria:** Curvas de nível e pontos cotados do concelho de V. N. de Gaia (utilizados para gerar o modelo TIN e o MDT).
* **Rede Hidrográfica:** Linhas de água do concelho de V. N. de Gaia.
* **Dados Socioeconómicos (BGRI):** Base Geográfica de Referenciação de Informação (Censos) — Instituto Nacional de Estatística (INE). Variáveis utilizadas: população residente reformada e população desempregada por subsecção estatística.

---

## 2. Parâmetros Técnicos e Normalização

* **Sistema de Coordenadas:** `ETRS_1989_Portugal_TM06` (EPSG:3763)
* **Resolução Espacial (Tamanho do Pixel):** `10 x 10 metros` em todas as conversões vetorial-matricial.

### Reclassificação e Lógica Fuzzy
* **BGRI (Vulnerabilidade Social):** Seleção de subsecções com valores superiores à média concelhia (reformados > 18,55 e desempregados > 7,47) → Distância Euclidiana → *Fuzzy Small*.
* **Declives (%):** Derivados do MDT (*Slope* em percentagem) → *Fuzzy Small*.
* **Hidrografia:** Distância Euclidiana → *Fuzzy Small*.
* **Uso do Solo (COS Nível 2):** Probabilidade condicional de transição urbano-agrícola:
  * Culturas temporárias: `0.2`
  * Culturas permanentes e áreas agrícolas heterogéneas: `0.1`
  * Restantes classes: `0.0`
* **RAN (Fator de Exclusão):** Reclassificada para `0` (áreas dentro da RAN) e `1` (restante território).

---

## 3. Fórmulas de Álgebra de Mapas (*Raster Calculator*)

* **Índice de Potencial para Agricultura Urbana:**
  ```text
  (("Fuzzy_BGRI" * 0.41) + ("COS_N2_Lookup" * 0.29) + ("Fuzzy_Dec_Perc" * 0.18) + ("Fuzzy_Hidro" * 0.12)) * "RAN_Reclass_Clip"
  ```

* **Superfície de Custo (Atrito para Caminhos Ótimos):**
  ```text
  ("Reclass_Declives" * 0.66) + ("Reclass_UsoSolo" * 0.34)
  ```text
