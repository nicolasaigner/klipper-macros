# macros de corte

> [Readme na versão PT-BR](https://github.com/nicolasaigner/klipper-macros/blob/develop/README.pt.md)

Esta é uma coleção de macros para o[Cortando firmware da impressora 3D](https://github.com/Klipper3d/klipper). EU
criei originalmente este repositório apenas para ter um conjunto consistente de macros compartilhadas
entre minhas próprias impressoras 3D. Mas como os achei úteis, pensei em outras
as pessoas também poderiam.

## O que posso fazer com isso?

A maioria dessas macros melhora a funcionalidade básica (por exemplo,[folhas de construção selecionáveis](#bed-surface)) e compatibilidade do Klipper com código g direcionado ao Marlin
impressoras. No entanto, também existem alguns extras interessantes:

-   **[Programar comandos em alturas e mudanças de camadas](#layer-triggers)**-
    Isso é semelhante ao que seu fatiador já pode fazer, mas acho mais simples e
    você pode agendar esses comandos enquanto uma impressão está ativa. Como exemplo de
    uso, adicionei um[Item de menu LCD](#lcd-menus)para pausar a impressão na próxima
    mudança de camada. Dessa forma, a pausa não prejudicará a impressão, por ex. pausando dentro
    um perímetro externo.
-   **Dimensionar dinamicamente[aquecedores](#heaters)e[fãs](#fans)**- Isso faz com que
    fácil de fazer coisas como ajustar persistentemente as configurações do ventilador durante uma impressão ao vivo,
    ou manter perfis de segmentação mais simples movendo coisas como um aquecedor para um
    bocal de aço endurecido no estado armazenado na impressora.
-   **Limpador[Interface do menu LCD](#lcd-menus)**- Simplifiquei os menus e
    forneceu uma maneira muito mais fácil de personalizar materiais no menu LCD (ou pelo menos
    Eu penso que sim). Também adicionei caixas de diálogo de confirmação para comandos que seriam
    anular uma impressão ativa.
-   **[Nivelamento otimizado da base de malha](#bed-mesh-improvements)**- Sondas apenas dentro
    a área impressa, o que pode economizar muito tempo em impressões menores.
-   **[Linhas de purga automatizadas](#draw_purge_line)**- Defina a extrusão desejada
    comprimento como`variable_start_purge_length`na sua configuração e um tamanho correto
    conjunto de linhas de purga será extrudado na frente da área de impressão imediatamente
    antes do início da impressão.

## Alguns avisos...

-   **FAÇA BACKUP DE SUA CONFIGURAÇÃO COMPLETA ANTES DE FAZER QUALQUER ALTERAÇÃO!!!**Eu vi tantos
    recém-chegados procurando desesperadamente ajuda em fóruns públicos porque não
    ter uma boa configuração para voltar depois de bagunçar sua configuração atual enquanto
    experimentando macros de outras pessoas. Você salvará a si mesmo e a todos
    caso contrário, muito tempo e incômodo se você apenas garantir que sempre terá um
    configuração de trabalho com backup.
-   **Você realmente deve evitar macros personalizadas como essa até se sentir confortável
    usando o Klipper com uma configuração básica.**Macros avançadas do Klipper tendem a depender
    extensivamente em[remendo de macaco](https://en.wikipedia.org/wiki/Monkey_patch),
    o que pode levar a problemas com configurações incomuns ou ao misturar macros
    de várias fontes. Então, você realmente quer saber o que está fazendo antes
    incluindo macros de outra pessoa, especialmente ao incluir macros com
    funcionalidade sobreposta de diferentes fontes.
-   Você deve ter um`heater_bed`,`extruder`, e outro[seções listadas
    abaixo](#klipper-setup)configurado, caso contrário as macros**_forçar um
    desligamento da impressora na inicialização_**. Infelizmente, o sistema macro Klipper
    não tem uma maneira mais elegante de lidar com esse tipo de coisa.
-   A funcionalidade da multiextrusora e do aquecedor de câmara é muito pouco testada e
    pode ter bugs, já que não o usei muito. Patches são bem-vindos.
-   Provavelmente há outras coisas que não usei o suficiente para testar completamente, então use
    essas macros por sua conta e risco.

# Solução de problemas

-   Verifique novamente se você seguiu o[instruções de instalação](#installation)e não estou vendo nenhum erro de console ou de log.
-   Certifique-se de estar executando a versão mais atual do Klipper padrão e não
    um fork ou uma cópia alterada ou desatualizada.
-   Verifique se você está usando a versão mais atual dessas macros e não
    fez alterações em quaisquer arquivos no`klipper-macros`diretório.
-   Certifique-se de ter reiniciado o Klipper após quaisquer atualizações ou alterações de configuração.
-   Correr`CHECK_KM_CONFIG`no console do Klipper e corrija quaisquer erros relatados
    para o console e/ou logs (não produzirá nada se não houver erros de configuração
    foram detectados).
-   Correr`_INIT_SURFACES`no console do Klipper para validar se as superfícies da cama estão
    sendo inicializado sem nenhum erro relatado ao console e/ou logs.
-   Verifique as configurações da segmentação de dados e verifique se a saída do gcode está correta. Pagar
    atenção especial às partes de inicialização do gcode e ao
    parâmetros passados ​​para PRINT_START.
-   Procure problemas semelhantes e publique perguntas de solução de problemas no[Perguntas e respostas do Github
    Discussão](https://github.com/jschuh/klipper-macros/discussions/categories/q-a).

# Relatando erros

Se você seguiu as etapas de solução de problemas e não conseguiu resolver o problema
problema você pode[relatar um bug via Github](https://github.com/jschuh/klipper-macros/issues/new/choose). Eu provavelmente irei
responda dentro de alguns dias (quase certamente dentro de uma semana). Eu provavelmente não vou
responder através de outros canais (por exemplo, Discord, Twitter), porque não encontro
são úteis para lidar com relatórios de bugs.

Algumas coisas importantes para lembrar ao relatar bugs:

-   **Cole o texto completo do comando que acionou o erro, junto com qualquer
    mensagens de erro impressas no console**e seções relevantes do klipper
    logs, se apropriado (e por favor[formate este texto como código](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax#quoting-code),
    caso contrário, o Github irá formatá-lo como uma nota de resgate).
-   **Anexe sua configuração ao relatório de bug.**Geralmente não há como diagnosticar
    qualquer coisa sem as configurações.
-   **Verifique se o seu problema é reproduzido na instalação atual de estoque do
    Klipper e macros klipper.**Configurações fora de estoque e versões desatualizadas
    tornar o diagnóstico quase impossível.
-   Por favor, não trate os relatórios de bugs como um substituto para seguir a instalação
    e instruções para solução de problemas.
-   Direcione as solicitações de recursos para o[Discussão de ideias no Github](https://github.com/jschuh/klipper-macros/discussions/categories/ideas).

> **Observação:**Relatórios que não seguem as diretrizes acima_**provavelmente será
> fechado sem qualquer outra ação tomada.**_

# Contribuindo

Fico feliz em aceitar PRs de correção de bugs. Também estou potencialmente aberto a aceitar novos
recursos ou adições. No entanto, posso recusar o PR se for algo que não sou
interessado ou apenas parece que seria um incômodo para mim manter.

## Formatação

Não existe um estilo padrão para macros do Klipper, então tente seguir o
estilo nos arquivos. Dito isto, aqui estão algumas regras a serem lembradas:

-   Enrole em 80 caracteres, se possível
-   Recuar 2 espaços e alinhar com o bloco lógico ao agrupar (sem tabulações)
-   Prefixar macros internas com`_`ou`_km_`
-   Prefixe qualquer tipo de estado global com`_KM_`(e.g.`_KM_SAVE_GCODE_STATE`)

## Confirmar mensagens

Estas são as regras para mensagens de commit, mas você também pode apenas olhar o
commit log e siga o padrão observado:

-   Use a regra 50/72 para mensagens de commit: Não mais que 50 caracteres no
    título e linhas de quebra na descrição com 72 caracteres.
-   Comece o título com o nome do módulo (geralmente o arquivo principal que está sendo modificado,
    menos qualquer extensão) seguido por dois pontos.
-   Mensagens de commit somente com título são adequadas para commits simples, mas certifique-se de
    inclua uma linha em branco após o título.
-   Esmague vários commits se o que você está trabalhando faz mais sentido como um
    commit lógico único._Isso pode exigir que você faça um empurrão forçado em uma abertura
    RP._

# Instalação

Para instalar as macros, primeiro clone este repositório dentro do seu`printer_data/config`diretório com o seguinte comando.

    git clone https://github.com/jschuh/klipper-macros.git

Em seguida, cole as seções abaixo em seu`printer.cfg`para começar. Ou mesmo
melhor, cole tudo em um arquivo separado no mesmo caminho da sua configuração,
e inclua esse arquivo. Isso tornará mais fácil se você quiser remover esses
macros no futuro.

Pode ser necessário personalizar algumas configurações para sua própria configuração. Tudo configurável
as configurações estão em[globals.cfg](globals.cfg#L5), e pode ser substituído criando
uma variável correspondente com um novo valor em seu`[gcode_macro _km_options]`seção._**Não modifique diretamente as declarações de variáveis ​​em globals.cfg.**_A inicialização da macro assume certos valores padrão e direciona
modificações provavelmente quebrarão as coisas de maneiras muito inesperadas.

> **Observação:**Os caminhos neste README seguem[Estrutura da pasta de dados do Moonraker.](https://moonraker.readthedocs.io/en/latest/installation/#data-folder-structure)Pode ser necessário alterá-los se estiver usando uma estrutura diferente.

> **Observação:**Certifique-se de que você não tenha nenhuma macro que forneça o mesmo
> função básica como as macros neste repositório (por exemplo, o padrão[Vela grande](https://docs.mainsail.xyz/configuration#macros)ou[fluidez](https://docs.fluidd.xyz/configuration/initial_setup#macros)macros).
> Como regra, você deve evitar usar vários conjuntos de macros que substituem o
> mesma macro base (a menos que você realmente saiba o que está fazendo) porque conflitantes
> macros podem causar todos os tipos de problemas estranhos e frustrantes.

> **Observação:**Se você tem um`[homing_override]`seção que você precisará atualizar
> qualquer`G28`comandos na parte gcode para usar`G28.6245197`em vez disso (que é
> a versão renomeada do integrado do Klipper`G28`). Não fazer isso
> causa`G28`comandos para errar com a mensagem**_Macro G28 chamada
> recursivamente_**.

# Configuração do cortador

    # All customizations are documented in globals.cfg. Just copy a variable from
    # there into the section below, and change the value to meet your needs.

    [gcode_macro _km_options]
    # These are examples of some likely customizations:
    # Any sheets in the below list will be available with a configurable offset.
    #variable_bed_surfaces: ['smooth_1','texture_1']
    # Length (in mm) of filament to load (bowden tubes will be longer).
    #variable_load_length: 90.0
    # Hide the Octoprint LCD menu since I don't use it.
    #variable_menu_show_octoprint: False
    # Customize the filament menus (up to 10 entries).
    #variable_menu_temperature: [
    #  {'name' : 'PLA',  'extruder' : 200.0, 'bed' : 60.0},
    #  {'name' : 'PETG', 'extruder' : 230.0, 'bed' : 85.0},
    #  {'name' : 'ABS',  'extruder' : 245.0, 'bed' : 110.0, 'chamber' : 60}]
    # Length of filament (in millimeters) to purge at print start.
    #variable_start_purge_length: 30 # This value works for most setups.
    gcode: # This line is required by Klipper.
    # Any code you put here will run at klipper startup, after the initialization
    # for these macros. For example, you could uncomment the following line to
    # automatically adjust your bed surface offsets to account for any changes made
    # to your Z endstop or probe offset.
    #  ADJUST_SURFACE_OFFSETS

    # This line includes all the standard macros.
    [include klipper-macros/*.cfg]
    # Uncomment to include features that require specific hardware support.
    # LCD menu support for features like bed surface selection and pause next layer.
    #[include klipper-macros/optional/lcd_menus.cfg]
    # Optimized bed leveling
    #[include klipper-macros/optional/bed_mesh.cfg]

    # The sections below here are required for the macros to work. If your config
    # already has some of these sections you should merge the duplicates into one
    # (or if they are identical just remove one of them).
    [idle_timeout]
    gcode:
      _KM_IDLE_TIMEOUT # This line must be in your idle_timeout section.

    [pause_resume]

    [respond]

    [save_variables]
    filename: ~/printer_data/variables.cfg # UPDATE THIS FOR YOUR PATH!!!

    [virtual_sdcard]
    path: ~/gcode_files # UPDATE THIS FOR YOUR PATH!!!
    on_error_gcode: CANCEL_PRINT

    [display_status]

## Configuração do Slicer

### PrusaSlicer/SuperSlicer

PrusaSlicer e suas variantes são bastante fáceis de configurar. Apenas abra**Impressora
Configurações → Código G personalizado**para sua impressora Klipper e cole o texto abaixo
nas seções relevantes.

#### Iniciar código G

    M190 S0 ; Remove this if autoemit_temperature_commands is off in Prusa Slicer 2.6 and later
    M109 S0 ; Remove this if autoemit_temperature_commands is off in Prusa Slicer 2.6 and later
    _PRINT_START_PHASE_INIT EXTRUDER={first_layer_temperature[initial_tool]} BED=[first_layer_bed_temperature] MESH_MIN={first_layer_print_min[0]},{first_layer_print_min[1]} MESH_MAX={first_layer_print_max[0]},{first_layer_print_max[1]} LAYERS={total_layer_count} NOZZLE_SIZE={nozzle_diameter[0]}
    ; Insert custom gcode here.
    _PRINT_START_PHASE_PREHEAT
    ; Insert custom gcode here.
    _PRINT_START_PHASE_PROBING
    ; Insert custom gcode here.
    _PRINT_START_PHASE_EXTRUDER
    ; Insert custom gcode here.
    _PRINT_START_PHASE_PURGE

    ; This is the place to put slicer purge lines if you haven't set a non-zero
    ; variable_start_purge_length to have START_PRINT automatically calculate and 
    ; perform the purge (e.g. if using a Mosaic Palette, which requires the slicer
    ; to generate the purge).

#### Código G adicional do SuperSlicer Start

Se estiver usando o SuperSlicer, você pode adicionar o seguinte imediatamente antes do`PRINT_START`linha de cima. Isso executará algumas verificações adicionais de limites e
permitirá que você use o recurso de realocação aleatória de impressão sem a necessidade`exclude_object`entradas no arquivo de impressão.

    PRINT_START_SET MODEL_MIN={bounding_box[0]},{bounding_box[1]} MODEL_MAX={bounding_box[3]},{bounding_box[4]}

#### Fim do código G

    PRINT_END

#### Antes da mudança de camada do código G

    ;BEFORE_LAYER_CHANGE
    ;[layer_z]
    BEFORE_LAYER_CHANGE HEIGHT=[layer_z] LAYER=[layer_num]

#### Após a mudança de camada, código G

    ;AFTER_LAYER_CHANGE
    ;[layer_z]
    AFTER_LAYER_CHANGE

### Ultimaker Cura

Cura é um pouco mais difícil de configurar e vem com os seguintes recursos conhecidos
problemas:

-   O Cura não possui espaços reservados adequados para antes e depois das alterações de camada, então
    o antes desencadeia todo o fogo e é seguido imediatamente pelo depois
    gatilhos, tudo isso acontece dentro da mudança de camada. Isso provavelmente não
    importa, mas significa que você não pode usar os gatilhos antes e depois para
    evite executar código na mudança de camada.
-   O Cura não fornece a altura Z da camada atual, então é inferido de
    a posição atual do bico, que incluirá o Z-hop se o bico estiver
    atualmente levantada. Isso significa que os gatilhos do gcode baseados em altura podem ser acionados antes
    esperado.
-   Cura's**Inserir na mudança de camada**dispara o`After`gatilho e depois o`Before`gatilho (ou seja, antes ou depois do_layer_, versus antes ou depois do_mudança de camada_). Essas macros e o PrusaSlicer fazem o oposto, que é
    algo para ter em mente se você está acostumado com a forma como o Cura faz isso. Observe que estes
    macros usam um**Inserir na mudança de camada**script para forçar`LAYER`Comente
    geração, mas isso não afeta a ordem do gatilho.
-   Cura não fornece o retângulo delimitador da primeira camada, apenas o modelo
    volume limite. Isso significa que a caixa delimitadora XY é usada para acelerar a sondagem da malha
    pode ser maior do que o necessário, resultando em uma sondagem do leito que não é tão rápida
    como poderia ser.

Aceitando as advertências, as macros funcionam muito bem com o Cura se você seguir as instruções
etapas de configuração listadas abaixo.

#### Iniciar código G

    M190 S0
    M109 S0
    _PRINT_START_PHASE_INIT EXTRUDER={material_print_temperature_layer_0} BED={material_bed_temperature_layer_0} NOZZLE_SIZE={machine_nozzle_size}
    ; Insert custom gcode here.
    _PRINT_START_PHASE_PREHEAT
    ; Insert custom gcode here.
    _PRINT_START_PHASE_PROBING
    ; Insert custom gcode here.
    _PRINT_START_PHASE_EXTRUDER
    ; Insert custom gcode here.
    _PRINT_START_PHASE_PURGE

    ; This is the place to put slicer purge lines if you haven't set a non-zero
    ; variable_start_purge_length to have START_PRINT automatically calculate and 
    ; perform the purge (e.g. if using a Mosaic Palette, which requires the slicer
    ; to generate the purge).

#### Fim do código G

    PRINT_END

#### Plug-in de pós-processamento

Use o item de menu para**Extensões → Pós-processamento → Modificar código G**para
abra o**Plug-in de pós-processamento**e adicione os quatro scripts a seguir._O
os scripts devem ser executados na ordem listada abaixo e certifique-se de copiar as strings
exatamente, sem espaços iniciais ou finais._

##### Pesquisar e substituir

-   Procurar:`(\n;(MIN|MAX)X:([^\n]+)\n;\2Y:([^\n]+))`
-   Substituir:`\1\nPRINT_START_SET MESH_\2=\3,\4`
-   Use expressões regulares: ☑️

##### Pesquisar e substituir

-   Procurar:`(\n;LAYER_COUNT:([^\n]+))`
-   Substituir:`\1\nINIT_LAYER_GCODE LAYERS=\2\nPRINT_START_SET LAYERS=\2`
-   Use expressões regulares: ☑️

##### Inserir na mudança de camada

-   Quando inserir:`Before`
-   Código G para inserir:`;BEFORE_LAYER_CHANGE`

##### Pesquisar e substituir

-   Procurar:`(\n;LAYER:([^\n]+))`
-   Substituir:`\1\nBEFORE_LAYER_CHANGE LAYER=\2\nAFTER_LAYER_CHANGE`
-   Use expressões regulares: ☑️

## Configuração do Moonraker

Depois de ter as macros funcionando e se sentir confortável em usá-las, você pode ter
Moonraker mantenha-os atualizados adicionando o seguinte em seu`moonraker.conf`.

    [update_manager klipper-macros]
    type: git_repo
    origin: https://github.com/jschuh/klipper-macros.git
    path: ~/printer_data/config/klipper-macros # UPDATE THIS FOR YOUR PATH!!!
    primary_branch: main
    is_system_service: False
    managed_services: klipper

> **Observação:**Eu desaconselho adicionar entradas de atualização automática ao Moonraker até
> você tem tudo funcionando bem, pois pode dificultar um pouco a desinstalação
> mais difícil devido ao comportamento de atualização automática do Moonraker.

## Remoção

Se você optar por desinstalar essas macros, basicamente precisará reverter o processo.
etapas de instalação. No entanto, as partes mais críticas estão listadas abaixo.

### Remoção de configuração de clipes

Certifique-se de remover o seguinte da configuração do Klipper (e qualquer
configurações):

-   O cheio`[gcode_macro _km_options]`seção
-   Qualquer`include`seções com`klipper-macros`no caminho
-   `_KM_IDLE_TIMEOUT`no`[idle_timeout]`seção

Se você não tiver as atualizações automáticas do Moonraker configuradas, você pode excluir o`klipper-macros`diretório com algo como o seguinte comando (ajustado
para seus próprios caminhos):

    rm -rf ~/printer_data/config/klipper-macros

### Remoção da configuração do Slicer

Se você não quiser alterar a configuração do seu slicer, você poderá sair
como está, porque gera apenas uma pequena quantidade de gcode adicional, e o
parâmetros básicos devem funcionar com qualquer outro`PRINT_START`macros.

## Remoção da configuração do Moonraker

Se você configurou as atualizações automáticas do Moonraker, você precisará remover todo o`[update_manager klipper-macros]`seção e reinicie o moonraker antes de
excluindo o`klipper-macros`diretório, caso contrário Moonraker pode tentar
recrie-o. Você também pode descobrir que são necessárias algumas verificações de atualização do Moonraker e
reinicia antes que a seção klipper-macros desapareça da IU.

# Referência de comando

## Costumização

Todos os recursos são configurados pela configuração`variable_`valores no`[gcode_macro _km_options]`seção. Todas as variáveis ​​disponíveis e sua finalidade
estão listados em[globals.cfg](globals.cfg#L5).

> **Observação:**`PRINT_START`personalizações específicas são[abordado com mais detalhes
>   abaixo](#print-start-and-end).

### Melhorias na malha da cama

`BED_MESH_CALIBRATE_FAST`

Envolve o Klipper`BED_MESH_CALIBRATE`comando para dimensionar e redistribuir o
pontos de sonda de modo que apenas a área apropriada em`MESH_MIN`e`MESH_MAX`é
sondado. Isto pode reduzir drasticamente os tempos de sondagem para qualquer coisa que não
preencha a primeira camada da cama. Se o`MESH_MIN`e`MESH_MAX`argumentos
são fornecidos a`PRINT_START`ele usará isso automaticamente para malha de cama
calibração (desde que`[bed_mesh]`seção é detectada em sua configuração).

As seguintes opções de configuração adicionais estão disponíveis em[globals.cfg](globals.cfg#L5).

-   `variable_probe_mesh_padding`- Preenchimento extra ao redor do retângulo definido por`MESH_MIN`e`MESH_MAX`.
-   `variable_probe_min_count`- Número mínimo de sondas para sondagem parcial de um
    malha de cama.
-   `variable_probe_count_scale`- Fator de escala para aumentar a contagem de sondas para
     sondas de leito parcial.

> **Observação:**Veja o[seção opcional](#bed-mesh)para macros adicionais.

> **Observação:**As otimizações da malha da cama são desativadas silenciosamente para impressoras delta.

> **Observação:**Se o`bed_mesh`configuração inclui um[`relative_reference_index`](https://www.klipper3d.org/Bed_Mesh.html#the-relative-reference-index)então
>   o ponto de índice selecionado na malha otimizada será o ponto mais próximo de
>   o ponto de índice na malha da configuração. No entanto, se um`RELATIVE_REFERENCE_INDEX`parâmetro é fornecido diretamente para esta macro
>   será usado sem modificações.

`BED_MESH_CHECK`

-   `ABORT`- Defina um valor diferente de zero para abortar o processamento da macro em caso de erro.
-   `MESH_MIN`- Consulte a documentação do Klipper para`BED_MESH_CALIBRATE`.
-   `MESH_MAX`- Consulte a documentação do Klipper para`BED_MESH_CALIBRATE`.

Verifica o`[bed_mesh]`config e parâmetros fornecidos opcionalmente. Irá avisar
(ou opcionalmente abortar) se`mesh_min`ou`mesh_max`poderia permitir uma mudança de
alcance durante`BED_MESH_CALIBRATE`. Isso é executado implicitamente na inicialização do Klipper
e como parte`BED_MESH_CALIBRATE_FAST`.

### Superfície da cama

Fornece um conjunto de macros para selecionar diferentes superfícies da cama com
ajustes de deslocamento Z correspondentes para compensar sua espessura. Todos
superfícies disponíveis devem ser listadas no`variable_bed_surfaces`variedade.
Menus LCD correspondentes para seleção de folhas e babystep serão adicionados ao_Configurar_e_Afinação_se[`lcd_menus.cfg`](#lcd-menus)está incluído. Qualquer deslocamento Z
ajustes feitos nos menus LCD, console ou outros clientes (por exemplo, vela principal,
Fluidd) será aplicado à planilha atual e persistirá nas reinicializações.

#### `ADJUST_SURFACE_OFFSETS`

Ajusta os deslocamentos de superfície para levar em conta as alterações na posição do fim de curso Z ou
deslocamento Z da sonda. Uma mensagem para invocar este comando será mostrada no console
quando uma alteração relevante for feita`printer.cfg`.

-   `IGNORE`- Defina como 1 para redefinir os deslocamentos salvos sem ajustar as superfícies.

#### `LOAD_SURFACE_MESH`

Tenta carregar uma malha associada à superfície especificada.

-   `SURFACE`_(padrão: superfície atual)_- Superfície da cama.

#### `MAKE_SURFACE_MESH`

Gera uma malha associada à superfície especificada. Se`variable_start_try_saved_surface_mesh`é verdade então`START_PRINT`irá carregar
esta malha quando a superfície for selecionada (e pule a etapa de nivelamento da malha se
foi especificado).

-   `BED`-_(padrão: 70)_Temperatura do leito ao sondar o leito.
-   `EXTRUDER`-_(padrão:`variable_start_extruder_probing_temp`)_Extrusora
    temperatura ao sondar a cama.
-   `SURFACE`_(padrão: superfície atual)_- Superfície da cama.
-   `MESH_MULTIPLIER`_(padrão: 2)_- Aumenta a densidade da malha pelo especificado
    valor inteiro, preservando os pontos de malha existentes e a referência relativa
    índice. Um valor de`1`deixa a malha inalterada,`2`duplica a densidade,`3`triplica, etc. (ou seja, se`bed_mesh`especifica`probe_count: 5,5`e`MESH_MULTIPLIER=2`então esta macro irá gerar uma grade 9x9, enquanto`MESH_MULTIPLIER=3`irá gerar uma grade 13x13.)
-   _Veja o Klipper`BED_MESH_CALIBRATE`documentação para argumentos adicionais._

#### `SET_SURFACE_ACTIVE`

Define a superfície ativa fornecida (de uma listada em listada em`variable_bed_surfaces`) e ajusta o deslocamento Z atual para corresponder ao
deslocamento para a superfície. Se não`SURFACE`argumento é fornecido o disponível
superfícies são listadas, com superfície ativa precedida por um`*`.

-   `SURFACE`- Superfície do leito com deslocamento associado.

#### `SET_SURFACE_OFFSET`

Define diretamente o deslocamento Z de`SURFACE`ao valor de`OFFSET`. Se não
argumento para`SURFACE`é fornecido que a superfície ativa atual seja usada. Se não
argumento para`OFFSET`é fornecido que o deslocamento atual seja exibido.

-   `OFFSET`- Novo deslocamento Z para a superfície determinada.
-   `SURFACE`_(padrão: superfície atual)_- Superfície da cama.

> **Observação:**O`SET_GCODE_OFFSET`macro é substituída para atualizar o
> deslocamento para a superfície ativa. Isso faz com que a superfície da cama funcione com deslocamento Z
> ajustes feitos através de qualquer interface ou cliente.

### Bip

Implementa o comando M300 (se um correspondente`[output_pin beeper]`seção é
presente). Este comando faz com que o alto-falante emita um tom audível.

#### `M300`

Emite um tom audível.

-   `P`_(padrão:`100`)_- Duração do tom (em milissegundos).
-   `S`_(padrão:`1000`)_- Frequência do tom.

### Empate

Fornece métodos convenientes para extrusão ao longo de um caminho e desenho de linhas de purga.

> **Observação:**As macros de desenho requerem cada`extruder`configurações para ter
> correto`nozzle_diameter`e`filament_diameter`configurações.

#### DRAW_LINE_TO

Expulsa uma linha de filamento na altura e largura especificadas a partir do atual
coordenada para a coordenada XY fornecida (usando a extrusora atualmente ativa).

-   `X`_(padrão: posição X atual)_- Coordenada X absoluta para desenhar.
-   `Y`_(padrão: posição Y atual)_- Coordenada Y absoluta para desenhar.
-   `HEIGHT`_(padrão: definido via`SET_DRAW_PARAMS`)_- Altura (em mm) usada para
    calcular o volume de extrusão.
-   `WIDTH`_(padrão: definido via`SET_DRAW_PARAMS`)_- Largura de extrusão em mm.
-   `FEEDRATE`_(padrão: definido via`SET_DRAW_PARAMS`)_- Avanço de desenho em mm/m.

> **Observação:**A posição do eixo Z deve ser definida antes de chamar esta macro. O`HEIGHT`O parâmetro é usado apenas para calcular o volume de extrusão.

#### SET_DRAW_PARAMS

Define os parâmetros padrão usados ​​por DRAW_LINE_TO. Isto é útil para reduzir`DRAW_LINE_TO`comprimentos de linha de comando (particularmente importantes ao depurar em
a consola).

-   `HEIGHT`_(opcional; 0,2 mm na inicialização)_- Altura (em mm) usada para
    calcular o volume de extrusão.
-   `WIDTH`_(opcional; diâmetro do bico na inicialização)_- Largura de extrusão em mm.
-   `FEEDRATE`_(opcional; 1200 mm/m na inicialização)_- Avanço de desenho em mm/m.

#### DRAW_PURGE_LINE

Move para uma posição na borda frontal da primeira camada de impressão e limpa a
comprimento especificado do filamento como uma linha (ou fileiras de linhas) na frente do
área de impressão fornecida. Se nenhuma área de impressão for especificada, as linhas de eliminação serão desenhadas em
a borda frontal da área máxima de impressão. Se nenhuma área de impressão estiver definida,
o padrão é os respectivos limites do eixo.

-   `PRINT_MIN`_(padrão:`variable_print_min`)_- Limite superior de impressão.
-   `PRINT_MAX`_(padrão:`variable_print_max`)_- Limite inferior de impressão.
-   `HEIGHT`_(padrão: 62,5% do diâmetro do bico)_- Altura de extrusão em mm.
-   `WIDTH`_(padrão: 125% do diâmetro do bico)_- Largura de extrusão em mm.
-   `LENGTH`_(padrão:`variable_start_purge_length`)_- Comprimento do filamento
    para purgar._O padrão em`variable_start_purge_length`também é a quantia
    que é eliminado automaticamente no início da impressão._

> **Observação:**Você deve definir`variable_print_min`e`variable_print_max`se o
> Os limites dos eixos X e Y em sua configuração permitem que seu cabeçote se mova para fora do
> área imprimível (por exemplo, para sondas encaixáveis ​​ou baldes de purga).

> **Observação:**Se a sua impressão tocar a borda frontal da cama, ela se sobreporá
> com as extrusões de`DRAW_PURGE_LINE`.

### Fãs

Implementa parâmetros de escala que alteram o comportamento do comando M106. Uma vez
definidos, esses parâmetros se aplicam a qualquer velocidade do ventilador até que sejam apagados (por padrão
isso acontece no início e no final da impressão).

#### `SET_FAN_SCALING`

Define parâmetros de escala para o ventilador da extrusora.

-   `BOOST`_(padrão:`0`)_- Adicionado à velocidade do ventilador.
-   `SCALE`_(padrão:`1.0`)_- O`BOOST`o valor é adicionado e então o ventilador
    velocidade é multiplicada por`SCALE`.
-   `MAXIMUM`_(padrão:`255`)_- A velocidade do ventilador não é maior
    que`MAXIMUM`.
-   `MINIMUM`_(padrão:`0`)_- A velocidade do ventilador é fixada em pelo menos
    que`MINIMUM`; se este for um valor diferente de zero, o ventilador poderá ser parado somente via
    o comando M107.
-   `SPEED`_(opcional)_- Isso especifica uma nova meta de velocidade, caso contrário, qualquer novo
    os ajustes serão aplicados ao valor não ajustado da última velocidade do ventilador definida.

#### `RESET_FAN_SCALING`

-   Limpa todos os fatores de escala de ventilador existentes.

### Filamento

#### `LOAD_FILAMENT`/`UNLOAD_FILAMENT`

Carrega ou descarrega o filamento no bocal.

-   `LENGTH`_(padrão:`variable_load_length`)_- O comprimento do filamento a carregar
    ou descarregar.
-   `SPEED`_(padrão:`variable_load_speed`)_- Velocidade (em mm/m) para alimentar o
    filamento.
-   `MINIMUM`_(padrão:`variable_load_min_temp`)_- Garante que a extrusora seja aquecida
     pelo menos à temperatura especificada.

#### Compatibilidade com Marlin

-   O`M701`e`M702`comandos são implementados com um comprimento de filamento padrão
    de`variable_load_length`.

### Aquecedores

Adiciona parâmetros de escala que podem alterar o comportamento do aquecedor especificado.
Uma vez definidos, esses parâmetros se aplicam a qualquer alvo de temperatura naquele aquecedor até
os parâmetros de escala são apagados. Uma temperatura alvo zero transformará o
aquecedor desligado independentemente dos parâmetros de escala.

#### `SET_HEATER_SCALING`

Define parâmetros de escala para o aquecedor especificado. Se executado sem argumentos
quaisquer aquecedores atualmente dimensionados e seus parâmetros de escala serão listados. Por
padrão, a escala é apagada no início e no final de uma impressão.

-   `HEATER`- O nome do aquecedor em escala.
-   `BOOST`_(padrão:`0.0`)_- Adicionado a uma temperatura alvo diferente de zero.
-   `SCALE`_(padrão:`1.0`)_- Multiplicado pelo alvo impulsionado
    temperatura.
-   `MAXIMUM`_(padrão:`max_temp`)_- A temperatura alvo é fixada
    para não maior que`MAXIMUM`. Este valor deve estar entre`min_temp`e`min_temp`, inclusive.
-   `MINIMUM`_(padrão:`min_temp`)_- Uma temperatura alvo diferente de zero é
    preso a nada menos que`MINIMUM`. Este valor deve estar entre`min_temp`e`min_temp`, inclusive.
-   `TARGET`_(opcional)_- Isto especifica uma nova temperatura alvo, caso contrário qualquer
    novos ajustes serão aplicados ao valor não ajustado da última meta definida
    temperatura.

> **Observação:**uma temperatura alvo zero desligará o aquecedor independentemente de
> parâmetros de escala.

#### `RESET_HEATER_SCALING`

Limpa a escala do aquecedor atual.

-   `HEATER`_(opcional)_- O nome do aquecedor a reiniciar.

> **Observação:**se nenhum argumento HEATER for especificado, os parâmetros de escala serão redefinidos
> para todos os aquecedores.

#### `SET_HEATER_TEMPERATURE_SCALED`

A versão em escala do Klipper`SET_HEATER_TEMPERATURE`. Todos os argumentos são os
iguais e a função é idêntica, exceto que os valores de escala são aplicados.

#### `TEMPERATURE_WAIT_SCALED`

A versão em escala do Klipper`TEMPERATURE_WAIT`. Todos os argumentos são os
iguais e a função é idêntica, exceto que os valores de escala são aplicados.

#### Compatibilidade com Marlin

-   Os comandos de aquecimento da câmara`M141`e`M191`são implementados se um
    correspondente`[heater_generic chamber]`seção é definida na configuração.
-   O`R`parâmetro de temperatura do Marlin é implementado para o`M109`e`M190`comandos. Este parâmetro causará uma espera até que a temperatura alvo
    estabiliza (ou seja, o comportamento normal do Klipper para`S`).
-   O`S`parâmetro para o`M109`e`M190`comandos é alterado para se comportar como
    acontece em Marlin. Em vez de causar uma espera até que a temperatura
    estabilizar, a espera será concluída assim que a temperatura alvo for
    excedido.
-   O`M109`,`M190`,`M191`,`M104`,`M140`, e`M141`são todos substituídos por
    implementar a escala do aquecedor descrita acima.

> **Observação:**Ambos`SET_HEATER_TEMPERATURE`e`TEMPERATURE_WAIT`são**não**substituído e não dimensionará valores. Isto significa que o dimensionamento do aquecedor
> ajustes feitos em clientes como Mainsail e Fluidd não serão escalonados
> (porque esse parecia ser o comportamento mais claro). O[menus LCD personalizados](#lcd-menus)também substituirá os controles de temperatura
> com versões sem escala. Se você usar os menus de estoque, você será dimensionado
> valores.

### Cinemática

#### `G28`

Estende o`G28`comando para adicionar retorno lento ao não realocar eixos já posicionados
quando o`O`argumento está incluído (equivalente ao mesmo argumento em Marlin).
Veja o Klipper`G28`documentação para informações gerais e detalhes sobre os outros
argumentos.

-   `O`- Omite eixos já referenciados do procedimento de localização.

> **Observação:**Se você tem um`[homing_override]`seção que você precisará atualizar
> qualquer`G28`comandos na parte gcode para usar`G28.6245197`em vez disso (que é
> a versão renomeada do integrado do Klipper`G28`). Não fazer isso
> causa`G28`comandos para errar com a mensagem**_Macro G28 chamada
> recursivamente_**.

#### `LAZY_HOME`

Aloja os eixos especificados; por padrão, omite quaisquer eixos que já estejam na posição inicial.

-   `AXES`_(padrão: XYZ)_- Lista de eixos para casa.
-   `LAZY`_(padrão: 1)_- Omite eixos já referenciados do procedimento de localização.

### Gatilhos de camada

Fornece a capacidade de executar comandos de código G especificados pelo usuário em camadas arbitrárias
mudanças.

#### `GCODE_AT_LAYER`

Executa comandos de código g abertos fornecidos pelo usuário na camada especificada pelo usuário ou
altura. Se nenhum argumento for especificado, ele exibirá todos os agendamentos atualmente
comandos de código g junto com sua camada ou altura associada.

-   `HEIGHT`- Altura Z (em mm) para executar o comando. Exatamente um dos`HEIGHT`ou`LAYER`deve ser especificado.
-   `LAYER`- Número da camada (indexado zero) para executar o comando. Exatamente um dos`HEIGHT`ou`LAYER`deve ser especificado. O valor especial`next`talvez
    especificado, execute o comando na próxima mudança de camada.
-   `COMMAND`- O comando para executar na mudança de camada. Tome cuidado para citar corretamente
    espaços e escapar de quaisquer caracteres especiais.
-   `CANCEL`_(padrão:`0`)_- Cancelar os comandos previamente agendados no
    determinada camada ou altura. Se não`COMMAND`argumento é fornecido todos os comandos em
    o especificado`LAYER`ou`HEIGHT`são cancelados.

#### `CANCEL_ALL_LAYER_GCODE`

Cancela todos os comandos do código G previamente agendados em qualquer camada ou altura.

#### Macros convenientes de mudança de camada

-   `PAUSE_NEXT_LAYER ...`
    -   Programa a impressão atual para pausar na próxima mudança de camada. Ver[`PAUSE`](#pause)macro para argumentos adicionais.
-   `PAUSE_AT_LAYER  { HEIGHT=<pos> | LAYER=<layer> } ...`
    -   Programa a impressão atual para pausar na mudança de camada especificada.
        Ver[`PAUSE`](#pause)para argumentos adicionais.
-   `SPEED_AT_LAYER { HEIGHT=<pos> | LAYER=<layer> } SPEED=<percentage>`
    -   Programa um ajuste de avanço na mudança de camada especificada. (`SPEED`parâmetro se comporta da mesma forma que o`M220``S`parâmetro.)
-   `FLOW_AT_LAYER { HEIGHT=<pos> | LAYER=<layer> } FLOW=<percentage>`
    -   Programa um ajuste de taxa de fluxo na mudança de camada especificada. (`FLOW`parâmetro se comporta da mesma forma que o`M221``S`parâmetro.)
-   `FAN_AT_LAYER { HEIGHT=<pos> | LAYER=<layer> } ...`
    -   Agenda um ajuste do ventilador na mudança de camada especificada. Ver[`SET_FAN_SCALING`](#set_fan_scaling)para argumentos adicionais.
-   `HEATER_AT_LAYER { HEIGHT=<pos> | LAYER=<layer> } ...`
    -   Agenda um ajuste do aquecedor na mudança de camada especificada. Ver[`SET_HEATER_SCALING`](#set_heater_scaling)para argumentos adicionais.

> **Observação:**Se algum gatilho causar uma exceção, a impressão atual será
> abortar. As macros de conveniência acima validam seus argumentos tanto quanto
> possível reduzir as chances de uma impressão abortada, mas não podem totalmente
> elimine o risco de uma macro fazer algo que aborte a impressão.

### Parque

Implementa o estacionamento do cabeçote de ferramenta.

#### `PARK`

Estaciona o cabeçote da ferramenta.

-   `P`_(padrão:`2`)_- Modo de estacionamento
    -   `P=0`- Se a posição Z atual for menor que o estacionamento Z, o bico será levantado
        para atingir a altura do parque Z
    -   `P=1`- Não importa a posição Z atual, o bocal será levantado/abaixado para
        atingir a altura do parque Z.
    -   `P=2`- A altura do bico será aumentada pela quantidade de Z-park, mas nunca irá
        acima do limite de altura Z da máquina.
-   `X`_(padrão:`variable_park_x`)_- Coordenada de estacionamento X absoluta.
-   `Y`_(padrão:`variable_park_y`)_- Coordenada de estacionamento Y absoluta.
-   `Z`_(padrão:`variable_park_z`)_- Coordenada de estacionamento Z aplicada de acordo
    para o`P`parâmetro.
-   `LAZY`_(padrão: 1)_- Irá posicionar quaisquer eixos não posicionados, se necessário, e não
    mover qualquer eixo se já estiver na posição inicial e estacionado (mesmo se`P=2`).

> **Observação:**Se uma impressão estiver em andamento, a maior camada impressa mais alta ou
> a posição Z atual será usada como a posição Z atual, para evitar
> colisões com objetos já impressos durante uma impressão sequencial.

#### Compatibilidade com Marlin

-   O`G27`comando é implementado com um padrão`P0`argumento.

### Pausar, retomar, cancelar

#### `PAUSE`

Pausa a impressão atual.

-   `X`_(padrão:`variable_park_x`)_- Coordenada de estacionamento X absoluta.
-   `Y`_(padrão:`variable_park_y`)_- Coordenada de estacionamento Y absoluta.
-   `Z`_(padrão:`variable_park_z`)_- Coordenada de estacionamento relativa Z
-   `E`_(padrão:`5`)_- Comprimento de retração para evitar exsudação.
-   `B`_(padrão:`10`)_- Número de bips a emitir (se`M300`está ativado).

#### `RESUME`

-   `E`_(padrão:`5`)_- Comprimento de retração para evitar exsudação.

#### `CANCEL_PRINT`

Cancela a impressão e executa todas as funções que`PRINT_END`.

#### Compatibilidade com Marlin

-   O`M24`,`M25`,`M600`,`M601`, e`M602`comandos são todos implementados por
    agrupando os comandos acima.

### Imprimir início e fim

#### `PRINT_START`

Configura a impressora antes de iniciar uma impressão (chamada no arquivo de impressão do slicer
iniciar código g). Um alvo`CHAMBER`temperatura pode ser fornecida se um`[heater_generic chamber]`está presente na configuração do klipper.
Se`MESH_MIN`e`MESH_MAX`são fornecidos, então`BED_MESH_CALIBRATE`irá sondar
apenas a área dentro do retângulo especificado e dimensionará o número de
sondas para a densidade apropriada (isso pode reduzir drasticamente os tempos de sonda para
impressões menores).

-   `BED`- Temperatura inicial do aquecedor de cama.
-   `EXTRUDER`- Temperatura inicial do aquecedor da extrusora.
-   `CHAMBER`_(opcional)_- Temperatura inicial do aquecedor da câmara.
-   `MESH_MIN`_(opcional)_- Coordenada mínima x,y da primeira camada.
-   `MESH_MAX`_(opcional)_- Coordenada máxima x,y da primeira camada.
-   `NOZZLE_SIZE`_(padrão: bocal_diâmetro)_- Diâmetro do bico do primário
     extrusora.
-   `LAYERS`_(opcional)_- Número total de camadas na impressão.

Estas são as opções de personalização que você pode adicionar ao seu`[gcode_macro _km_options]`seção para alterar`PRINT_START`comportamento:

-   `variable_start_bed_heat_delay`_(padrão: 2000)_- Este atraso (em
    microssegundos) é usado para permitir que a cama se estabilize depois de atingir seu
    temperatura alvo. Isto está presente para explicar o fato de que o
    os sensores de temperatura para a maioria das camas estão localizados perto do elemento de aquecimento,
    e, portanto, será registrado como estando na temperatura alvo antes da superfície
    da cama é. Para canteiros maiores ou mais grossos você pode querer aumentar esse valor.
    Para camas menores ou mais finas, você pode querer desabilitar isso totalmente configurando
    isso para`0`.

-   `variable_start_bed_heat_overshoot`_(padrão: 2.0)_- Este valor (em graus
    Celsius) é adicionado à temperatura alvo do leito fornecida e usado como
    temperatura alvo inicial ao pré-aquecer a cama. Depois que a cama pré-aquecer
    esta meta se houver um breve atraso antes que a meta final seja definida. Esse
    permite que o leito se estabilize na temperatura final mais rapidamente. Para
    camas menores ou mais finas você pode querer reduzir este valor ou desativá-lo
    inteiramente configurando-o para`0.0`.

-   `variable_start_end_park_y`_(padrão:`max`)_- A posição Y final do
    cabeçote de ferramenta no`PRINT_END`macro, para garantir que o cabeçote da ferramenta esteja fora da
    maneira quando a cama é apresentada para remoção de impressão. Isso pode ser definido como um Y
    coordenada (por ex.`0.0`),`max`usar`stepper_y.position_max`, ou`min`para
    usar`stepper_y.position_min`.

-   `variable_start_extruder_preheat_scale`_(padrão: 0,5)_- Este valor é
    multiplicado pela temperatura alvo da extrusora e o resultado é usado como
    valor de pré-aquecimento para a extrusora enquanto a cama está aquecendo. Isto é feito para
    reduza o vazamento da extrusora enquanto a cama está aquecendo ou sendo sondada. Definir
    para`1.0`para pré-aquecer a extrusora até a temperatura alvo total ou para`0.0`não pré-aquecer a extrusora até que o leito atinja a temperatura.

-   `variable_start_extruder_probing_temp`_(padrão: 0)_- Se definido como diferente de zero
    valor, a extrusora será estabilizada na temperatura fornecida antes da cama
    sondando. Isto é útil para[Corvo TAP](https://github.com/VoronDesign/Voron-Tap), células de carga e outras formas de
    sondagem que usa o bocal diretamente. Quando este valor é fornecido`variable_start_extruder_preheat_scale`é ignorado.

-   `variable_start_level_bed_at_temp`_(padrão: Verdadeiro se`bed_mesh`configurado
    )_- Se for verdade o`PRINT_START`macro será executada[`BED_MESH_CALIBRATE_FAST`](#bed-mesh-improvements)depois que a cama se estabilizou em seu alvo
    temperatura.

-   `variable_start_home_z_at_temp`_(padrão: Verdadeiro se`probe:z_virtual_endstop`configurado)_- Reposiciona o eixo Z assim que a cama atinge a temperatura desejada,
    para contabilizar o movimento durante o aquecimento.

-   `variable_start_clear_adjustments_at_end`_(padrão: Verdadeiro)_- Limpa temporariamente
    ajustes após a impressão ser concluída ou cancelada (por exemplo, taxa de avanço,
    porcentagem de fluxo).

-   `variable_start_purge_clearance`_(padrão: 5,0)_- Distância (em milímetros)
    entre as linhas de purga e a área de impressão (se for`start_purge_length`é
    oferecido).

-   `variable_start_purge_length`_(padrão: 0,0)_- Comprimento do filamento (em
    milímetros) para purgar após a extrusora terminar o aquecimento e antes de
    iniciando a impressão. Para a maioria das configurações`30`é um bom ponto de partida.

-   `variable_start_purge_prime_length`_(padrão: 10,0)_- Comprimento do filamento (em
    milímetros) para preparar a extrusora antes de desenhar as linhas de purga.

-   `variable_start_quad_gantry_level_at_temp`_(padrão: Verdadeiro se`quad_gantry_level`configurado)_- Se for verdade o`PRINT_START`macro será executada`QUAD_GANTRY_LEVEL`depois que o leito se estabilizou na temperatura alvo.

-   `variable_start_random_placement_max`_(padrão: 0)_- Um valor positivo
    especifica a distância +/- nos eixos XY que a impressão pode ser aleatoriamente
    realocado (assumindo que a cama tenha espaço suficiente). Isso pode ajudar a reduzir a cama
    desgaste devido à impressão repetida no mesmo local. Observe que esse recurso
    requer informações adicionais para determinar os limites adequados do
    impressão realocada. Como tal,`START_PRINT`deve ter válido`MESH_MIN`/`MESH_MAX`parâmetros e`MODEL_MIN`/`MODEL_MAX`deve ser definido ou o arquivo de impressão
    deve incluir`EXCLUDE_OBJECT_DEFINE`declarações com`POLYGON`lista que
    definir os limites dos objetos ([ver`exclude_object`Para maiores informações](https://www.klipper3d.org/Exclude_Object.html)).

-   `variable_start_random_placement_padding`_(padrão: 10,0)_- O mínimo
    distância em que a impressão realocada será colocada da borda imprimível do
    cama.

-   `variable_start_try_saved_surface_mesh`_(padrão: Falso)_- Se habilitado e`bed_mesh.profiles`contém uma malha correspondente para o leito atualmente selecionado
    superfície, então a malha será carregada a partir do perfil salvo (e[`BED_MESH_CALIBRATE_FAST`](#bed-mesh-improvements)será ignorado se
    teria sido executado de outra forma).

-   `variable_start_z_tilt_adjust_at_temp`_(padrão: Verdadeiro se`z_tilt`configurado)_- Se for verdade o`PRINT_START`macro será executada`Z_TILT_ADJUST`depois
    o leito estabilizou na temperatura alvo.

Você pode personalizar ainda mais o`PRINT_START`macro declarando sua própria substituição
embrulho. Isso pode ser útil para coisas como carregar perfis de malha/inclinação ou qualquer
outras configurações que talvez precisem ser realizadas antes da impressão.

Aqui está um esqueleto de um`PRINT_START`substituir o wrapper:

    [gcode_macro PRINT_START]
    rename_existing: KM_PRINT_START
    gcode:

      # Put macro code here to run before PRINT_START.

      KM_PRINT_START {rawparams}

      # Put macro code here to run after PRINT_START but before the print gcode

> **Observação:**Você pode usar esse mesmo padrão para agrupar outras macros para
>   leve em conta personalizações específicas para sua impressora. Por exemplo. Se você tem um
>   sonda acoplável que você pode optar por embrulhar`BED_MESH_CALIBRATE`com o
>   comandos de encaixe/desencaixe apropriados.

#### `PRINT_START`Fases

O fatiador recomendado inicia quebras de gcode`PRINT_START`nas fases abaixo.
Esta abordagem permite pausar ou cancelar e inserir gcode personalizado
entre as fases (por exemplo, para definir LEDs de status, implantar/acoplar sondas, carregar filamento).
As fases são descritas na ordem abaixo:

-   `_PRINT_START_PHASE_INIT`- Inicializa o`PRINT_START`configurações e começa
    aquecendo a cama e a câmara.
-   `_PRINT_START_PHASE_PREHEAT`- Estabiliza a cama e (se aplicável) a câmara
    nas temperaturas alvo. Também posiciona os eixos enquanto o aquecimento está em andamento.
-   `_PRINT_START_PHASE_PROBING`- Executa operações de sondagem, incluindo malha
    calibração da cama, nivelamento do pórtico quádruplo, etc.
-   `_PRINT_START_PHASE_EXTRUDER`- Estabiliza a extrusora no alvo
    temperatura.
-   `_PRINT_START_PHASE_PURGE`- Extrusa uma linha de purga na frente da área de impressão.

#### `PRINT_END`

Estaciona o cabeçote de impressão, desliga aquecedores, ventiladores, etc., e executa o estado geral
limpeza no final da impressão (chamada no final da impressão do slicer
código g).

### Imprimir eventos de status

> **Observação:**Este é um recurso totalmente novo que provavelmente evoluirá no próximo
> futuro. Os status provavelmente serão adicionados e/ou removidos conforme eu tiver uma noção melhor
> de como eles estão sendo usados._**Tenha isso em mente ao integrar isso em
> sua própria configuração.**_

Os eventos a seguir são disparados durante o processo de impressão e o`GCODE_ON_PRINT_STATUS`O comando associa gcode personalizado a esses eventos. Esse
gcode personalizado pode ser usado para definir LEDs, emitir bipes ou executar qualquer outro
operações que você pode querer executar em um determinado status no processo de impressão.

-   `ready`- A impressora está pronta para receber um trabalho
-   `filament_load`- Carregando filamento
-   `filament_unload`- Descarregamento de filamento
-   `bed_heating`- Esperando a cama atingir o alvo
-   `chamber_heating`- Esperando que a câmara atinja o alvo
-   `homing`- Homing qualquer eixo
-   `leveling_gantry`- Realização de nivelamento de pórtico quádruplo
-   `calibrating_z`- Executando ajuste de inclinação z
-   `meshing`- Calibrar uma malha de cama
-   `extruder_heating`- Esperando que a extrusora atinja o alvo
-   `purging`- Linha de purga de impressão
-   `printing`- Imprimindo ativamente
-   `pausing`- A impressão está pausada
-   `cancelling`- A impressão está sendo cancelada
-   `completing`- Conclusão da impressão (não dispara em uma impressão cancelada)

#### `GCODE_ON_PRINT_STATUS`

Associa um comando gcode a um status específico e define os parâmetros para
quando e como o evento de status é acionado.

-   `STATUS`- Uma lista separada por vírgulas de eventos de status aos quais este comando está associado
    com. Passando o valor`all`associará o gcode a todos os status.
-   `COMMAND`- O texto do comando.
-   `ARGS`_(padrão:`0`)_- Definido como`1`para permitir a passagem do seguinte status
    argumentos para a macro:`TYPE`,`WHEN`,`LAST_STATUS`, e`NEXT_STATUS`.
    Isso é útil ao chamar uma macro personalizada que determina seu comportamento com base
    sobre os detalhes exatos da transição de estado.
-   `FILTER_ENTER`- Uma lista opcional de status que, se fornecida, impedirá
    o comando seja disparado, a menos que o último status corresponda a um status na lista.
-   `FILTER_LEAVE`- Uma lista opcional de status que, se fornecida, impedirá
    o comando seja disparado, a menos que o próximo status corresponda a um status na lista.
-   `TYPE`_(padrão:`ENTER`)_- Se definido para`ENTER`o comando é executado quando
    inserindo o status especificado. Se definido para`LEAVE`o comando é executado quando
    deixando o status especificado. Se definido para`BOTH`o comando é executado quando
    entrando e saindo. O`LEAVE`comandos são processados ​​primeiro, seguidos por
    o`ENTER`comandos, todos na ordem em que foram originalmente definidos.
-   `WHEN`_(padrão:`PRINTING`)_- Definido como`PRINTING`para disparar o evento de status
    somente ao imprimir,`IDLE`quando não estiver imprimindo e`ALWAYS`para ambos.

#### Exemplo de evento de status de impressão

Abaixo está um exemplo simples de como configurar uma configuração de evento de status. As chamadas para`GCODE_ON_PRINT_STATUS`são colocados no`gcode`do`[gcode_macro _km_options]`seção de configuração, para que eles sejam executados uma vez em
início da impressora. Quando a impressora entra no`ready`indique na inicialização o comando
vai ecoar_` TYPE=LEAVE WHEN=IDLE LAST_STATUS=none NEXT_STATUS=ready`_para o
console, e quando sai do`ready`estado para começar a imprimir, ele ecoará_`TYPE=ENTER WHEN=PRINTING LAST_STATUS=ready NEXT_STATUS=homing`_.

    [gcode_macro _km_options]
    # Any options variables declared here
    gcode:
      GCODE_ON_PRINT_STATUS STATUS=ready COMMAND="STATUS_TEST" ARGS=1 WHEN=ALWAYS TYPE=BOTH

    [gcode_macro status_test]
    variable_extruder: 0
    gcode:
      M118 STATUS_TEST {rawparams}

### Velocidade

Estes são alguns invólucros básicos para os análogos do Klipper a algumas das velocidades do Marlin
comandos relacionados, como aceleração, jerk e avanço linear.

#### Compatibilidade com Marlin

-   O`M201`,`M203`,`M204`, e`M205`comandos são implementados chamando
    Klipper`SET_VELOCITY_LIMIT`comando.
-   O`M900`O comando é implementado chamando o comando do Klipper`SET_PRESSURE_ADVANCE`comando. O`K`fator é escalonado por`variable_pressure_advance_scale`_(padrão:`-1.0`)_. Se o valor da escala for negativo, o`M900`comando não tem
    efeito.

## Configurações opcionais

### Malha de cama

`BED_MESH_CALIBRATE`e`G29`

Substitui o padrão`BED_MESH_CALIBRATE`usar`BED_MESH_CALIBRATE_FAST`em vez disso, e adiciona o`G29`comando.

**_Configuração:_**

    [include klipper-macros/optional/bed_mesh.cfg]

**\*Requisitos:**Um configurado corretamente`bed_mesh`seção.\*

### Menus LCD

Adiciona itens de menu relevantes a um display LCD e melhora alguns itens existentes
funcionalidade. Veja o[costumização](#customization)seção acima para mais
informações sobre como configurar comportamentos específicos.

-   Adicionada confirmação para cancelar a impressão ou desabilitar steppers durante um
    imprimir.
-   Várias alterações no menu de temperatura:
    -   Até 10 filamentos e suas temperaturas correspondentes podem ser definidos via`variable_menu_temperature`.
    -   Controles de temperatura por câmara de filamento estão disponíveis se um`[heater_generic chamber]`seção está configurada.
    -   Os comandos de resfriamento são movidos para o menu de temperatura de nível superior.
-   Os comandos de carregamento do filamento são substituídos por macros que usam os comprimentos
    e velocidades especificadas em`variable_load_length`e`variable_load_speed`,
    que inclui uma fase de escorva no final da carga (controlada via`variable_load_priming_length`e`variable_load_priming_speed`).
-   [Superfície da cama](#bed-surface)o gerenciamento é integrado à configuração e ajuste
    menus.
-   O menu do cartão SD foi simplificado para modos de impressão e não impressão.
-   O menu de configuração inclui desligamento do host, reinicialização do host, velocidade e controles de fluxo.
-   Você pode ocultar os menus do Octoprint ou do cartão SD se não os usar
    (através da`variable_menu_show_octoprint`e`variable_menu_show_sdcard`,
    respectivamente).

**_Configuração:_**

    [include klipper-macros/optional/lcd_menus.cfg]

**\*Requisitos:**Um configurado corretamente`display`seção.\*
