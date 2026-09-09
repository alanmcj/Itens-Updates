# Checklist de release (Itens-Updates)

Canal público **somente de binários**. O código-fonte permanece no repo privado.

## Publicação automática (preferida)

Quando uma Release é **publicada** no repositório privado `alanmcj/ProgramaDeInventario`:

1. O workflow **Mirror Itens-Updates** baixa os assets permitidos.
2. Gera `SHA256SUMS`.
3. Cria/atualiza a mesma tag neste repo (`alanmcj/Itens-Updates`).

**Não sobe:** `gerador-capa-linux-x86_64.tar.gz` nem código-fonte.

### Secret no repo privado

Em `alanmcj/ProgramaDeInventario` → Settings → Secrets and variables → Actions:

| Nome | Valor |
|------|--------|
| `ITENS_UPDATES_TOKEN` | PAT fine-grained com **Contents: Read and write** só em `alanmcj/Itens-Updates` |

Sem esse secret o espelho automático falha de propósito.

O script local `scripts/create_release.sh` do monorepo também tenta espelhar ao final
(use `ITENS_SKIP_PUBLIC_MIRROR=1` para pular no PC).

## Pode ir na Release

- `programa-de-inventario-linux-x86_64.tar.gz`
- `coletor-android-release.apk` + `coletor-app.json` (opcional)
- `SHA256SUMS`
- `README-pacote.md` / notas da versão (texto)

## NÃO pode ir na Release

- GeradorCapa (`gerador-capa-linux-x86_64.tar.gz`)
- Código-fonte: `.cpp`, `.h`, `.hpp`, `.kt`, `.java`, pastas `src/`, `ColetorAndroid/` completo
- Banco / dados de cliente: `pgdata`, `*.dump`, `backups/`
- Segredos: chaves, `.env`, tokens, certificados privados
- Histórico do monorepo: pasta `.git/`

## Publicação manual (fallback)

No monorepo privado, com assets em `.release_dist/`:

```bash
./scripts/publish_itens_updates.sh v1.4.32 .release_dist
```

Ou com `gh` direto (sem GeradorCapa):

```bash
TAG=v1.4.32
cd .release_dist
sha256sum programa-de-inventario-linux-x86_64.tar.gz coletor-android-release.apk 2>/dev/null > SHA256SUMS

gh release create "$TAG" \
  --repo alanmcj/Itens-Updates \
  --title "Programa de Inventário ${TAG}" \
  --notes "Somente binários — sem código-fonte." \
  programa-de-inventario-linux-x86_64.tar.gz \
  coletor-android-release.apk \
  coletor-app.json \
  SHA256SUMS
```

## Inspecionar o tar.gz (antes de publicar à mão)

```bash
TGZ=programa-de-inventario-linux-x86_64.tar.gz
tar -tzf "$TGZ" | head -50
if tar -tzf "$TGZ" | grep -E '(^|/)(\.git/|pgdata/|src/|.*\.(cpp|h|hpp|kt)$)'; then
  echo "ABORTADO: conteúdo proibido encontrado no pacote"
  exit 1
fi
echo "Inspeção OK"
```

## Depois da publicação

- https://github.com/alanmcj/Itens-Updates/releases/latest
- Download direto:
  https://github.com/alanmcj/Itens-Updates/releases/latest/download/programa-de-inventario-linux-x86_64.tar.gz
