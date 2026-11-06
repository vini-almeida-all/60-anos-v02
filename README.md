# Leitura 60 Anos — Protótipo único v0.22 / Galeria Mobile

**Atualização responsiva aprovada em 6/11/2026:** a fotografia Mobile da Galeria usa **largura Fill Container e altura calculada por `aspect-ratio: 326 / 245`**. A proporção do frame Figma a 390px foi preservada, mas a altura NÃO fica fixada em 245px em outras larguras. Esta decisão mais recente prevalece sobre o `layoutSizingVertical=FIXED` existente no frame `3744:3194`; harmonizar a annotation antes do handoff final.

O HTML usa uma única `index.html` com CSS e JavaScript internos, e todos os SVGs e sete JPEGs locais na pasta `assets/`. Desktop e Tablet aprovados permanecem intactos; o Mobile tem **três coleções empilhadas sem abas**, cada uma com galeria, contador, paginação 100×32px, setas e legenda independentes. Os datasets demonstrativos continuam com **7, 6 e 5 fotos** nos respectivos assuntos. Os arquivos de imagem originais não foram recompactados nem renomeados nesta revisão.

## Comportamento Mobile

- Conteúdo com margem lateral de 32px, título principal Galeria, três assuntos separados por gap de 48px. Cada assunto tem heading de 20px, gap de 16px até o carrossel, controles centralizados 24px abaixo da foto e padding inferior de segurança de 120px na seção.
- Foto: largura `100%` do container (326px a 390px de viewport), altura `auto`, proporção `326 / 245`, `object-fit: cover`, raio de 24px. Dimensões verificadas: **256×192,4px** a 320px; **311×233,7px** a 375px; **326×245px** a 390px; **366×275,1px** a 430px; **550×413,3px** a 614px.
- Contador original GalleryImageCounter no canto superior direito a 12px. Legenda com gradiente original abre/fecha por toque curto quando houver conteúdo; toque fora fecha e swipe troca slide sem abrir legenda. Em Desktop, hover continua funcionando. Arrasto com mouse não seleciona imagem/texto.
- Cada assunto tem contador, setas, swipe e paginação **independentes**. A página não tem overflow horizontal; bolinha azul ocupa o primeiro slot no primeiro slide, o centro nos intermediários e o último no final. O menu horizontal e as listras inferiores permanecem **instâncias globais**, não duplicadas entre assuntos.

## QA e restrição de legenda em 320px

Chromium: QA da Galeria Mobile em **320, 375, 390 (alturas 844 e 550), 430 e 614px**; regressão em **615, 768, 1023, 1024 e 1440px**. Testados sem overflow horizontal; labels, controles e imagens carregados; contadores independentes (1/7, 1/6, 1/5); próxima/anterior; índice final (5/5 com terceiro indicador azul); toque abre/fecha legenda; swipe avança um slide sem abrir legenda. A tela 390px mantém a seção Galeria de 1362px como no Figma; demais alturas variam de acordo com a largura proporcional das fotos.

**Atenção para futuro QA de conteúdo:** em larguras estreitas como 320px, a legenda fictícia longa da amostra **ultrapassa a área visual da foto** com tipografia de 16px e padding original de 36px. Esta entrega mantém o contrato visual `GalleryImage` e NÃO altera estilos nem acrescenta scroll, botões ou truncamento arbitrário. Antes do handoff final, é preciso validar com o designer como a legenda deve se comportar com textos longos nessas larguras — ou estabelecer limite editorial apropriado.

**Assets:** todas as sete fotografias da Galeria são JPEGs locais do `Assets.zip`. Hero e Nossa História ainda referenciam imagens externas temporárias do Figma, fora do escopo desta revisão. Fonte Nunito é carregada por recurso externo.

---

## Histórico das versões anteriores

# Leitura 60 anos — Galeria v0.20 com fotografias reais para teste

A partir do **protótipo v0.19** aprovado, somente os **assets de conteúdo da Galeria e o dataset demonstrativo** foram atualizados. Nada mudou no CSS, na estrutura de componentes, na paginação, nos gestos, no Hero ou em Nossa História. Todas as sete fotografias vêm do `Assets.zip` enviado pelo usuário, preservadas **byte a byte** em `assets/gallery/` (JPEG original; sem recortes nem recompactação).

## Como as imagens foram distribuídas

