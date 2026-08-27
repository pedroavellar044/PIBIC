# CLAUDE.md — Contexto do projeto PIBIC (ler antes de codar)

Este arquivo transfere o contexto construído no planejamento. Leia-o inteiro antes de sugerir código.

## O projeto
PIBIC (EPGE/FGV). Aluno: Pedro Cardoso Milazzo Avellar Leal. Orientador: Bruno Barsanetti.
**Pergunta:** quando a produção de cocaína andina aumenta, o garimpo cresce mais nos municípios
amazônicos conectados às rotas fluviais de tráfico do que em municípios semelhantes fora delas?

## Desenho empírico (decisões já tomadas)
- **Unidade:** município-ano (começar municipal; migrar para grid cells depois).
- **Desfecho:** área de garimpo por satélite (MapBiomas), agregada por unidade. Base de garimpo
  ilegal (Nature Comms 2024) para robustez; PRODES para desmatamento complementar.
- **Tratamento (rota):** município cortado por "rio-rota" = rio navegável (hidrovias DNIT), com
  nascente em país produtor (BO/CO/PE) e tributário do Amazonas. Usar os 16 rios já listados no
  *Landing on Water* (Pereira, Pucci & Soares 2024): Abunã, Acre, Amazonas, Caquetá, Envira, Içá,
  Japurá, Javari, Juruá, Madeira, Mamoré, Negro, Purus, Tarauacá, Uaupés, Xiê.
- **Exposição:** CocaExposure_it = ln(Σ_r D_ir · CocaProd_rt). Produção da origem via INCSR
  (nacional) + UNODC/SIMCI (provincial).
- **Identificação:** interação rota × produção andina (painel com FE de município e de estado-ano).
  Desenho alternativo/preferível para garimpo: **rota × preço do ouro × aptidão geológica**
  (tripla interação — a rota AMPLIFICA a resposta do garimpo ao preço).

## Decisões metodológicas críticas (não violar sem discutir)
1. **Preço do ouro é colinear com FE de tempo** → nunca entra sozinho; entra interagido com
   aptidão geológica (gera variação espacial). Segue Girard, Molina-Millán & Vic (2025, "Green for Gold").
2. **Aptidão geológica que multiplica o preço = SÓ geologia/geoquímica (exógena, fixa).**
   NÃO embutir acesso (drenagem/estrada/pista) nem garimpo observado na aptidão que entra na
   regressão causal — isso a torna endógena/circular.
3. **Acesso e rota são variáveis próprias, não controle-embutido.** Garimpo aluvionar segue
   drenagem; acesso ≈ rota (ambos seguem rio). Se acesso entrar na "aptidão", ele engole o efeito
   da rota (que é o tratamento). Modelo só-geologia erra; mas para causalidade, separar as camadas.
4. **Modelo de prospectividade completo (geologia+geoquímica+acesso, via MapBiomas como resposta)**
   = ferramenta DESCRITIVA e fonte do resíduo interpretável (onde deveria haver garimpo e não há =
   info sobre fiscalização/acesso). Calibrar fora-da-amostra para evitar circularidade.
5. **Recorte geográfico é questão em aberto:** o paper usa "Amazônia Ocidental" (AC, AM, RO, RR, MT)
   porque é perto dos Andes. MAS o Pará (Tapajós) é o maior polo de garimpo e fica de fora nesse
   recorte. Decidir com o orientador: Amazônia Ocidental (identificação limpa) vs. Amazônia Legal
   (captura o garimpo, mas exige repensar as rotas).
6. **Municípios novos (anos 1990):** usar Áreas Mínimas Comparáveis (AMC) de Ehrl (2017) para
   controles municipais ao longo do tempo. MapBiomas por satélite mantém fronteira fixa.

## Stack de ferramentas
- **Google Earth Engine** — extrair/agregar MapBiomas, PRODES, satélite (trabalho pesado na nuvem).
- **QGIS** — cruzamentos vetoriais (rios × municípios), inspeção, mapas.
- **R + `fixest`** (FE de alta dimensão + interações) — estimação; `sf`, `terra`, `geobr`.
  Alternativa Python: geopandas, rasterio, pyfixest.
- **Prospectividade:** random forest / logística (`ranger`/`tidymodels` ou scikit-learn).

## Fontes de dados
Ver `docs/bases_dados_garimpo_trafico.xlsx` (bases) e `docs/revisao_literatura_garimpo_trafico.xlsx`
(literatura). Principais: MapBiomas, PRODES/INPE, IBGE (malha), DNIT (hidrovias), GeoSGB/CPRM
(geologia/geoquímica), SIGMINE/ANM, FUNAI/ICMBio, World Bank/FRED (preço do ouro), INCSR/UNODC (coca).

## Convenções de repositório
- Dados pesados NÃO vão para o Git (ver `.gitignore`); reproduzir via scripts em `code/01_download/`.
- Scripts numerados por etapa: 01_download → 02_build → 03_analysis → 04_figures.
- Dados brutos intocados em `data/raw/`; derivados em `data/processed/` (parquet/gpkg).

## Estado atual / próximos passos
- Fase: revisão de literatura + definição do desenho. Ainda não começou o código.
- Próximo passo sugerido: montar o tratamento (interseção 16 rios × malha IBGE) num recorte pequeno
  (um estado, poucos anos) antes de escalar; validar a extração do MapBiomas no GEE.
