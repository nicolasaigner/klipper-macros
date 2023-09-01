# Conjunto de Macros para o Firmware de Impressora 3D Klipper

Este é um repositório contendo um conjunto de macros desenvolvidas para o [firmware de impressora 3D Klipper](https://github.com/Klipper3d/klipper). A origem deste repositório é proporcionar um conjunto consistente de macros compartilhadas entre as minhas próprias impressoras 3D. No entanto, como achei essas macros úteis, pensei que outras pessoas também pudessem se beneficiar.

## O que é possível fazer com essas macros?

A maioria dessas macros tem como objetivo melhorar a funcionalidade básica (por exemplo, [superfícies de impressão selecionáveis](#superfície-da-cama)) e a compatibilidade do Klipper com códigos G direcionados para impressoras Marlin. Além disso, há alguns recursos extras interessantes:

- **[Agendar comandos em alturas e mudanças de camada](#gatilhos-de-camada)** - Isso é semelhante ao que um fatiador já é capaz de fazer, mas de maneira mais simples. É possível agendar esses comandos durante uma impressão em andamento. Um exemplo de uso é a adição de um [item de menu LCD](#menus-lcd) para pausar a impressão na próxima mudança de camada. Isso evita que a pausa ocorra dentro de um perímetro externo e danifique a impressão.
- **Escala dinamicamente [aquecedores](#aquecedores) e [ventoinhas](#ventoinhas)** - Isso facilita o ajuste persistente das configurações das ventoinhas durante a impressão em andamento. Também permite manter perfis de fatiamento mais simples, movendo ajustes como o aumento de temperatura para um bico de aço endurecido para o estado armazenado na impressora.
- **Interface de menu LCD mais organizada** - Os menus foram simplificados e uma maneira mais fácil de personalizar materiais no menu LCD foi providenciada. Também foram adicionadas caixas de diálogo de confirmação para comandos que podem interromper uma impressão em andamento.
- **[Nivelamento otimizado da cama de malha](#melhorias-na-malha-da-cama)** - Realiza sondagens apenas dentro da área impressa, economizando tempo em impressões menores.
- **[Linhas de purga automatizadas](#desenhar_linha_de_purga)** - Defina o comprimento de extrusão desejado como `variable_start_purge_length` em sua configuração e um conjunto de linhas de purga será extrudado na frente da área de impressão imediatamente antes do início da impressão.

## Algumas observações importantes...

- **FAÇA UM BACKUP DE SUA CONFIGURAÇÃO COMPLETA ANTES DE REALIZAR QUAISQUER MUDANÇAS!!!** Já vi muitos iniciantes procurando desesperadamente ajuda em fóruns públicos porque não tinham uma boa configuração para retornar após terem comprometido sua configuração atual ao experimentar macros de outras pessoas. Ter uma configuração funcional armazenada vai economizar muito tempo e aborrecimento para você e para os outros.
- **Evite macros personalizadas como essas até estar confortável em usar o Klipper com uma configuração básica.** Macros avançadas do Klipper normalmente dependem muito do [monkey patching](https://en.wikipedia.org/wiki/Monkey_patch), o que pode causar problemas com configurações incomuns ou quando macros de várias fontes são misturadas. Portanto, é recomendado ter conhecimento antes de incluir macros de terceiros, especialmente se elas possuem funcionalidades sobrepostas de diferentes fontes.
- É necessário configurar uma `heater_bed`, `extruder` e outras [seções listadas abaixo](#configuração-do-klipper), caso contrário as macros **forçarão o desligamento da impressora durante a inicialização**. Infelizmente, o sistema de macros do Klipper não possui um método mais suave para lidar com essa situação.
- A funcionalidade de múltiplos extrusores e aquecedores de câmara é pouco testada e pode conter bugs, já que não foi muito utilizada. Contribuições são bem-vindas.
- É possível que existam outras partes não testadas o suficiente, portanto, utilize essas macros por sua própria conta e risco.

## Solução de Problemas

- Verifique se você seguiu as [instruções de instalação](#instalação) e se não há erros sendo exibidos no console ou nos logs.
- Garanta que está utilizando a versão mais recente do Klipper original, e não uma versão modificada, desatualizada ou bifurcada.
- Utilize a versão mais recente dessas macros e evite fazer alterações nos arquivos do diretório `klipper-macros`.
- Reinicie o Klipper após qualquer atualização ou mudança na configuração.
- Execute `CHECK_KM_CONFIG` no console do Klipper e corrija quaisquer erros que forem relatados no console e/ou logs (se nenhum erro de configuração for detectado, nenhum resultado será exibido).
- Execute `_INIT_SURFACES` no console do Klipper para validar que as superfícies da cama estão sendo inicializadas sem erros relatados no console e/ou logs.
- Verifique as configurações do fatiador e revise a saída do G-Code. Preste atenção especial às partes de inicialização do G-Code e aos parâmetros passados para PRINT_START.
- Se encontrar problemas semelhantes, faça perguntas de solução de problemas na [Discussão de Perguntas e Respostas do Github](https://github.com/jschuh/klipper-macros/discussions/categories/q-a).

# Reportando Problemas

Se você seguiu os passos de solução de problemas e não conseguiu resolver o problema, você pode [reportar um bug pelo Github](https://github.com/jschuh/klipper-macros/issues/new/choose). Eu provavelmente vou responder em alguns dias (quase certamente dentro de uma semana). Eu provavelmente não vou responder por outros canais (como Discord, Twitter), pois não os acho úteis para lidar com relatórios de bugs.

Algumas coisas importantes para lembrar ao reportar bugs:

- **Cole o texto completo do comando que gerou o erro, juntamente com quaisquer mensagens de erro exibidas no console**, além das seções relevantes dos logs do Klipper, se apropriado (e por favor, [formate esse texto como código](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#quoting-code), caso contrário o Github vai formatá-lo como um bilhete de resgate).
- **Anexe sua configuração ao relatório de bug.** Geralmente, não é possível diagnosticar qualquer coisa sem as configurações.
- **Verifique se o problema ocorre na instalação atual padrão do Klipper e klipper-macros.** Configurações personalizadas e versões desatualizadas dificultam muito o diagnóstico.
- Por favor, não trate relatórios de bugs como um substituto para seguir as instruções de instalação e solução de problemas.
- Direcione pedidos de recursos para a [Discussão de Ideias do Github](https://github.com/jschuh/klipper-macros/discussions/categories/ideas).

> **Nota:** Relatórios que não seguirem as diretrizes acima _**provavelmente serão fechados sem qualquer outra ação.**_

# Contribuindo

Fico feliz em aceitar PRs de correção de bugs. Também estou potencialmente aberto a receber novos recursos ou adições. No entanto, posso recusar o PR se for algo que não me interesse ou se parecer que seria uma complicação para eu manter.

## Formatação

Não há um estilo padrão para macros do Klipper, então tente seguir o estilo dos arquivos. No entanto, aqui estão algumas regras a serem lembradas:

- Limite as linhas a 80 caracteres, se possível
- Indente 2 espaços e mantenha a mesma linha com o bloco lógico ao quebrar linhas (sem tabulações)
- Prefixe macros internas com `_` ou `_km_`
- Prefixe qualquer tipo de estado global com `_KM_` (por exemplo, `_KM_SAVE_GCODE_STATE`)

## Mensagens de Commit

Essas são as regras para mensagens de commit, mas você também pode simplesmente olhar o log de commits e seguir o padrão observado:

- Use a regra 50/72 para mensagens de commit: No máximo 50 caracteres no título e quebre as linhas na descrição em 72 caracteres.
- Comece o título com o nome do módulo (geralmente o arquivo principal sendo modificado, excluindo qualquer extensão) seguido por dois pontos.
- Mensagens de commit apenas com título são aceitáveis para commits simples, mas certifique-se de incluir uma linha em branco após o título.
- Combine commits múltiplos se o que você está trabalhando fizer mais sentido como um único commit lógico. _Isso pode exigir um push forçado em um PR aberto._

# Instalação

Para instalar as macros, primeiro clone este repositório dentro do diretório `printer_data/config` com o seguinte comando:

```shell
git clone https://github.com/jschuh/klipper-macros.git
```
Depois, cole as seções abaixo no seu arquivo `printer.cfg` para começar. Ou até melhor, cole tudo em um arquivo separado no mesmo caminho da sua configuração e inclua esse arquivo. Isso facilitará caso queira remover essas macros no futuro.

Você talvez precise personalizar algumas configurações para a sua própria configuração. Todas as configurações configuráveis estão em [globals.cfg](globals.cfg#L5) e podem ser sobrescritas criando uma variável correspondente com um novo valor na sua seção `[gcode_macro _km_options]`. **Não modifique diretamente as declarações de variáveis em globals.cfg.** A inicialização das macros assume certos valores padrão, e modificações diretas provavelmente causarão problemas de formas muito inesperadas.

> **Nota:** Os caminhos neste README seguem a [estrutura de pastas de dados do Moonraker](https://moonraker.readthedocs.io/en/latest/installation/#data-folder-structure). Você talvez precise alterá-los se estiver usando uma estrutura diferente.

> **Nota:** Certifique-se de que atualmente não possui macros que ofereçam a mesma função básica das macros neste repositório (por exemplo, as macros padrão do [Mainsail](https://docs.mainsail.xyz/configuration#macros) ou [fluidd](https://docs.fluidd.xyz/configuration/initial_setup#macros)). Como regra, evite usar vários conjuntos de macros que substituam a mesma macro base (a menos que realmente saiba o que está fazendo), pois macros conflitantes podem causar diversos problemas estranhos e frustrantes.

> **Nota:** Se você tiver uma seção `[homing_override]`, precisará atualizar quaisquer comandos `G28` na parte do G-Code para usar `G28.6245197` em vez disso (que é a versão renomeada do `G28` incorporado ao Klipper). Não fazer isso causará erro nos comandos `G28` com a mensagem ***Macro G28 chamada recursivamente***.

# Configuração do Klipper

```
# Todas as personalizações estão documentadas em globals.cfg. Basta copiar uma variável de
# na seção abaixo e altere o valor para atender às suas necessidades.

[gcode_macro _km_options]
# Estes são exemplos de algumas personalizações prováveis:
# Todas as folhas da lista abaixo estarão disponíveis com deslocamento configurável.
#variable_bed_surfaces: ['smooth_1','texture_1']
# Comprimento (em mm) do filamento a carregar (os tubos Bowden serão mais longos).
#variable_load_length: 90.0
# Oculte o menu LCD do Octoprint, pois não o uso.
#variable_menu_show_octoprint: False
# Personalize os menus de filamentos (até 10 entradas).
#variable_menu_temperature: [
#  {'name' : 'PLA',  'extruder' : 200.0, 'bed' : 60.0},
#  {'name' : 'PETG', 'extruder' : 230.0, 'bed' : 85.0},
#  {'name' : 'ABS',  'extruder' : 245.0, 'bed' : 110.0, 'chamber' : 60}]
# Comprimento do filamento (em milímetros) a ser eliminado no início da impressão.
#variable_start_purge_length: 30 # Este valor funciona para a maioria das configurações.
gcode: # Esta linha é exigida pelo Klipper.
# Qualquer código que você colocar aqui será executado na inicialização do klipper, após a inicialização
# para essas macros. Por exemplo, você poderia descomentar a seguinte linha para
# ajusta automaticamente os deslocamentos da superfície da cama para levar em conta quaisquer alterações feitas
# ao seu fim de curso Z ou deslocamento da sonda.
#  ADJUST_SURFACE_OFFSETS

# Esta linha inclui todas as macros padrão.
[include klipper-macros/*.cfg]
# Remova o comentário para incluir recursos que requerem suporte de hardware específico.
# Suporte ao menu LCD para recursos como seleção da superfície da cama e pausa na próxima camada.
#[include klipper-macros/optional/lcd_menus.cfg]
# Nivelamento otimizado da cama
#[include klipper-macros/optional/bed_mesh.cfg]

# As seções abaixo são necessárias para que as macros funcionem. Se sua configuração
# já possui algumas dessas seções, você deve mesclar as duplicatas em uma
# (ou se forem idênticos basta remover um deles).
[idle_timeout]
gcode:
  _KM_IDLE_TIMEOUT # Esta linha deve estar na sua seção idle_timeout.

[pause_resume]

[respond]

[save_variables]
filename: ~/printer_data/variables.cfg # ATUALIZE ISSO PARA SEU CAMINHO!!!

[virtual_sdcard]
path: ~/gcode_files # ATUALIZE ISSO PARA SEU CAMINHO!!!
on_error_gcode: CANCEL_PRINT

[display_status]
```

## Configuração do Slicer

### PrusaSlicer / SuperSlicer

O PrusaSlicer e suas variantes são relativamente fáceis de configurar. Basta abrir **Configurações da Impressora → Custom G-code** para a sua impressora Klipper e colar o texto abaixo nas seções relevantes.

#### Código de Início (Start G-code)

```
M190 S0 ; Remova isso se autoemit_temperature_commands estiver desativado no Prusa Slicer 2.6 e posterior
M109 S0 ; Remova isso se autoemit_temperature_commands estiver desativado no Prusa Slicer 2.6 e posterior
_PRINT_START_PHASE_INIT EXTRUSORA={first_layer_temperature[initial_tool]} MESA=[first_layer_bed_temperature] MESH_MIN={first_layer_print_min[0]},{first_layer_print_min[1]} MESH_MAX={first_layer_print_max[0]},{first_layer_print_max[1]} CAMADAS={total_layer_count} TAMANHO_DO_BICO={nozzle_diameter[0]}
; Insira o gcode personalizado aqui.
_PRINT_START_PHASE_PREHEAT
; Insira o gcode personalizado aqui.
_PRINT_START_PHASE_PROBING
; Insira o gcode personalizado aqui.
_PRINT_START_PHASE_EXTRUDER
; Insira o gcode personalizado aqui.
_PRINT_START_PHASE_PURGE

; Este é o lugar para colocar as linhas de purga do slicer se você não tiver definido um comprimento de purga não nulo
; variable_start_purge_length para que START_PRINT calcule e execute automaticamente a purga (por exemplo, se estiver usando um Mosaic Palette,
; que exige que o slicer gere a purga).
```

#### G-Code adicional de início do SuperSlicer

Se estiver usando o SuperSlicer, você pode adicionar o seguinte imediatamente antes do
Linha `PRINT_START` acima. Isso executará algumas verificações adicionais de limites e
permitirá que você use o recurso de realocação aleatória de impressão sem a necessidade
Entradas `exclude_object` no arquivo de impressão.

```
PRINT_START_SET MODEL_MIN={bounding_box[0]},{bounding_box[1]} MODEL_MAX={bounding_box[3]},{bounding_box[4]}
```

#### End G-code

```
PRINT_END
```

#### Antes da mudança de camada do G-Code

```
;BEFORE_LAYER_CHANGE
;[layer_z]
BEFORE_LAYER_CHANGE HEIGHT=[layer_z] LAYER=[layer_num]
```

#### Após mudança de camada G-Code

```
;AFTER_LAYER_CHANGE
;[layer_z]
AFTER_LAYER_CHANGE
```

### Ultimaker Cura

Cura é um pouco mais difícil de configurar e vem com os seguintes recursos conhecidos
problemas:

- O Cura não possui espaços reservados adequados para antes e depois das alterações de camada, então
   o antes desencadeia todo o fogo e é seguido imediatamente pelo depois
   gatilhos, tudo isso acontece dentro da mudança de camada. Isso provavelmente não
   importa, mas significa que você não pode usar os gatilhos antes e depois para
   evite executar código na mudança de camada.
- O Cura não fornece a altura Z da camada atual, então é inferido
   a posição atual do bico, que incluirá o Z-hop se o bico estiver
   atualmente levantada. Isso significa que os gatilhos do gcode baseados em altura podem ser acionados antes
   esperado.
- **Inserir na mudança de camada** do Cura dispara o gatilho `After` e então o
   Gatilho `Before` (ou seja, antes ou depois da *camada*, versus antes ou depois do
   *mudança de camada*). Essas macros e o PrusaSlicer fazem o oposto, que é
   algo para ter em mente se você está acostumado com a forma como o Cura faz isso. Observe que estes
   macros usam um script **Inserir na mudança de camada** para forçar o comentário `LAYER`
   geração, mas isso não afeta a ordem do gatilho.
- Cura não fornece o retângulo delimitador da primeira camada, apenas o modelo
   volume limite. Isso significa que a caixa delimitadora XY é usada para acelerar a sondagem da malha
   pode ser maior do que o necessário, resultando em uma sondagem do leito que não é tão rápida
   como poderia ser.

Aceitando as advertências, as macros funcionam muito bem com o Cura se você seguir as instruções
etapas de configuração listadas abaixo.
#### Start G-code

```
M190 S0
M109 S0
_PRINT_START_PHASE_INIT EXTRUSORA={material_print_temperature_layer_0} MESA={material_bed_temperature_layer_0} TAMANHO_DO_BICO={machine_nozzle_size}
; Insira o gcode personalizado aqui.
_PRINT_START_PHASE_PREHEAT
; Insira o gcode personalizado aqui.
_PRINT_START_PHASE_PROBING
; Insira o gcode personalizado aqui.
_PRINT_START_PHASE_EXTRUDER
; Insira o gcode personalizado aqui.
_PRINT_START_PHASE_PURGE

; Este é o local para colocar as linhas de purga do slicer se você não tiver definido um comprimento de purga não nulo
; variable_start_purge_length para que START_PRINT calcule e execute automaticamente a purga (por exemplo, se estiver usando um Mosaic Palette,
; que exige que o slicer gere a purga).

```

#### Fim do G-Code

```
PRINT_END
```

#### Plug-in de pós-processamento

Use o item de menu **Extensões → Pós-processamento → Modificar G-Code** para
abra o **Plugin de pós-processamento** e adicione os quatro scripts a seguir. *O
os scripts devem ser executados na ordem listada abaixo e certifique-se de copiar as strings
exatamente, sem espaços iniciais ou finais.*

##### Search and Replace

- Search: `(\n;(MIN|MAX)X:([^\n]+)\n;\2Y:([^\n]+))`
- Replace: `\1\nPRINT_START_SET MESH_\2=\3,\4`
- Use Regular Expressions: ☑️

##### Search and Replace

- Search: `(\n;LAYER_COUNT:([^\n]+))`
- Replace: `\1\nINIT_LAYER_GCODE LAYERS=\2\nPRINT_START_SET LAYERS=\2`
- Use Regular Expressions: ☑️

##### Insert at layer change

- When to insert: `Before`
- G-code to insert: `;BEFORE_LAYER_CHANGE`

##### Search and Replace

- Search: `(\n;LAYER:([^\n]+))`
- Replace: `\1\nBEFORE_LAYER_CHANGE LAYER=\2\nAFTER_LAYER_CHANGE`
- Use Regular Expressions: ☑️

## Moonraker Configuration

Depois de ter as macros funcionando e se sentir confortável em usá-las, você pode ter
Moonraker mantenha-os atualizados adicionando o seguinte em seu
`moonraker.conf`.

```
[update_manager klipper-macros]
type: git_repo
origin: https://github.com/jschuh/klipper-macros.git
path: ~/printer_data/config/klipper-macros # ATUALIZE ISSO PARA O SEU CAMINHO!!!
primary_branch: main
is_system_service: False
managed_services: klipper
```

> **Nota:** Eu desaconselho adicionar as entradas de atualização automática ao Moonraker até
>você tem tudo funcionando bem, pois pode dificultar um pouco a desinstalação
> mais difícil devido ao comportamento de atualização automática do Moonraker.

## Remoção

Se você optar por desinstalar essas macros, basicamente precisará reverter o processo.
etapas de instalação. No entanto, as partes mais críticas estão listadas abaixo.

### Remoção da configuração do Klipper

Certifique-se de remover o seguinte da configuração do Klipper (e qualquer
configurações):

* A seção completa `[gcode_macro _km_options]`
* Quaisquer seções `include` com `klipper-macros` no caminho
* `_KM_IDLE_TIMEOUT` na seção `[idle_timeout]`
* 
Se você não tiver as atualizações automáticas do Moonraker configuradas, você pode excluir o
diretório `klipper-macros` com algo como o seguinte comando (ajustado
para seus próprios caminhos):

```
rm -rf ~/printer_data/config/klipper-macros
```

### Remoção da configuração do Slicer

Se você não quiser alterar a configuração do seu slicer, você poderá sair
como está, porque gera apenas uma pequena quantidade de gcode adicional, e o
parâmetros básicos devem funcionar com qualquer outra macro `PRINT_START`.

## Remoção da configuração do Moonraker

Se você configurou as atualizações automáticas do Moonraker, você precisará remover todo o
seção `[update_manager klipper-macros]` e reinicie o moonraker antes de
excluindo o diretório `klipper-macros`, caso contrário o Moonraker pode tentar
recrie-o. Você também pode descobrir que são necessárias algumas verificações de atualização do Moonraker e
reinicia antes que a seção klipper-macros desapareça da IU.

# Referência de comando

## Costumização

Todos os recursos são configurados definindo valores `variable_` no
Seção `[gcode_macro _km_options]`. Todas as variáveis disponíveis e sua finalidade
estão listados em [globals.cfg](globals.cfg#L5).

> **Observação:** personalizações específicas de `PRINT_START` são [abordadas com mais detalhes
   abaixo](#print-start-and-end).

Veja todas as macros em [macros](MACROS.md)