O ZIP não traz categorias nem legendas editoriais. Para testar a paginação dinâmica com quantidades diferentes e imagens consecutivas visualmente distintas, as abas **Assunto 01/02/03 permanecem placeholders**:

- **Assunto 01 — 7 slides:** Maceió 7423, Maceió 7446, Loja Nova 28, Loja Nova 71, Loja Nova 75, REN 3480, REN 3503.
- **Assunto 02 — 6 slides:** Loja Nova 28, Loja Nova 71, Loja Nova 75, REN 3480, REN 3503, Maceió 7423.
- **Assunto 03 — 5 slides:** REN 3480, REN 3503, Maceió 7423, Maceió 7446, Loja Nova 28.

Esta distribuição **não atribui assuntos reais às fotografias**. Se houver uma ordem e associação editorial específica no Figma, basta substituir a matriz `galleryOrders` dentro do último script de `index.html`. A imagem ocupa o componente `GalleryImage` original (960×450px, raio 36px, crop `object-fit: cover`); seu arquivo original não foi modificado. A legenda demonstrativa e os estados `caption=yes/no` foram mantidos da v0.19; não foram criadas legendas descritivas nem conteúdo oficial.

## Correspondência dos nomes originais

- `maceio-7423.jpg` ← `Assets/LEITURA Maceió IMG_7423 foto Flávia Correia.jpg` (SHA-256 `419722fd5c2c…`)
- `maceio-7446.jpg` ← `Assets/LEITURA Maceió IMG_7446 foto Flávia Correia.jpg` (SHA-256 `2e5afcb9b5cb…`)
- `loja-nova-28.jpg` ← `Assets/loja nova-28.jpg` (SHA-256 `b2dfa9dbcdf6…`)
- `loja-nova-71.jpg` ← `Assets/loja nova-71.jpg` (SHA-256 `38a886616adf…`)
- `loja-nova-75.jpg` ← `Assets/loja nova-75.jpg` (SHA-256 `020032297dc9…`)
- `ren-3480.jpg` ← `Assets/REN_3480.jpg` (SHA-256 `0737161d5ec3…`)
- `ren-3503.jpg` ← `Assets/REN_3503.jpg` (SHA-256 `1a7c41201234…`)

## QA e pendências

**QA executado em Chromium** (1024px, 1440px e 1920px, regressões a 768px e 390px): os sete JPEGs locais carregaram com `naturalWidth` válido; cada slide exibiu uma origem distinta dentro de seu assunto; contadores e paginação passaram de 1/N a N/N nos três assuntos (7/6/5 imagens), com reset ao mudar de aba e sem rolagem horizontal. O arrasto com mouse mudou da segunda para a terceira fotografia sem seleção nativa de texto. As regras de legenda, toque e teclado são as mesmas da v0.19, não modificadas nesta versão. Na Galeria, a dependência antiga de foto externa do Figma **foi eliminada**: todos os sete JPEGs agora são locais. **As fotos do Hero e de Nossa História continuam dependendo do asset Figma temporário**, por não fazerem parte deste ajuste. Nunito ainda depende de carregamento de fonte externa. Na implementação final, os assuntos e legendas devem ser mapeados ao conteúdo editorial definitivo.

---

## Documentação anterior — base v0.19

# Leitura 60 Anos — Protótipo único v0.18

## Revisão v0.18 — Contiguidade das seções e paginação nas extremidades

- **Gap entre seções 02 e 03:** havia os caracteres literais `\n` após `</section>` da Nossa História, fora de qualquer tag ou comentário. O navegador criava um line box de ~18px sobre o fundo branco entre as duas seções. Esse texto espúrio foi removido. Não foram modificados os paddings, as alturas ou o background de nenhuma seção.
- **Paginações extremas:** na **primeira E na última foto** de cada assunto, a bolinha azul de 12px ocupa o primeiro slot (x=26); em fotos intermediárias, ocupa o slot central (x=50). As outras duas bolinhas permanecem cinza de 8px. Mantidas a dimensão 100×32px, as posições 26/50/74, as transições de 240ms, a animação dinâmica intermediária e a janela de três marcadores, sem adicionar novos indicadores.
- **Estados:** setas, swipe, drag, teclado e troca de assunto atualizam simultaneamente índice, contador, estado das setas e paginação. Ao trocar assunto, retorna para a primeira posição azul; respeita `prefers-reduced-motion` e não ultrapassa os limites do dataset.
- **Figma:** trata-se de uma correção de comportamento explicitamente solicitada para a GalleryPagination original. O componente gráfico, os assets e as demais seções não foram modificados. A Galeria segue Desktop apenas nesta etapa.

