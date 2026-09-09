# Checklist de release (Itens-Updates)

Canal público **somente de binários**. O código-fonte permanece no repo privado.

## Pode ir na Release

- `programa-de-inventario-linux-x86_64.tar.gz`
- `coletor-android-release.apk` + `coletor-app.json` (opcional)
- `SHA256SUMS`
- `README-pacote.md` / notas da versão (texto)

## NÃO pode ir na Release

- Código-fonte: `.cpp`, `.h`, `.hpp`, `.kt`, `.java`, `CMakeLists.txt` de build interno, pastas `src/`, `ColetorAndroid/` completo
- Banco / dados de cliente: `pgdata`, `*.dump`, `backups/`, dumps SQL com dados reais
- Segredos: chaves, `.env`, tokens, certificados privados
- Histórico do monorepo: pasta `.git/`
- Artefatos de desenvolvimento: `build/`, objetos intermediários, logs locais com caminhos sensíveis

## Gerar SHA256SUMS

No diretório dos artefatos (ex.: `.release_dist/` do monorepo privado):

```bash
cd .release_dist
sha256sum \
  programa-de-inventario-linux-x86_64.tar.gz \
  coletor-android-release.apk \
  2>/dev/null > SHA256SUMS
cat SHA256SUMS
```

## Inspecionar o tar.gz (obrigatório antes de publicar)

```bash
TGZ=programa-de-inventario-linux-x86_64.tar.gz

# Deve listar PROGRAMA_DE_INVENTARIO/… sem src/ de aplicação nem pgdata
tar -tzf "$TGZ" | head -50

# Falhar se aparecer coisa proibida
if tar -tzf "$TGZ" | grep -E '(^|/)(\.git/|pgdata/|src/|.*\.(cpp|h|hpp|kt)$)'; then
  echo "ABORTADO: conteúdo proibido encontrado no pacote"
  exit 1
fi

echo "Inspeção OK"
```

(Use também o `scripts/validar_pacote.sh` do monorepo privado, se disponível.)

## Publicar neste repo

Exemplo (rode a partir da pasta dos artefatos):

```bash
TAG=v1.4.30
NOTES="Programa de Inventário ${TAG} — somente binários (sem código-fonte)."

gh release create "$TAG" \
  --repo alanmcj/Itens-Updates \
  --title "Programa de Inventário ${TAG}" \
  --notes "$NOTES" \
  programa-de-inventario-linux-x86_64.tar.gz \
  coletor-android-release.apk \
  coletor-app.json \
  SHA256SUMS
```

Omita assets opcionais que não existirem.  
**Não** use `--repo alanmcj/ProgramaDeInventario` para este canal de downloads.

## Depois da publicação

- Abra https://github.com/alanmcj/Itens-Updates/releases/latest e confira os nomes dos arquivos.
- Teste o link direto:
  https://github.com/alanmcj/Itens-Updates/releases/latest/download/programa-de-inventario-linux-x86_64.tar.gz
