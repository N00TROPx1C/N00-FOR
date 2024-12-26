# N00-FOR
Script de coleta de artefatos em hosts windows via powershell

ooooo......ooo....oooo.......oooo..............o88o.....oooooo....ooooooooo............................................................................................
'888b......'8'..d8P''Y8b...d8P''Y8b...........888.'...d8P....Y8b...888....Y88...........................................................................................
.8.'88b.....8..888....888.888....888.........o888oo..888......888..888....d88'..........................................................................................
.8...'88b...8..888....888.888....888..........888....888......888..888ooo88P'...........................................................................................
.8.....'88b.8..888....888.888....888.8888888..888....888......888..888'88b..............................................................................................
.8.......'888..'88b..d88'.'88b..d88'..........888....'88b....d88'..888..'88b............................................................................................
o8o........'8...'Y8bd8P'...'Y8bd8P'..........o888o....'Y8bood8P'..o888o..o888o.0.1......................................................................................

N00-FOR é um script para powershell com a função de coletar artefatos com relevancia forense para hosts windows(exclusivamente)

Para utilizar o script é recomendável que seja executado por uma midia removivel, se possível.
Ao ser executado ele irá te dar 4 opções.

  1 - Coleta completa                                                                       
  2 - Coleta simples (fast)                                                                 
  3 - Sair                                                                                  
  4 - Ajuda                                                    Feito por: N00TROPx1C

A primeira opção executa uma coleta completa e por consequencia mais demorada.
A segunda opção executa uma coleta simples que é entregue em menos de 1minuto
A terceira opção saí do script
A quarta opção exibe uma breve ajuda (não substitui esse README)

Ao optar pela coleta completa o script irá descarregar no disco (exclusivamente na pasta TEMP do usuário a qual está executando) a ferramenta Magnet RAM Capture, com o nome alterado, evitando detecção por nome.
Após o processo do Magnet ser finalizado ele irá seguir com as coletas, isso ocorre a fim de reduzir o maximo possível os ruídos no DUMP de memória.

O script tambem descarrega algumas outras ferramentas, como AmCacheParsers de Erick Zimmerman a fim de lidar com a impossibilidade de análise do NTUSER.DAT in live.

No final você irá obter um resultado com as seguintes informações.

appAmCcche/

Amcache_DeviceContainers.csv
Amcache_DevicePnps.csv
Amcache_DriveBinaries.csv
Amcache_DriverPackages.csv
Amcache_ShortCuts.csv
Amcache_UnassociatedFileEntries.csv














