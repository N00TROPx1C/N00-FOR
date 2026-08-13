# Changelog

Todas as mudanças notáveis do **N00-FOR** serão documentadas neste arquivo.
Formato baseado em [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/).

## [2.0.1] — 2026-08-13

Correções descobertas no teste real do modo Full (Windows Server 2019).

### Corrigido

- **Fatal no modo Full**: variável de loop `$pid` colidia com a variável
  automática read-only `$PID` do PowerShell (derrubava o script na etapa de
  processos com conexão ativa). Renomeada para `$procPid`.
- **Hollows Hunter sem evidência**: em sistemas limpos o HH não gera dumps nem
  relatório em disco (a saída fica no console). O script agora executa com
  `/log` e captura o stdout em `N00-Hunter_scan.log` — a varredura fica
  documentada mesmo sem achados.
- Parâmetro não utilizado (`-DestBase`) e variável morta removidos;
  catches vazios documentados (higiene via PSScriptAnalyzer — 0 erros).

## [2.0.0] — 2026-08-13

Reescrita completa com foco em confiabilidade de entrega: o objetivo do script é
rodar na máquina do cliente e produzir **um único ZIP verificável** para envio ao
analista.

### Adicionado

- **ZIP final** (`System.IO.Compression.ZipFile`) com hash `.sha256` ao lado —
  o cliente envia apenas o `.zip`; a integridade é conferida pelo `.sha256`.
- **manifest.json** dentro do zip: inventário de todos os artefatos com
  caminho, tamanho e SHA256 (cadeia de custódia).
- **Saída nomeada e única**: `N00-FOR_<HOST>_<yyyyMMdd-HHmmss>\` — coletas
  repetidas não colidem mais.
- **Modo CLI** para uso profissional: `-Mode Full|Fast`, `-NoRam`, `-NoZip`, `-Out`.
- **Copy-From-VSS**: cópia de arquivos travados (ex: `Amcache.hve`) via Volume
  Shadow Copy + symlink — os métodos comuns (Copy-Item, robocopy /b, esentutl)
  falham quando o sistema segura o handle.
- Coleta de `systeminfo`, ações das tarefas agendadas (tabela separada),
  serviços com `PathName` (`Win32_Service`), autoruns expandido (13 chaves).
- **Timeouts** em todos os waits (Hollows Hunter travado não trava mais o script).
- Tratamento de erro por coleta (uma falha não derruba as demais) e fallbacks
  para Windows Server 2012 R2 (`net user`, `netstat`, `ipconfig`).
- Log de execução (`N00-FOR_run.log`) em todos os modos.
- Verificação de espaço livre no destino.

### Alterado

- **AmCacheParser removido do script**: o `Amcache.hve` é copiado cru e o parse
  passa a ser feito **offline, no laboratório do analista** (ferramenta sempre
  atualizada, sem exigir .NET 4.7.2 na máquina do cliente).
- **xcopy embutido e cópia de cmd.exe eliminados**: substituídos por
  `Copy-Item -Recurse` nativo e `Get-NetTCPConnection` + `Get-Process`.
- **Binários embutidos em gzip + base64** (antes: base64 puro): o script caiu
  de **8,3 MB para 0,92 MB** e a assinatura "MZ em base64" deixou de existir.
- Dump de RAM fica **fora do ZIP** (pasta `RAM\`), pelo tamanho do arquivo.
- Estrutura de saída organizada por categoria (`system\`, `network\`, `users\`,
  `persistence\`, `evtx\`, `amcache\`, `hunter\`).
- Encoding padronizado (UTF-8) e nomes de arquivo ASCII.

### Corrigido

- **Opção 2 (Fast) não entregava nada**: o `exit` prematuro deixava a coleta em
  `%TEMP%` sem mover/zipar.
- **Coletas em paralelo sem espera**: o script movia a pasta com arquivos ainda
  sendo escritos (coleta incompleta silenciosa).
- `AmcacheParser` rodava sem `-Wait` (CSVs podiam sair incompletos).
- Nome de transcript inválido no Windows (`:` e `\` no formato de data).
- Ambiguidade no loop de tarefas agendadas (tasks de mesmo nome em paths
  diferentes; re-query desnecessária).
- Typos: `appAmCche`, `Listen_porsts_process.txt` e divergências README × script.
- Variáveis mortas e encoding inconsistente entre coletas.

### Testado

Validado em **Windows Server 2019** (PS 5.1): 357 artefatos coletados, zip de
45 MB, SHA256 do zip conferido, `Amcache.hve` travado copiado via VSS.

## [0.1.2] — 2024 (legado)

Versão original do script: coleta em `%TEMP%\N00-IR` com menu interativo,
binários (Magnet RAM Capture, AmCacheParser, Hollows Hunter, xcopy) embutidos
em base64 puro.

---

## Como utilizar

### Requisitos

- PowerShell 5.1+ (Windows 8.1 / Server 2012 R2 ou superior)
- Executar como **Administrador** (o script se auto-eleva)
- Executar de **mídia removível** — nunca do disco do sistema investigado

### Uso interativo (menu)

```powershell
powershell.exe -ExecutionPolicy Bypass -File .\N00-FOR.ps1
```

Opções do menu: `1` coleta completa, `2` coleta rápida, `3` sair, `4` ajuda.

### Uso via linha de comando

```powershell
# Coleta rápida (sem dump de RAM, sem Hollows Hunter)
powershell.exe -ExecutionPolicy Bypass -File .\N00-FOR.ps1 -Mode Fast

# Coleta completa para um destino específico
powershell.exe -ExecutionPolicy Bypass -File .\N00-FOR.ps1 -Mode Full -Out D:\

# Completa, mas pulando o dump de RAM (ex: dump já feito por outro meio)
powershell.exe -ExecutionPolicy Bypass -File .\N00-FOR.ps1 -Mode Full -NoRam

# Sem gerar o ZIP (apenas a pasta de evidências)
powershell.exe -ExecutionPolicy Bypass -File .\N00-FOR.ps1 -Mode Fast -NoZip
```

### Saída e entrega

```text
<destino>\N00-FOR_<HOST>_<data>\
├── evidence\          ← artefatos por categoria + manifest.json
├── RAM\               ← dump de memória (fora do ZIP)
└── N00-FOR_run.log    ← log de execução

<destino>\N00-FOR_<HOST>_<data>.zip        ← ENVIE AO ANALISTA
<destino>\N00-FOR_<HOST>_<data>.zip.sha256 ← verificação de integridade
```

O cliente envia apenas o `.zip`; o dump de RAM (`.mem`) é enviado por canal
próprio (arquivo grande, contém dados sensíveis). Conferir o zip:

```powershell
Get-FileHash .\N00-FOR_<HOST>_<data>.zip -Algorithm SHA256
# comparar com o conteúdo do .zip.sha256
```

### Antivírus

Ferramentas de resposta a incidentes são sinalizadas por AV **por design**
(extraem binários, dumpam RAM, varrem processos). Faça whitelist da mídia
removível antes de executar — isso é esperado, não indica arquivo malicioso.
