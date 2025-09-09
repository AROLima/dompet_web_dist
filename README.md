# DomPet Web Distribution

Este repositório/pasta contém **artefatos gerados** pelo comando de build do Flutter Web. Não é o código-fonte da aplicação.

## Origem do Build
- Framework: Flutter (web)
- URL base (produção): https://dompet-backend.onrender.com
- Comando usado (produção):
  flutter build web --release --dart-define=BASE_URL=https://dompet-backend.onrender.com --dart-define=FLAVOR=prod
- Script auxiliar: `scripts/build_web_prod.ps1`
- Metadados: `version.txt` (BASE_URL, FLAVOR, timestamp) e `version.json`.

## Estrutura Principal
- `index.html` – Shell da aplicação.
- `main.dart.js` – Código compilado minificado.
- `flutter_bootstrap.js` / `flutter.js` – bootstrap runtime Flutter.
- `flutter_service_worker.js` – Service Worker responsável por cache offline.
- `assets/` – Manifests, fontes, imagens, shaders.
- `canvaskit/` – Binários WASM para renderização (CanvasKit / Skwasm).
- `version.json` / `.last_build_id` – Metadados do build.
- `version.txt` – Variáveis simples (para troubleshooting rápido).

## Rebuild / Atualização (Produção)
1. No projeto fonte Flutter:
   flutter pub get
   ./scripts/build_web_prod.ps1
2. Substitua o conteúdo desta pasta pelos novos arquivos de `build/web` (preserve `.git` e README se for repositório separado).
3. Commit & push.
4. Invalide cache CDN (se houver) e force atualização do PWA (ver abaixo).

## Forçar atualização PWA / Cache SW
- Usuário: abrir app, atualizar (Ctrl+Shift+R) ou nas DevTools > Application > Unregister.
- Server-side: alterar algo em `flutter_service_worker.js` ou garantir novo hash de assets (o build já faz isso se arquivos mudarem).
- Se trocar só BASE_URL mas assets idênticos, considere adicionar um comentário com timestamp em `index.html` para mudar hash.

## Segurança
- Nada sensível deve ser incluído; tudo aqui é público.
- BASE_URL pública aponta para API já protegida por autenticação JWT.

## Boas Práticas
- Automatizar deploy (CI) copiando `build/web` para hosting.
- Adicionar cabeçalho `Cache-Control: no-cache` apenas para `index.html` (não para assets com hash) para facilitar updates.

## Licença
Distribuição gerada da aplicação DomPet. Ver licença no repositório principal do código-fonte.
