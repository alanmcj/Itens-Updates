# Itens-Updates

**Pacotes de atualização do Programa de Inventário — somente binários.**

Este repositório **não contém código-fonte** e **não é open source**.  
Serve apenas para publicar **Releases** com instaladores e atualizações prontas para uso.

O código-fonte do programa fica em um repositório **privado** e **não está aqui**.

---

## Downloads

| O quê | Link |
| --- | --- |
| Página da última release | https://github.com/alanmcj/Itens-Updates/releases/latest |
| Pacote desktop (Linux x86_64) | https://github.com/alanmcj/Itens-Updates/releases/latest/download/programa-de-inventario-linux-x86_64.tar.gz |
| Gerador de capa (opcional) | https://github.com/alanmcj/Itens-Updates/releases/latest/download/gerador-capa-linux-x86_64.tar.gz |
| Coletor Android APK (opcional) | https://github.com/alanmcj/Itens-Updates/releases/latest/download/coletor-android-release.apk |

Confira sempre o arquivo `SHA256SUMS` da mesma release (quando publicado).

---

## Assets esperados (nomes exatos)

| Arquivo | Obrigatório | Descrição |
| --- | --- | --- |
| `programa-de-inventario-linux-x86_64.tar.gz` | sim | Pacote desktop Itens (binário + runtime PostgreSQL portátil + scripts de abertura/atualização) |
| `gerador-capa-linux-x86_64.tar.gz` | opcional | GeradorCapa (ferramenta auxiliar) |
| `coletor-android-release.apk` | opcional | APK do coletor Android |
| `coletor-app.json` | opcional | Metadados do APK (versão/sha) para atualização automática |
| `SHA256SUMS` | recomendado | Checksums SHA-256 dos arquivos da release |
| `README-pacote.md` | opcional | Notas rápidas do pacote |

**Não publicamos** código-fonte (`.cpp`, `.h`, `.kt`, pastas `src/`, projeto Android completo), banco (`pgdata`), dumps, backups de cliente, chaves ou o histórico `.git` do monorepo.

---

## Instalação nova (PC)

```bash
tar -xzf programa-de-inventario-linux-x86_64.tar.gz
cd PROGRAMA_DE_INVENTARIO
bash instalar_programa.sh
```

Depois use o **atalho** da área de trabalho. A instalação oficial fica em:

`~/.local/share/Itens/app`

---

## Atualização (sem tocar no banco)

O banco de dados **não vem no pacote**. Os dados ficam em:

`~/.local/share/Itens`  
(ex.: `pgdata`, backups, logs)

### Pelo menu / atalho (recomendado)

1. Baixe o `.tar.gz` da [última release](https://github.com/alanmcj/Itens-Updates/releases/latest).
2. No menu **INICIAR** → **Atualizar**, ou:

```bash
tar -xzf programa-de-inventario-linux-x86_64.tar.gz
bash PROGRAMA_DE_INVENTARIO/atualizar_facil.sh
```

### Manual

```bash
bash atualizar_programa.sh /caminho/PROGRAMA_DE_INVENTARIO_NOVO [/opt/inventario]
```

O segundo argumento (destino) é opcional. Se omitido, o script usa a instalação oficial já registrada.

---

## Suporte

Em caso de falha na abertura ou atualização, envie ao suporte:

`~/.local/share/Itens/registro.log`

---

## Licença / propriedade

Software proprietário. Este repositório é só um canal de distribuição de binários.  
Não há licença open-source associada ao conteúdo das Releases.
