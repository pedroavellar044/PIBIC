# PIBIC — Rotas de Tráfico e Garimpo na Amazônia Brasileira

Projeto de Iniciação Científica (EPGE/FGV).
**Aluno:** Pedro Cardoso Milazzo Avellar Leal · **Orientador:** Bruno Barsanetti

## Pergunta de pesquisa
Quando a produção de cocaína nos países andinos aumenta, o garimpo cresce de forma
desproporcional nos municípios amazônicos conectados às rotas fluviais de tráfico,
em comparação com municípios semelhantes fora dessas rotas?

## Desenho empírico (resumo)
- **Unidade:** município-ano (Amazônia; recorte a definir — Ocidental vs. Amazônia Legal).
- **Desfecho:** área de garimpo por satélite (MapBiomas).
- **Tratamento:** exposição a rotas fluviais de tráfico (rios navegáveis com nascente em país
  produtor e tributários do Amazonas — 16 rios do *Landing on Water*).
- **Identificação:** interação exposição-à-rota × produção de cocaína andina; e/ou
  preço do ouro × aptidão geológica (só-geologia/geoquímica, exógena).
- **Aptidão geológica:** camada exógena (geologia + geoquímica). Acesso (drenagem/estrada/
  pista) e rota entram como variáveis próprias, NÃO dentro da aptidão.

## Estrutura do repositório
```
PIBIC/
├── data/
│   ├── raw/         # dados baixados, intocados (NÃO versionar — ver .gitignore)
│   └── processed/   # painéis e camadas prontas (parquet/gpkg)
├── code/
│   ├── 01_download/     # scripts de aquisição
│   ├── 02_build/        # construção do tratamento, exposição, aptidão
│   ├── 03_analysis/     # estimação (fixest) e robustez
│   └── 04_figures/      # mapas e gráficos
├── output/
│   ├── figures/
│   └── tables/
├── docs/            # projeto, revisão de literatura, notas
└── README.md
```

## Dados (fontes principais)
MapBiomas · PRODES/INPE · Malha municipal IBGE · Hidrovias DNIT · Geologia/geoquímica GeoSGB/CPRM ·
SIGMINE/ANM · FUNAI/ICMBio · Preço do ouro (World Bank/FRED) · Produção de cocaína (INCSR/UNODC).
Detalhes em `docs/bases_dados_garimpo_trafico.xlsx`.

## Ferramentas
Google Earth Engine · QGIS · R (`fixest`, `sf`, `terra`, `geobr`) · (alt.: Python geopandas/pyfixest).

## Status
Em fase de revisão de literatura e definição do desenho empírico.
