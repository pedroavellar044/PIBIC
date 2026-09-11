# Como o "Green for Gold" definiu a aptidão para ouro (e onde estão os dados)

Referência para construir a nossa camada de aptidão geológica seguindo o método do paper.

## Método (o que eles fizeram)
- **Base geológica:** o mapa geológico mais recente da África — contornos, **idade** e
  **composição química** das rochas (bedrock), do serviço geológico francês (BRGM). O coautor
  Guillaume Vic é geólogo de exploração do BRGM.
- **Classificação:** marcaram quais **camadas de rocha podem hospedar ouro**, com base em
  pesquisa geológica (que tipos/idades/composições de rocha contêm ouro). Feito pela geologia,
  NÃO a partir de onde havia mineração observada.
- **Medida por célula:** share da célula (grade 0,5°, PRIO-GRID) coberto por rocha apta → valor
  contínuo de 0 (nada apta) a 1 (totalmente apta). 18% da superfície da África é apta.
- **Validação (não construção):** compararam a aptidão geológica com 5 fontes de registros de
  ouro/garimpo (BRGM, GOLDATA, Min. de Minas de Burkina Faso, surveys IPIS, emissões de mercúrio).
  Correlação de 37%; 75% das células com registro caem em rocha apta. Eles PREFEREM a aptidão
  geológica porque os registros são "parciais, inconsistentes e endógenos".
- Detalhe unidade-a-unidade: no **apêndice do paper** (EJ).

## Lição para o nosso projeto
Replicar o método no mapa geológico do SGB (Amazônia): classificar as unidades de rocha por
favorabilidade a ouro (tipo/idade/composição; greenstone belts, sequências vulcanossedimentares,
granitóides de províncias auríferas), e usar as ocorrências de ouro (GEOBANK) só para VALIDAR.
Evitar o atalho de contar ocorrências (endógeno a onde se estudou).

## Links (dados abertos e paper)
- **Camada de aptidão (aberta, CC BY-NC-SA)** — figshare, DOI 10.60580/novasbe.28957058:
  https://novasbe.figshare.com/articles/dataset/Gold_suitable_geological_layers_for_the_African_continent_shp_and_tif_/28957058
  - `gold_suitable_geology.shp` (2,4 MB) — camadas classificadas (ver tabela de atributos)
  - `gold_suitable_geology.tif` — raster de aptidão
  - `gold_suit_X_priogrid.csv` — aptidão por célula PRIO-GRID
  - `README.pdf` — descrição do dataset
- **Paper publicado (EJ, apêndice tem o método detalhado):** https://doi.org/10.1093/ej/ueaf058
- **Working paper (ungated):** https://novafrica.org/wp-content/uploads/2022/03/2201.pdf
- **Pacote de replicação (Zenodo, 50 GB):** https://doi.org/10.5281/zenodo.15543090
- **Mapa geológico do SGB p/ a Amazônia (nosso insumo):**
  https://rigeo.sgb.gov.br/items/75255efe-a8ef-49dd-9ae6-96344fb2a913

## Próximo passo concreto (fazer no Claude Code)
1. Baixar o `gold_suitable_geology.shp` (pequeno) e ler a tabela de atributos → ver exatamente
   quais tipos de rocha eles classificaram como aptos.
2. Baixar o mapa geológico do SGB e listar suas unidades.
3. Montar a tabela de correspondência: unidade do SGB → apta/não apta (usando os tipos aptos do
   Green for Gold + províncias auríferas da CPRM como guia).
4. Validar contra as ocorrências de ouro do GEOBANK.