---

**Etapa atual:** Galeria Desktop (Seção 03), revisão de paginação e gestos. Continuidade do mesmo `index.html` da v0.15. As Seções 01 e 02 preservam os breakpoints Desktop, Tablet e Mobile aprovados. A Galeria tem apenas Desktop nesta entrega; Tablet e Mobile serão implementados após validação.

## Revisão v0.16 — Paginação dinâmica e legenda/drag

Referência comportamental: https://swiperjs.com/demos/040-pagination-dynamic/core . Mantivemos **a aparência 1:1 do GalleryPagination do Figma** (100×32px, fundo #D9D9D9 e 3 dots de 8/12/8px nas posições 26/50/74; azul ativo #0033A0, cinza inativo #999999). A geometria do SVG original foi reutilizada no HTML para possibilitar a animação dos círculos, e o arquivo `assets/gallery-pagination-original.svg` foi preservado integralmente para o handoff. A janela representa **anterior / atual / próximo** e desloca seus indicadores em 24px ao avançar/retroceder, usando transição de 240ms e respeitando `prefers-reduced-motion`; na v0.16 o indicador ativo permanecia no centro inclusive nas extremidades (corrigido na v0.18). O estado real é indexado em `data-index`, o total em `data-total` e o contador acessível mostra imagem atual/total. Não há uma bolinha por slide, e não se navega além dos limites.

O conjunto de slides é **demonstrativo** para o QA: `Assunto 01` tem 7 imagens, `Assunto 02` tem 6, `Assunto 03` tem 5, todas reutilizando **a única fotografia e a legenda de placeholder já previstas no Figma**. Não foram criadas fotos, textos ou categorias oficiais. A mudança de aba sempre reinicia em 1/N, atualiza setas/contador/dots e fecha a legenda.

**Legenda e gestos:** no Desktop, hover ou foco de teclado continuam revelando o overlay do `GalleryImage` com gradiente e texto já aprovados. Ao mover horizontalmente ≥8px com o botão pressionado, o gesto assume prioridade e oculta a legenda; ao deslocar ≥45px, a soltura navega exatamente um slide. O overlay usa `pointer-events:none`, mantendo o drag no próprio container da imagem. Depois de trocar de slide, a legenda permanece fechada até o próximo movimento de mouse ou novo foco. No toque, um toque curto abre/fecha legenda quando o slide tem texto; swipe horizontal navega uma imagem e **nunca** ativa a legenda do mesmo gesto. Um toque fora fecha a legenda. Escape fecha; setas de teclado sobre a imagem também navegam. Rolagem vertical natural é preservada via `touch-action:pan-y`. O DOM não adiciona controles visuais.

**Escopo:** Galeria segue apenas no breakpoint Desktop (`>=1024px`) nesta entrega; a lógica de toque foi testada em emulação touch na largura Desktop como preparação para Tablet/Mobile, **sem acrescentar layout de Galeria nesses breakpoints**. Hero e Nossa História mantidos sem modificações.

### QA v0.16

Chromium testado a 1920×900, 1440×900, 1024×768, mais regressões 1023×800, 768×892 e 390×844. Foram conferidas a altura original de 786px, o pager de 100×32px, ausência de overflow horizontal, 3 dots em repouso / 4 durante a transição, contadores 1/7→7/7, limites das setas, três abas com datasets de 7/6/5 e reset do índice, hover/drag/foco no Desktop, gesto curto sem mudar slides, touch tap alternando legenda e swipe navegando uma imagem sem exibir legenda. Também foram testados toque rápido após swipe e `prefers-reduced-motion`. A fonte e a foto via URLs externas não carregam no navegador de QA; o enquadramento fotográfico real permanece pendente até obter o asset original local.

---

## Fonte de verdade no Figma

Arquivo `OtFihfNvBMeg5ITRk3ry2t`. Galeria Default `3639:151`, Hover com legenda `3639:243`. Componentes aprovados: `GalleryImage` (`3730:2184`), `CarouselButton` (`2001:3531`), `GalleryPagination` (`3731:2186`), `GalleryImageCounter` (`3810:2309`) e `GallerySubjectTabs` (`3804:3413`). O menu flutuante e a faixa de cinco listras são globais: **uma única instância de cada**, sem duplicação em outras seções. O menu assume o rail `Light Blue/500` (`#3BA5F2`) na Galeria.

