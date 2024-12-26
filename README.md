# N00-FOR - Em desenvolvimento
Script de coleta de artefatos em hosts windows via powershell

>>>N00-FOR 0.1<<<

N00-FOR é um script para powershell com a função de coletar artefatos com relevância forense para hosts windows(exclusivamente)

Para utilizar o script é recomendável que seja executado por uma midia removivel, se possível.
Ao ser executado ele irá te dar 4 opções.

  1 - Coleta completa                                                                       
  2 - Coleta simples (fast)                                                                 
  3 - Sair                                                                                  
  4 - Ajuda                                                    

Feito por: N00TROPx1C

A primeira opção executa uma coleta completa e por consequência mais demorada.
A segunda opção executa uma coleta simples que é entregue em menos de 1minuto
A terceira opção saí do script
A quarta opção exibe uma breve ajuda (não substitui esse README)

Ao optar pela coleta completa o script irá descarregar no disco (exclusivamente na pasta TEMP do usuário a qual está executando) a ferramenta Magnet RAM Capture, com o nome alterado, evitando detecção por nome.
Após o processo do Magnet ser finalizado ele irá seguir com as coletas, isso ocorre a fim de reduzir o máximo possível os ruídos no DUMP de memória.

O script também descarrega algumas outras ferramentas, como AmCacheParsers de Erick Zimmerman a fim de lidar com a impossibilidade de análise do NTUSER.DAT in live.

No final você irá obter um resultado com as seguintes informações.

appAmCcche/
|
Amcache_DeviceContainers.csv
Amcache_DevicePnps.csv
Amcache_DriveBinaries.csv
Amcache_DriverPackages.csv
Amcache_ShortCuts.csv
Amcache_UnassociatedFileEntries.csv
#Coletas do AmCacheParses

LogsEvt/
#Todos os logs de eventos do windows

%User%/
#Pode conter: Histórico de execuções do powershell de cada usuário
#E
#Startup folder (shel:startup)

N00-HUNTER-RESULTS/
#Resultados da busca do HollowHunter

Autoruns.txt
#Chaves de registros AUTORUNS

DiscosFisicos.txt
#Discos fisicos associados ao host

DNSCACHE.txt
#Cache de DNS do host

Hotfixes.txt
#hotfixes instalados

listen_ports_process.txt
#Processos que possuem conexão com a internet, e o calculo do hash do path (se houver)

listen_ports.txt
#Lista detalhada de todos os processos que possuem conexão de rede, sem hash.

Local_users.txt
#Lista de usuários locais

N00-NetCon.txt
#Resultado do netstat

netadapter.txt
#adaptadores de rede associado ao host

Process_connection_detail.txt
#Semelhante ao listen_ports_process.txt mas com melhor visibilidade

Processos_em_execução.txt
#Todos os processos atualmente em execução

scheduletaskhashes.txt
#calculo do hash do path que estiver no campo ACTION de uma tarefa agendasa. o formato é:
#PATH DO PROCESSO
#HASH DO PROCESSO 

SCHEDULETASKS.txt
#Todas as tarefas agendadas registradas

Services.txt
#Todos os serviços associados ao host

SMBSHARE.txt
#Todos os compartilhamentos de rede disponivel no host

O script (N00-FOR) está em constante evolução. A medida que é identificado algo novo que pode ser associado a ele, assim será feito.
Você pode sugerir algo novo!

A ultima coleta, atualmente, é a utilização do HOLLOW_HUNTER (https://github.com/hasherezade/hollows_hunter)
Ele é carregado diretamente no disco de uma grande string em base 64.  # Sua ferramenta de EDR pode reclamar


coming soon....

linkedin.com/in/viniciusth-mello












