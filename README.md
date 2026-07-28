# 📊 Painel.Ads — Dashboard Executivo para Meta Ads

Dashboard executivo standalone para análise de campanhas do **Meta Ads Manager**, construído em **HTML + CSS + JavaScript puro**, sem backend e sem etapas de build. Basta abrir o arquivo no navegador.

![Status](https://img.shields.io/badge/status-est%C3%A1vel-31D6A0)
![Tipo](https://img.shields.io/badge/tipo-single--file%20HTML-5B7CFA)
![Depend%C3%AAncias](https://img.shields.io/badge/depend%C3%AAncias-100%25%20embutidas-8E7CF6)

---

## ✨ Visão geral

O painel lê um CSV exportado do Meta Ads Manager e transforma os dados em uma interface interativa de BI: KPIs com comparação semanal, gráficos de evolução, ranking de campanhas, comparativos entre semanas, alertas automáticos e um resumo executivo gerado por regras — tudo processado localmente no navegador, sem enviar dados para nenhum servidor.

Vem pré-carregado com um conjunto de dados de demonstração. Para usar com dados reais, é só clicar em **Importar CSV** e selecionar o export do Meta Ads Manager.

## 🖥️ Telas

| Tela | O que mostra |
|---|---|
| **Visão Geral** | KPIs principais (investimento, impressões, alcance, cliques, CTR, CPC, CPM, resultados, CPA, ROAS, frequência) com variação vs. período anterior, resumo executivo, evolução diária, funil de performance, alertas e distribuição de investimento |
| **Campanhas** | Ranking ordenável de campanhas, investimento e resultados por campanha, dispersão CTR × CPC |
| **Conjuntos & Anúncios** | Detalhamento por conjunto de anúncios / anúncio (quando o CSV importado contém essas colunas) |
| **Evolução** | Evolução semanal, evolução do CPA, evolução do ROAS e heatmap de performance por dia da semana |
| **Comparativos** | Comparação lado a lado entre duas semanas quaisquer do período, com participação de cada campanha nos resultados |
| **Insights** | Lista de leituras automáticas geradas a partir dos dados (crescimento, quedas, oportunidades, concentração de investimento etc.) |

## 🔍 Principais funcionalidades

- **Importação de CSV com acúmulo de histórico** — cada CSV importado é **combinado** com os dados já carregados (não os substitui). Se o mesmo dia/campanha aparecer em dois arquivos, os valores mais recentes prevalecem; dias novos são simplesmente adicionados ao histórico. Assim, você pode importar um relatório por semana e o painel vai acumulando o histórico completo ao longo do tempo.
- **Persistência local** — os dados combinados ficam salvos no `localStorage` do navegador. Ao fechar e reabrir o painel (ou recarregar a página), o histórico continua lá, sem precisar reimportar tudo de novo. Os dados nunca saem do seu navegador.
- **Restaurar dados de demonstração** — botão no rodapé do menu lateral para apagar os dados salvos localmente e voltar ao conjunto de exemplo, caso precise começar do zero.
- **Detecção robusta de colunas** — se alguma coluna esperada não existir no CSV (ex.: valor de conversão para ROAS, ou nome de conjunto de anúncios), o painel avisa de forma clara em vez de quebrar, e desabilita apenas a métrica afetada.
- **Filtros dinâmicos** — por semana, campanha, objetivo, status e busca por nome.
- **Insights e alertas automáticos** — regras que identificam CTR baixo, CPA alto, frequência excessiva, CPM elevado, campanhas sem conversão e concentração de investimento sem retorno proporcional.
- **Painel lateral (drill-down)** — clique em qualquer campanha na tabela para ver um detalhamento individual com histórico diário.
- **Exportação** — botões para exportar a tela atual como **PNG** ou como **relatório em PDF**.
- **100% client-side** — todas as bibliotecas (Chart.js, PapaParse, html2canvas, jsPDF) estão embutidas no próprio arquivo HTML. Não há chamadas a CDNs externas nem a servidores — os dados nunca saem do navegador.

## 🚀 Como usar

1. Baixe o arquivo `painel-meta-ads.html` deste repositório.
2. Abra o arquivo em qualquer navegador moderno (Chrome, Edge, Firefox, Safari) — basta dar duplo clique ou arrastar para uma aba do navegador.
3. Clique em **Importar CSV** no topo da tela e selecione o arquivo exportado do Meta Ads Manager.
4. Navegue pelas seções pelo menu lateral e use os filtros para explorar os dados.

> Não é necessário instalar nada, rodar `npm install` ou subir um servidor. É um único arquivo HTML autossuficiente.

### Como exportar o CSV do Meta Ads Manager

No Gerenciador de Anúncios: selecione o período desejado → **Exportar** → **Exportar tabela como .csv**. O painel espera colunas como `Nome da campanha`, `Dia`, `Alcance`, `Impressões`, `Resultados`, `Valor usado (BRL)`, `Cliques no link`, `CTR`, `CPC`, `CPM`, entre outras — os nomes padrão do export do Meta já são reconhecidos automaticamente.

## 🛠️ Stack técnica

| Camada | Tecnologia |
|---|---|
| Estrutura / estilo | HTML5 + CSS3 (design tokens customizados, sem framework CSS) |
| Lógica | JavaScript puro (vanilla), sem frameworks |
| Parsing de CSV | [PapaParse](https://www.papaparse.com/) |
| Gráficos | [Chart.js](https://www.chartjs.org/) |
| Exportação PNG | [html2canvas](https://html2canvas.hertzen.com/) |
| Exportação PDF | [jsPDF](https://github.com/parallax/jsPDF) |
| Tipografia | Space Grotesk (títulos), Inter (texto), JetBrains Mono (dados numéricos) |

Todas as bibliotecas estão **embutidas diretamente no arquivo** (não carregadas via CDN), o que garante que o painel funcione mesmo em ambientes sem acesso à internet ou com políticas de segurança restritivas (ex.: links de artefato publicados, intranets corporativas).

## 📁 Estrutura do arquivo

Por ser um único arquivo HTML, o código está organizado internamente em blocos comentados:

```
painel-meta-ads.html
├── <head>            → bibliotecas embutidas + design tokens (CSS)
├── <body>
│   ├── Sidebar        → navegação entre as 6 telas
│   ├── Topbar          → conta, período, ações (importar / exportar)
│   └── Views            → uma seção <section> por tela
├── CORE                → utilidades, detecção de colunas, normalização de dados
├── INSIGHTS             → geração de insights e alertas automáticos
└── APP                  → estado, filtros, gráficos, renderização, eventos
```

## ⚠️ Limitações conhecidas

- **ROAS** só é calculado se o CSV importado contiver uma coluna de valor de conversão (ex.: `Valor de conversão de compras`) ou de ROAS direto do Meta Ads. Sem essa coluna, o painel indica "Indisponível" em vez de estimar o valor.
- **Conjuntos & Anúncios** só exibe dados se o CSV importado tiver as colunas `Nome do conjunto de anúncios` e/ou `Nome do anúncio` — exports apenas no nível de campanha mostram uma mensagem explicando a limitação.
- **O histórico é salvo por navegador/dispositivo**, não na nuvem. Se você importar os relatórios em um computador e depois abrir o painel em outro (ou em modo anônimo/privado), o histórico salvo não estará lá — só o conjunto de demonstração inicial. Para levar o histórico para outro dispositivo, seria necessário exportar/importar os dados manualmente (não incluído nesta versão).
- Se o navegador estiver em modo privado/anônimo ou com armazenamento local desativado, a importação ainda funciona normalmente durante a sessão, mas os dados não serão salvos para a próxima visita — o painel avisa quando isso acontece.

## 📄 Licença

Uso livre para fins internos, de agência ou pessoais. Adapte como quiser.

---

Feito com Claude ✨
