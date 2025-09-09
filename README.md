# DomPet Web Distribution

Este repositório/pasta contém **artefatos gerados** pelo comando de build do Flutter Web. Não é o código-fonte da aplicação.

## Origem do Build
- Framework: Flutter (web)
- Comando usado (exemplo):
  flutter build web --release
- Commit origem (frontend app): consulte o hash registrado em `version.json` (campo `app_version` se presente) ou mantenha manualmente abaixo.

## Estrutura Principal
- `index.html` – Shell da aplicação.
- `main.dart.js` – Código compilado minificado.
- `flutter_bootstrap.js` / `flutter.js` – bootstrap runtime Flutter.
- `flutter_service_worker.js` – Service Worker responsável por cache offline.
- `assets/` – Manifests, fontes, imagens, shaders.
- `canvaskit/` – Binários WASM para renderização (CanvasKit / Skwasm).
- `version.json` / `.last_build_id` – Metadados do build.

## Rebuild / Atualização
1. No projeto fonte Flutter:
   flutter clean
   flutter pub get
   flutter build web --release
2. Substitua todo o conteúdo desta pasta pelos novos arquivos de `build/web`.
3. Faça commit/push.
4. (Se hospedagem estática) Invalide/limpe cache CDN se existir.

## Cache & Service Worker
- Após publicar um novo build alguns navegadores podem servir o SW antigo.
- Para garantir atualização: abrir DevTools > Application > Unregister Service Worker e fazer hard reload.
- Opcional: incremente manualmente `cacheName` dentro de `flutter_service_worker.js` se quiser forçar invalidação imediata.

## Segurança
- Não armazene segredos (tokens, chaves) em tempo de build dentro destes arquivos; qualquer pessoa pode baixá-los.
- Variáveis sensíveis devem ser resolvidas via backend (ex: endpoints) ou feature flags públicas.

## Boas Práticas
- Versionar um arquivo `version.txt` com o hash do commit fonte.
- Automatizar pipeline para gerar e publicar esta pasta (CI) evitando edição manual.
- Evitar commits de arquivos grandes obsoletos (remover antes de adicionar novos builds se usar histórico longo).

## Licença
Distribuição gerada da aplicação DomPet. Ver licença no repositório principal do código-fonte.