## Desktop — geometria e comportamentos (base v0.15)

Fundo `Brand/Primary #0033A0`, padding vertical 64px e altura 786px no frame 1920px. Container max-width 1368px com padding lateral 32px. Título Nunito ExtraBold 28px e sublinhado original multicolorido com altura de 4px. Abas “Assunto 01–03” de 144×44px, gap 16px; painel central de foto 960×450px com raio 36px; contador no canto superior direito a 20px das bordas, com altura 32px. Controles: gap vertical 24px; grupo de 212×32px, com setas CarouselButton de 36×32px, GalleryPagination de 100×32px e 20px de espaço entre elementos. Os quatro SVGs das setas (previous/next default/hover) e o SVG da paginação foram exportados da geometria vetorial original do componente Figma e estão em `assets/`, sem dependência de biblioteca de ícones.

O `GalleryImage` exibe gradiente preto 10%→80% e legenda original em Nunito Bold Italic 16px quando existe legenda e a imagem recebe hover ou foco de teclado; não exibe nada disso quando a imagem não possui legenda. A transição dura 220ms (sem animação em prefers-reduced-motion). Ao trocar slide, a legenda fecha; ao trocar aba, o índice é redefinido para o primeiro slide. `GalleryPagination` mantém três bolinhas originais (8/12/8) como janela compacta do estado; não se adicionam bolinhas por quantidade de imagens. O contador mostra o número real de slides, sem zeros à esquerda. O protótipo também responde a arrasto/swipe horizontal sobre a foto, avançando um slide por gesto, sem criar novos controles.

## Conteúdo demonstrativo — base v0.15 (substituído pelos dados de teste v0.16 acima)

“Assunto 01”, “Assunto 02” e “Assunto 03” são **placeholders aprovados no Figma**, não categorias oficiais. Para testar seleção independente, contadores e legendas, cada assunto oferece três slides demonstrativos com a **mesma imagem de referência do Figma** e, quando aplicável, **a mesma legenda de exemplo**. Nenhuma fotografia, legenda ou categoria nova foi inventada. O contador do protótipo mostra `1/3` em vez de `1/20`, porque `20` é um exemplo no Figma e a quantidade do conteúdo real ainda não foi definida.

A Galeria usa o mesmo IMAGE fill do Hero (hash Figma idêntico: `7fc95402c01635bbd14da48a8b028877ef830152`), aqui com link temporário atualizado `https://www.figma.com/api/mcp/asset/ee3c89ff-348d-467b-bc8b-981e4057f20a/880f8.png`. **Limitação:** a fotografia original não está empacotada localmente no ZIP porque o ambiente não acessa o host do Figma; também a fonte Nunito depende de Google Fonts. É preciso exportar os assets originais antes do handoff definitivo offline. Nenhuma fotografia foi substituída por screenshot/arte fictícia, e os assets antigos não foram modificados.

## QA da v0.15

Chromium: testes de geometria e overflow em 1920×900, 1440×900, 1024×768, com seção de 786px, imagem 960×450px, abas 464×44px e controles 212×32px. Em 1023px, 768px e 390px, Galeria permanece oculta até implementação dos breakpoints Tablet/Mobile. Validados: troca de aba e reset, teclado, setas e limites, atualização do contador, gradiente apenas para caption=yes, drag de uma imagem e mudança da variante escura do menu global. Todas as referências SVG locais existem e o JS passou em `node --check`. A imagem externa e Google Fonts não puderam ser inspecionados visualmente na automação local. A validação fotográfica 1:1 permanece pendente até carregar os assets reais.

---

## Histórico de revisões anteriores

# Leitura 60 anos — v0.12 (ajustes Mobile)

- **Hero:** altura automática com `min-height:0`, terminando imediatamente na fotografia, sem vazio criado por `100dvh`.
- **Timeline:** configuração `cardsPerView=1` explícita: uma carta cheia na área visível, largura `100vw - 64px`, outros cards ocultos fora da viewport; arrasto livre sem snap e setas com snap por marco.
- **Escopo:** Desktop, Tablet, HeroSectionNavigation, SVGs, menu flutuante e outras medidas mantidos.
- **Recursos externos:** a fotografia e Nunito continuam dependendo de URLs externas, como na v0.11; não foram substituídos.

---

# Leitura 60 Anos — Protótipo único v0.11

## Escopo

