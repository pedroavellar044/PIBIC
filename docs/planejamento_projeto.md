# Planejamento do projeto — registro de decisões

Documento de contexto (decision log) do PIBIC "Rotas de Tráfico e Garimpo na Amazônia".
Complementa o `CLAUDE.md` (instruções curtas) com o raciocínio por trás de cada escolha.
Aluno: Pedro Cardoso Milazzo Avellar Leal · Orientador: Bruno Barsanetti.

---

## 1. Pergunta e motivação
Quando a produção de cocaína andina aumenta, o garimpo cresce **desproporcionalmente** nos
municípios conectados às rotas fluviais de tráfico, ante municípios semelhantes fora delas?

Motivação: violência e degradação ambiental cresceram na Amazônia associadas a economias ilícitas.
Há literatura de tráfico→violência e de garimpo→violência, e a de "narco-deforestation" (América
Central), mas **falta evidência causal, no Brasil, ligando rotas de tráfico à expansão do garimpo**.
Esse é o gap.

## 2. Desenhos candidatos e veredito
Discutimos três formas de identificação:

1. **DiD simples com a Lei do Abate (rota × pós-2004).** Fraco para garimpo: o pós-2004 coincide
   com o boom do ouro, então confunde o canal do tráfico com o efeito do preço. Além disso, 2×2
   descarta a variação anual e depende de tendências paralelas frágeis entre rios-rota e não-rota.
2. **Tripla diferença "de tráfico": rota × produção de cocaína andina × pós-2004.** É o desenho do
   *Landing on Water* aplicado ao garimpo. A Lei do Abate entra como o teste placebo (antes de 2004
   a exposição não deveria mexer no garimpo). Isola o canal da oferta de cocaína. Limitação: exige
   janela pré-2004 razoável no MapBiomas.
3. **Tripla diferença "do ouro": rota × preço do ouro × aptidão geológica.** Provavelmente a melhor
   para garimpo: centra o driver real (preço do ouro), pergunta se **a rota amplifica** a resposta
   do garimpo ao preço, tem variação temporal em todo o período e ataca de frente a crítica
   "garimpo mora perto de rio porque é lá que está o ouro".

**Veredito:** a tripla diferença é claramente melhor que o DiD simples. Entre as duas triplas, a
nº 3 como principal e a nº 2 (ou a Lei do Abate) como checagem de mecanismo. Se ambas apontarem na
mesma direção, o argumento fica muito mais forte.

## 3. Construção do tratamento (rota vs. não-rota)
Procedimento do *Landing on Water*, reconstruído:
1. Partir das **hidrovias navegáveis** do Ministério da Infraestrutura/DNIT (não a hidrografia
   genérica da ANA — rota precisa de rio navegável).
2. Rio-rota = nasce em país produtor (BO/CO/PE) **e** é tributário do Amazonas (segue até Manaus,
   ponto final das rotas do oeste). Os demais são "off-route".
3. Resultado: **16 rios** — Abunã, Acre, Amazonas, Caquetá, Envira, Içá, Japurá, Javari, Juruá,
   Madeira, Mamoré, Negro, Purus, Tarauacá, Uaupés, Xiê. (Reaproveitar a lista pronta.)
4. Interseção espacial (QGIS ou geopandas) entre os 16 rios e a **malha municipal do IBGE** →
   define municípios tratados e a que país(es) de origem se associam.

Por que "tributário do Amazonas": garante um corredor fluvial contínuo da origem (Andes) ao
escoamento (Manaus). Rios que não desaguam no Amazonas correm por regiões com boa estrada, onde o
deslocamento do tráfico para o rio é improvável.

## 4. Variável de exposição
`CocaExposure_it = ln(Σ_r D_ir · CocaProd_rt)`, D = dummy de município na rota r, CocaProd =
produção no país r no ano t. Fora de rota → 0. Produção: INCSR (nacional) + UNODC/SIMCI (provincial).

## 5. Aptidão geológica — o ponto delicado
- **Não existe mapa pronto de "aptidão para garimpo".** Constrói-se um.
- Caminho empírico (favorabilidade calibrada em dado real): MapBiomas como resposta → cruzar com
  geologia/geoquímica → adicionar acesso (drenagem/estrada/pista) → sobrepor institucional
  (SIGMINE/áreas protegidas). Isso é um **modelo de prospectividade mineral**.
