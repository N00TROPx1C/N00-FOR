# N00-FOR

[![Versão](https://img.shields.io/badge/vers%C3%A3o-2.0.0-blue.svg)](https://github.com/N00TROPx1C/N00-FOR)

> Script de coleta de artefatos forenses em hosts Windows via PowerShell

---

## Índice

- [Sobre](#sobre)
- [Funcionalidades](#funcionalidades)
- [Pré-requisitos](#pré-requisitos)
- [Instalação e Uso](#instalação-e-uso)
  - [Opções](#opções)
- [Estrutura de Saída](#estrutura-de-saída)
- [Ferramentas Utilizadas](#ferramentas-utilizadas)
- [Nota sobre Antivírus](#nota-sobre-antivírus)
- [Changelog](CHANGELOG.md)
- [Contribuições](#contribuições)
- [Licença](#licença)
- [Autor](#autor)

---

## Sobre

O **N00-FOR** é um script em PowerShell (versão 5.1 ou superior) voltado para coleta
automatizada de artefatos relevantes para investigações forenses em sistemas Windows.

Projetado para ser executado a partir de mídia removível (pendrive/HD externo) em
máquinas potencialmente comprometidas, o script:

1. coleta artefatos voláteis e não voláteis em estrutura padronizada;
2. gera um **manifest.json** com o SHA256 de cada artefato (cadeia de custódia);
3. empacota tudo em um **arquivo ZIP** pronto para envio ao analista;
4. deixa o **dump de memória RAM** em pasta separada (fora do zip, pelo tamanho).

## Funcionalidades

- **Coleta Full**: dump de RAM via Magnet RAM Capture + todos os artefatos +
  hashes de tarefas agendadas e processos com conexão ativa + varredura Hollows Hunter.
- **Coleta Fast**: artefatos principais, sem dump de RAM e sem etapas demoradas.
- **Modo CLI**: parâmetros de linha de comando para uso profissional (sem menu).
- **Integridade**: SHA256 de todos os artefatos + hash do ZIP final.
- **Saída nomeada**: pasta com hostname + timestamp (sem colisão entre coletas).

## Pré-requisitos

1. PowerShell 5.1 ou superior com política de execução ajustada:

```
Set-ExecutionPolicy Bypass -Scope Process
```

2. Permissões de Administrador (o script se auto-eleva se necessário).
3. Recomenda-se mídia removível para execução (pendrive, HD externo etc.).
   **Nunca execute a partir do disco do sistema da máquina investigada.**

## Instalação e Uso

1. Clone ou baixe este repositório:

```
git clone https://github.com/N00TROPx1C/N00-FOR.git
```

2. Copie o `N00-FOR.ps1` para a mídia removível e execute:

```
powershell.exe -ExecutionPolicy Bypass -File .\N00-FOR.ps1
```

### Opções

| Parâmetro | Descrição |
|---|---|
| `-Mode Full` | Coleta completa (padrão do menu: opção 1) |
| `-Mode Fast` | Coleta rápida, sem etapas demoradas (menu: opção 2) |
| `-NoRam` | Pula o dump de memória RAM mesmo no modo Full |
| `-NoZip` | Não gera o ZIP final (mantém apenas a pasta) |
| `-Out <path>` | Diretório raiz de saída (padrão: diretório atual) |

Exemplos:

```
powershell.exe -ExecutionPolicy Bypass -File .\N00-FOR.ps1 -Mode Fast
powershell.exe -ExecutionPolicy Bypass -File .\N00-FOR.ps1 -Mode Full -Out D:\
powershell.exe -ExecutionPolicy Bypass -File .\N00-FOR.ps1 -Mode Full -NoRam -NoZip
```

Sem parâmetros, o script exibe o menu interativo.

## Estrutura de Saída

Após a execução, os resultados ficam em `<destino>\N00-FOR_<HOST>_<data>`:

- **evidence\**
  - `system\` — systeminfo, processos, serviços (com caminho do binário), discos, hotfixes, adaptadores
  - `network\` — conexões TCP, cache DNS, compartilhamentos SMB, portas de escuta + hashes
  - `users\` — usuários locais, histórico PowerShell (PSReadLine), pastas Startup por usuário
  - `persistence\` — autoruns, tarefas agendadas + hashes dos executáveis
  - `evtx\` — logs de eventos do Windows
  - `amcache\` — Amcache.hve (Program Execution) para análise offline
  - `hunter\` — resultados do Hollows Hunter (modo Full)
  - `manifest.json` — inventário com SHA256 de todos os artefatos
- **RAM\** — dump de memória (`.mem`) — **fora do ZIP**, envie por canal próprio
- **N00-FOR_run.log** — log de execução do script

E ao lado da pasta:

- `N00-FOR_<HOST>_<data>.zip` — **envie este arquivo ao analista**
- `N00-FOR_<HOST>_<data>.zip.sha256` — integridade do ZIP

## Ferramentas Utilizadas

| Ferramenta | Uso | Versão embutida |
|---|---|---|
| [Magnet RAM Capture](https://www.magnetforensics.com/resources/magnet-ram-capture/) | Dump de memória RAM | 1.0.0.0 |
| [Hollows Hunter](https://github.com/hasherezade/hollows_hunter) | Varredura de memória de processos (injeção/hollowing) | 0.4.0.0 |
| [AmCacheParser](https://github.com/EricZimmerman/AmcacheParser) (Eric Zimmerman) | Parse do Amcache — executado **offline no laboratório do analista** | — |

As ferramentas embutidas são armazenadas no script em **gzip + base64**
(extraídas em tempo de execução). O Amcache é copiado cru (`Amcache.hve`) e
parseado depois, no laboratório, com ferramentas atualizadas.

## Nota sobre Antivírus

Ferramentas de resposta a incidentes **são sinalizadas por antivírus por design**
(extraem binários em memória, dumpam RAM e varrem processos). Antes de usar o
N00-FOR em um ambiente com AV ativo, faça **whitelist da mídia removível** ou
pausa temporária do agente — isso é esperado e não indica que o script é malicioso.
Prefira sempre executar a partir de mídia externa de confiança.

## Contribuições

Contribuições são bem-vindas! Abra issues para sugestões ou pull requests para
correções e novas funcionalidades.

## Licença

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

## Autor

Feito por: **N00TROPx1C**
LinkedIn: [viniciusth-mello](https://linkedin.com/in/viniciusth-mello)
