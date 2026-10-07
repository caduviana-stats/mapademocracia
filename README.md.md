# Mapa do Índice de Democracia

Mapa mundial interativo dos 57 países da planilha fornecida, com o Índice de Democracia, espaço para o Índice de Autonomia Ministerial (IAM) e designação do Ministério Público. O projeto utiliza Leaflet e pode ser publicado diretamente no GitHub Pages.

## Estrutura do repositório

```text
index.html
readme.md
dados/
  paises.csv
  indices_democracia_capitais.xlsx
  mundo.geojson
```

## Interação

- Os países da amostra são coloridos pelo índice; os demais ficam em cinza.
- Passar o cursor sobre um território destaca sua fronteira e mostra o nome e o índice.
- Clicar no território ou no ponto da capital abre uma caixa com os indicadores e atualiza o painel lateral.
- Os pontos mostram o nome do país ao passar o cursor; a partir do zoom 4, os nomes ficam visíveis. A busca e a lista também permitem localizar países pequenos.
- A pesquisa aceita nomes sem acentos. Com um único resultado, Enter seleciona o país.
- O botão “Visão mundial” retorna ao enquadramento inicial.

## Base de dados e atualização

**A fonte efetivamente lida pelo mapa é `dados/paises.csv`.** O XLSX é uma cópia editável para trabalho em planilha. Não existe sincronização automática entre os dois arquivos.

| Coluna no CSV | Conteúdo |
| --- | --- |
| `pais` | Nome do país em português |
| `indice_democracia` | Valor numérico da planilha, na escala de 0 a 10 |
| `designacao_mp` | Nome institucional conforme informado na base |
| `latitude` / `longitude` | Coordenadas da capital, em graus decimais WGS84 |
| `capital` | Capital usada como ponto de referência |
| `iso3` | Identificador de três letras para associação ao polígono |
| `iam` | Campo numérico reservado, inicialmente vazio |

Para atualizar, edite o CSV mantendo esses cabeçalhos, a codificação UTF-8 e o separador vírgula. Use ponto decimal e mantenha textos com vírgulas entre aspas. Também é possível editar o XLSX e exportar a aba “Países” para CSV: nesse caso, substitua os cabeçalhos pelos nomes técnicos da tabela acima e confira o separador e os números antes de substituir `paises.csv`.

Preencha `iam` e `designacao_mp` conforme a pesquisa avançar. **Um IAM vazio é ausência de informação, não zero.** O mapa aceita novos valores sem alteração no HTML. A escala do IAM não foi definida nesta etapa e não é presumida pelo código.

Os 57 índices e as quatro designações existentes foram preservados integralmente. Designações vazias aparecem como “A preencher”. Não foi incorporado o exemplo genérico “Public Prosecutor Office” para os Estados Unidos como nome oficial. A determinação da instituição equivalente deve acompanhar a pesquisa comparativa e sua fonte.

A planilha não contém os componentes do Índice de Democracia nem um campo de ano; portanto, eles não foram inventados. As faixas da legenda são intervalos numéricos de visualização, não uma classificação de regimes políticos. Para introduzir novas variáveis, adicione a coluna ao CSV e inclua sua exibição nas funções `popup()` e `showDetails()`.

## Coordenadas e limites

Coordenadas de cidades e limites nacionais: [Natural Earth](https://www.naturalearthdata.com/), distribuição [natural-earth-vector](https://github.com/nvkelso/natural-earth-vector). Arquivos utilizados: `ne_10m_populated_places_simple.geojson` e `ne_50m_admin_0_countries.geojson`, consultados em 07/10/2026. Os limites foram incluídos no projeto com apenas os identificadores necessários. Fonte em domínio público.

As coordenadas representam um ponto urbano de referência da capital, não um edifício específico. Nomes de capitais seguem a fonte cartográfica. Em casos com mais de uma sede, adotaram-se Pretoria para a África do Sul, Amsterdam para os Países Baixos e Kuala Lumpur para a Malásia. Bern é a cidade federal utilizada para a Suíça. Jerusalem segue a referência cartográfica da fonte para Israel; essa escolha não representa uma posição sobre seu status internacional. Taiwan mantém um registro próprio como na planilha. Fronteiras seguem a representação cartográfica do Natural Earth.

## Executar e publicar

Para testar localmente, na pasta do projeto:

```bash
python -m http.server 8000
```

Acesse `http://localhost:8000`. Abrir `index.html` por duplo clique pode impedir a leitura do CSV e do GeoJSON devido às regras do navegador.

No GitHub, coloque `index.html`, `readme.md` e a pasta `dados` na raiz do repositório. Em **Settings → Pages**, selecione **Deploy from a branch**, a branch `main` e a pasta `/ (root)`. Salve e aguarde a publicação.

O mapa requer conexão para carregar Leaflet 1.9.4, Papa Parse 5.4.1 e o fundo CARTO/OSM. Não há exigência de senha nem de chave de API. Os indicadores e os limites nacionais são arquivos locais do próprio repositório.

## Organização do código

As nove seções do `index.html` estão identificadas por comentários com títulos Markdown. Configurações e URLs ficam na seção 4; validação do CSV na 5; conteúdo das caixas na 6; camadas na 7; busca na 8; inicialização na 9. Valores vindos da base são escapados antes de entrar no HTML. Erros de carregamento, duplicação de códigos e coordenadas inválidas aparecem no painel.

Ao adicionar países, confirme que `iso3` existe em `mundo.geojson`. Territórios não presentes nessa base exigem atualizar a geometria. Após alterações, confira a contagem, o posicionamento da capital, o destaque do território e a caixa de detalhes.