O mesmo `index.html` contém **Seção 01 (Hero) e Seção 02 (Nossa História / Linha do Tempo)** com CSS e JavaScript internos, sem framework. A versão 0.11 acrescenta **apenas o breakpoint Mobile da Seção 02**. Foram mantidos os arquivos SVG originais já existentes na pasta `assets/` e as regras aprovadas de Desktop e Tablet.

**Fonte de verdade — Figma:** arquivo `OtFihfNvBMeg5ITRk3ry2t`, frame de Nossa História Mobile fechado `3799:3413`, aberto `3931:7150`, narrativa `3922:7104`, componente de navegação `FloatingSectionNavigation` (`3614:1700`). Versões anteriores: Hero Desktop `3559:1712`, Tablet `3575:2061`, Mobile `3559:1758`; História Desktop `3788:3413`, Tablet `3798:3413`.

## Mobile — Nossa História (até 614px)

- Fundo `surface/subtle`, padding vertical `48px`, margem lateral `32px` e distância de `32px` entre título, painel narrativo e timeline.
- Título **Nossa história** em Nunito ExtraBold `24px` e faixa decorativa local de cinco cores e altura `4px`.
- Painel narrativo **326 × 560px** na referência de `390px`: largura fluida `(100vw - 64px)`, raio de `32px`, borda de `1px`. O scroll é **interno**, com a scrollbar recuada para não ultrapassar os cantos. Todo o conteúdo editorial que já existe nas outras versões permanece acessível, sem truncamento.
- Conteúdo do painel: padding lateral de `24px` em relação à borda de referência, título Nunito Black `32px`; corpo inicial `16px/125%`, fotografia original com proporção `560/320`, legenda de `16px`, subtítulo final `21px`, com alturas Hug.
- Timeline com eixo azul de `2px`, **um marco visível por vez**, ano/nó/conector alinhados ao centro do viewport, nós em SVG preservados. Card de `326px` (fluido) com padding `20px`, imagem `284×208px` na referência, raio de imagem de `32px`, texto de `16px/135%`, altura por conteúdo.
- **Arrasto livre** de cards, sem `scroll-snap`. **Apenas as setas** alinham automaticamente ao marco anterior/seguinte; o rótulo do ano acompanha o card mais próximo durante o arrasto. As datas adicionais além de 1983 e 1986 continuam **placeholders de teste**, não fatos históricos.
- Menu flutuante **global** (uma única instância no HTML) escondido no Hero, visível a partir da História. Mobile: `180×40px` recolhido, fixo ao centro horizontal `32px` acima da base; `250×191px` expandido. O rail recolhido mantém fundo azul a `50%`, `backdrop-filter: blur(5px)` e sombra `0 0 4px rgb(0 0 0 / 10%)`. Ao expandir: painel branco `250×135px`, gap `16px` e rail `180×40px`, ambos com sombra `0 0 10px rgb(0 0 0 / 35%)` (Figma atualizado). Os cinco traços são os SVGs originais. Em toque, a barra pode ser aberta antes de navegar; teclado/Escape também funcionam.
- A faixa decorativa global inferior é única e fixa à viewport, com `6px` no Mobile, não duplicada dentro da Seção 02.

**Não foi adicionado um seletor novo de décadas:** embora algumas annotations citem um dropdown, o frame Mobile atual mostra somente a timeline e o menu flutuante. Não inventar UI que não esteja desenhada. Quando houver conteúdo real de outras décadas, conectar os dados à navegação conforme as annotations, em vez de criar fatos fictícios.

## Desktop / Tablet preservados

A v0.11 acrescenta regras apenas no bloco `@media (max-width: 614px)` para a Seção 02 e uma camada de marcador mobile que permanece oculta nas outras larguras. A Seção 01 aprovada e a Seção 02 em Desktop/Tablet mantêm seus CSS e estruturas anteriores. A lógica de navegação global passa a considerar Mobile (onde a Seção 02 agora existe), sem duplicar o menu.

## Como testar

Abra `index.html` num navegador. Use **390×844px** como referência Mobile e teste 320px, 375px, 430px e 614px; confira também a transição para Tablet em 615px. Role além do Hero, leia o painel até o fim usando o scroll interno, arraste a timeline e clique nas setas. O menu flutuante deve surgir a partir de Nossa História e abrir no hover/foco ou toque.