- **DECISÃO CRÍTICA (identificação):** a aptidão que multiplica o preço do ouro na regressão causal
  deve ser **SÓ geologia/geoquímica** (exógena, fixa). NÃO embutir acesso nem garimpo observado:
  - Circularidade: aptidão calibrada no garimpo observado e usada para explicar o garimpo observado.
  - Acesso ≈ rota (ambos seguem rio). Pôr acesso na aptidão = enfiar parte do tratamento no controle,
    atenuando/apagando o efeito da rota.
- **Uso do modelo completo (com acesso):** ferramenta DESCRITIVA e fonte de um resíduo interpretável
  — a diferença entre onde o garimpo *deveria* estar e onde ele *está* é informação sobre
  fiscalização e acesso, não sobre geologia. Calibrar fora-da-amostra para evitar circularidade.
- Fontes: GeoSGB/CPRM (geologia, geoquímica de sedimento de corrente, aerogeofísica, ocorrências de
  ouro no GEOBANK); mapa de prospectividade amazônico de Silva & Costa (2011); USGS MRDS (global).
- Cautela: "ocorrências conhecidas" são endógenas à exploração; preferir litologia/geoquímica.

## 6. Recorte geográfico — QUESTÃO EM ABERTO
O *Landing on Water* define **Amazônia Ocidental** = AC, AM, RO, RR, MT (perto dos Andes) e usa a
**Amazônia Oriental** (Amapá, Maranhão, Pará, Tocantins) só como contraste descritivo. Para *garimpo*
isso é um problema: **o Pará (Tapajós) é o maior polo de garimpo do Brasil e fica de fora**.
Decidir com o orientador:
- Amazônia Ocidental → identificação limpa (rotas andinas), mas pouco garimpo e validade externa menor.
- Amazônia Legal (inclui Pará/Amapá) → captura o garimpo, mas exige repensar a definição das rotas
  (nem todo rio do Pará se conecta à origem andina).

## 7. Municípios novos (anos 1990)
MapBiomas por satélite mantém a fronteira fixa. Mas controles municipais ao longo do tempo exigem
lidar com a criação de municípios → usar **Áreas Mínimas Comparáveis (AMC)** de Ehrl (2017).

## 8. Unidade de análise
Começar **município-ano** (dados prontos, alinhado ao paper). Migrar para **grid cells** depois
(mais resolução; à la "This Mine is Mine!" e "Green for Gold", grid 0,5°).

## 9. Ferramentas
Google Earth Engine (MapBiomas/PRODES/satélite na nuvem) · QGIS (vetores) ·
R + `fixest`/`sf`/`terra`/`geobr` (estimação) · alt. Python (geopandas/rasterio/pyfixest) ·
prospectividade com random forest/logística. Git/GitHub para reproduzir.

## 10. Dados e literatura (planilhas)
- `docs/bases_dados_garimpo_trafico.xlsx` — bases por categoria (garimpo, rotas, aptidão, etc.).
- `docs/revisao_literatura_garimpo_trafico.xlsx` — literatura curada por tema.
- `docs/novos_papers_sugeridos.xlsx` — sugestões adicionais (shift-share, Dube & Vargas, Parfitt 2024…).

Papers-âncora: Pereira, Pucci & Soares (2024, Landing on Water); Girard, Molina-Millán & Vic (2025,
Green for Gold); Idrobo, Mejía & Tribin (2014); Berman et al. (2017); Chimeli & Soares (2017, mogno);
McSweeney/Sesnie/Tellman/Devine (narco-deforestation); Dix-Carneiro, Soares & Ulyssea (2018).

## 11. Próximos passos
1. Montar o tratamento (16 rios × malha IBGE) num recorte pequeno (um estado, poucos anos).
2. Validar a extração do MapBiomas no GEE (garimpo por município-ano).
3. Construir a camada de aptidão só-geologia a partir do GeoSGB.
4. Primeira estimação descritiva antes de escalar.

## 12. Questões em aberto para o orientador
- Recorte: Amazônia Ocidental vs. Amazônia Legal (Pará).
- Eixo temporal principal: preço do ouro (nº 3) vs. Lei do Abate/cocaína (nº 2).
- Cobertura da geoquímica do GeoSGB na área de estudo (é irregular).
- Disponibilidade do raster de prospectividade de Silva & Costa (2011).
