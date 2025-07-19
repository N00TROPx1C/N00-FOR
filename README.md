# N00-FOR

[![Versão](https://img.shields.io/badge/version-0.1-blue.svg)](https://github.com/N00TROPx1C/N00-FOR)

> Script de coleta de artefatos forenses em hosts Windows via PowerShell

---

## Índice

* [Sobre](#sobre)
* [Funcionalidades](#funcionalidades)
* [Pré-requisitos](#pré-requisitos)
* [Instalação e Uso](#instalação-e-uso)

  * [Opções](#opções)
* [Estrutura de Saída](#estrutura-de-saída)
* [Ferramentas Utilizadas](#ferramentas-utilizadas)
* [Contribuições](#contribuições)
* [Licença](#licença)
* [Autor](#autor)

---

## Sobre

O **N00-FOR** é um script em PowerShell (versão 5.1 ou superior) voltado para coleta automatizada de artefatos relevantes para investigações forenses em sistemas Windows. Ideal para uso a partir de mídia removível, reduzindo ruídos durante a captura de memória.

## Funcionalidades

* **Coleta Completa**: Dump de memória RAM via Magnet RAM Capture, seguido de varredura de diversos artefatos.
* **Coleta Rápida**: Execução simplificada, finaliza em menos de 1 minuto.
* **Ajuda**: Informações de uso diretamente no console.

## Pré-requisitos

1. PowerShell 5.1 ou superior com política de execução ajustada:

   ```powershell
   Set-ExecutionPolicy Bypass -Scope Process
   ```
2. Permissões de Administrador.
3. Recomenda-se mídia removível para execução (pendrive, HD externo etc.).

## Instalação e Uso

1. Clone ou baixe este repositório:

   ```powershell
   git clone https://github.com/N00TROPx1C/N00-FOR.git
   ```
2. Na pasta do script, execute:

   ```powershell
   powershell.exe -ExecutionPolicy Bypass -File .\N00-FOR.ps1
   ```

### Opções

Ao iniciar, o script exibirá um menu com quatro opções:

| Opção | Descrição                                |
| :---: | ---------------------------------------- |
|   1   | Coleta completa (mais demorada)          |
|   2   | Coleta rápida (fast) — conclusão < 1 min |
|   3   | Sair                                     |
|   4   | Ajuda                                    |

---

## Estrutura de Saída

Após a execução, os resultados serão salvos em pastas e arquivos na pasta `%TEMP%` do usuário:

* **appAmCache/**

  * `Amcache_DeviceContainers.csv`
  * `Amcache_DevicePnps.csv`
  * `Amcache_DriveBinaries.csv`
  * `Amcache_DriverPackages.csv`
  * `Amcache_ShortCuts.csv`
  * `Amcache_UnassociatedFileEntries.csv`
* **LogsEvt/**

  * Todos os arquivos de log de eventos do Windows (*.evtx*)
* **%User%/**

  * Histórico de execução do PowerShell
  * Conteúdo da pasta de inicialização do usuário (shel\:startup)
* **N00-HUNTER-RESULTS/**

  * Saída do Hollow Hunter (EDR scanning)
* `Autoruns.txt`        — Chaves de registro de inicialização automática
* `DiscosFisicos.txt`  — Listagem de discos físicos
* `DNSCACHE.txt`       — Cache de DNS
* `Hotfixes.txt`       — Atualizações instaladas
* `listen_ports.txt` e `listen_ports_process.txt` — Conexões de rede e hashes
* `Local_users.txt`    — Usuários locais
* `N00-NetCon.txt`     — Resultado do netstat
* `netadapter.txt`     — Adaptadores de rede
* `Processos_em_execução.txt` — Processos ativos
* `SCHEDULETASKS.txt` e `scheduletaskhashes.txt` — Tarefas agendadas e hashes
* `Services.txt`       — Serviços do Windows
* `SMBSHARE.txt`       — Compartilhamentos de rede

---

## Ferramentas Utilizadas

* [Magnet RAM Capture](https://github.com/Velocidex/WinPmem)
* [AmCacheParser](https://github.com/EricZimmerman/AmcacheParser)
* [Hollows Hunter](https://github.com/hasherezade/hollows_hunter)

---

## Contribuições

Contribuições são bem-vindas! Abra issues para sugestões ou pull requests para correções e novas funcionalidades.

---

## Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

---

## Autor

Feito por: **N00TROPx1C**
LinkedIn: [viniciusth-mello](https://linkedin.com/in/viniciusth-mello)