**QA executado:** Chromium headless com 320, 375, 390 (incluindo viewport de altura 550), 430, 614, 615, 768, 900, 1023, 1024 e 1440px. Conferidos: ausência de overflow horizontal, painel de 560px, scroll completo, recuo da barra, eixo/card centralizados, snap de `350px` na seta de 390px, arrasto sem snap em posição intermediária, menu 180×40 → 250×191 com sombras de `35%`, e regressão de medidas Desktop/Tablet. Também passou a validação sintática do JavaScript. A comparação de tipografia e fotografias não é completa no navegador de teste, que bloqueia recursos externos.

## Pendência de asset original

A fotografia da loja **ainda está apontada para uma URL temporária antiga do Figma**, herdada do protótipo de origem; o ambiente atual não consegue resolver o host para baixá-la (`curl: Could not resolve host`). Não foi feita substituição por outra fotografia nem por um screenshot. Para handoff final offline, exportar a foto original em arquivo local e trocar as referências no `index.html`. Os SVGs existentes, incluindo logo, nós, setas e riscos, estão preservados localmente no pacote e não foram rasterizados. A fonte Nunito é referenciada via Google Fonts e requer carregamento externo, mas **arquivos de fonte não estão incluídos**.


## v0.13 — Correção da altura da Seção 01 no Tablet

- Em `615px–1023px`, a Seção 01 passa a ter `height: auto; min-height: 0;` em vez de `min-height: 100vh/100dvh - header - listras`. Assim a base do Hero coincide com a base real da fotografia, mesmo em viewports altos.
- Mantidos os gaps, paddings, proporção 768/560 e bordas de 32px originais do Tablet. A seção Nossa História começa imediatamente após a foto, sem espaço em branco gerado pelo Hero.
- A mudança é limitada ao CSS do Hero Tablet; todos os outros componentes e breakpoints foram preservados, incluindo o Mobile corrigido na v0.12.


## v0.14 — Correção da narrativa Mobile e fundo das setas (06/11/2026)

- Removido **apenas** o override de `18px` em `.history__figure + .history__prose` no Mobile. Todos os parágrafos do painel narrativo Mobile usam `16px` (Nunito SemiBold, line-height `125%`, letter-spacing `-0.304px`), conforme Figma `3799:3413` / texto `3922:7111`. A tipografia original de `18px` do Desktop e Tablet **permanece** conforme seus respectivos frames.
- Adicionado o token CSS `--surface-brand-soft: #f5fbfe` correspondente à variável Figma **Surface/Brand Soft** (`VariableID:2086:1347`) e aplicado como `background` em `.history__timeline-arrow`, em **Desktop, Tablet e Mobile**. As caixas de previous/next continuam com `24 × 24px`, sem mudança em padding, raio, posição, SVG nem interações (drag livre; snap apenas nas setas).
- Nenhuma outra seção, breakpoint ou componente foi alterado.


## v0.17 — Evitar seleção de conteúdo durante o arrasto da Galeria

Alteração exclusivamente na interação `.gallery__media` da seção Galeria. O contêiner da imagem recebeu `user-select: none`, `-webkit-user-select: none` e `-webkit-touch-callout: none` para impedir seleção nativa da imagem, legenda e contador. O início do gesto com o **mouse** cancela o comportamento padrão (`pointerdown.preventDefault()`), mas eventos touch não são cancelados, preservando scroll vertical e swipe. O elemento `img` já mantinha `draggable="false"` e `-webkit-user-drag: none`, preservados. Não há mudança de aparência, tamanhos, assets, paginação, hover, foco ou comportamento dos componentes existentes.

**QA:** Chromium 1920×1080, 1440×900 e 1024×820: arrasto de mouse avança slides sem texto selecionado, sem overflow; setas e teclado continuam funcionando. Em sessão touch, toque curto exibe legenda, swipe horizontal avança imagem e fecha legenda. Fotografia externa Figma continua sujeita à limitação registrada anteriormente.


## Revisão v0.19 — Última bolinha da paginação

Corrigida apenas a lógica de posição do indicador ativo do `GalleryPagination`: no primeiro slide a bolinha azul maior fica no indicador da esquerda (`x=26`), nos slides intermediários fica no centro (`x=50`) e **no último slide fica no indicador da direita (`x=74`)**. Os três indicadores continuam no componente original de 100×32px, com raios 6/4/4 conforme o ativo, mantendo transições, arrasto, setas e alternância de assuntos da v0.18. Não foram alterados os frames Desktop/Tablet/Mobile nem o conteúdo de outras seções.
