# 📖 Bíblia JEM — Guia Completo

## 📁 Estrutura de Arquivos

```
Bibliajm/
├── index.html                  ← App principal
├── sw.js                       ← Service Worker (offline)
├── manifest.json               ← PWA manifest (instalação)
├── firebase.json               ← Configuração Firebase Hosting
├── icon-192.png                ← Ícone do app (você fornece)
├── icon-512.png                ← Ícone do app (você fornece)
├── .github/
│   └── workflows/
│       └── deploy.yml          ← Deploy automático
└── .well-known/
    └── assetlinks.json         ← Link Android TWA
```

---

## 🚀 Fluxo: Editar → Push → Deploy Automático

```
Você edita um arquivo
        ↓
Faz commit + push no GitHub (pelo celular ou web)
        ↓
GitHub Actions executa automaticamente
        ↓
Firebase Hosting atualizado em ~40 segundos
        ↓
bibliajem.web.app atualizado ✅
```

---

## 📱 Como editar e fazer push SEM notebook

### Opção 1 — GitHub.com direto pelo navegador (mais fácil)

1. Acesse **github.com/alfastore-apolo/Bibliajm**
2. Clique no arquivo que quer editar (ex: `index.html`)
3. Clique no ✏️ (lápis) no canto superior direito
4. Faça a edição
5. Role até o final → **"Commit changes"**
6. Escreva uma mensagem → clique **"Commit changes"**
7. O deploy começa automaticamente! Aguarde ~40s.

### Opção 2 — GitHub Mobile (app Android)

1. Instale o app **GitHub** na Play Store
2. Faça login com sua conta alfastore-apolo
3. Navegue até o repositório Bibliajm
4. Edite arquivos e faça commits direto pelo app

### Opção 3 — GitHub Codespaces (editor online completo)

1. No repositório, clique em **"Code"** → **"Codespaces"**
2. Clique em **"Create codespace on main"**
3. Abre um VS Code completo no navegador — grátis até 60h/mês
4. Edite, salve, faça `git push` pelo terminal integrado

---

## 🔧 Configuração inicial (só precisa fazer uma vez)

### 1. Corrigir o submódulo fantasma

No terminal (Git Bash no notebook) ou no Codespaces:

```bash
git rm --cached Bibliajm -f
git rm .gitmodules -f 2>/dev/null || true
git commit -m "fix: remove submodule fantasma"
git push
```

### 2. Configurar o secret FIREBASE_SERVICE_ACCOUNT

Já deve estar configurado (deploy do Firebase funciona). Se não:

1. Firebase Console → Configurações do projeto → Contas de serviço
2. Gerar nova chave privada → baixar JSON
3. GitHub repo → Settings → Secrets → Actions
4. New secret: `FIREBASE_SERVICE_ACCOUNT` → colar o conteúdo do JSON

---

## 📲 Transformar em APK (Android)

### Método 1 — PWABuilder (sem notebook, grátis)

1. Acesse **pwabuilder.com**
2. Cole a URL: `https://bibliajem.web.app`
3. Clique em **Start**
4. Vá em **Package for stores** → **Android**
5. Baixe o APK gerado
6. Instale direto no Android (habilite "Fontes desconhecidas")

> 💡 Para publicar na Play Store, você precisa de uma conta de desenvolvedor ($25 único).

### Método 2 — Bubblewrap (TWA — mais profissional)

O app vira um TWA (Trusted Web Activity) — abre o site como app nativo sem barra do browser.

No Codespaces ou terminal:

```bash
npm install -g @bubblewrap/cli
bubblewrap init --manifest=https://bibliajem.web.app/manifest.json
bubblewrap build
```

Gera o `app-release-signed.apk` na pasta `android/app/build/outputs/apk/release/`.

### Depois de gerar o APK com Bubblewrap:

1. Pegue o SHA-256 do keystore gerado:
```bash
keytool -list -v -keystore android.keystore
```

2. Cole o SHA-256 no arquivo `.well-known/assetlinks.json`

3. Faça push — o Firebase vai servir esse arquivo automaticamente

4. Reinstale o APK — agora a barra do browser some! 🎉

---

## 🌐 URLs do projeto

| O que | URL |
|---|---|
| App ao vivo | https://bibliajem.web.app |
| Repositório | https://github.com/alfastore-apolo/Bibliajm |
| Actions (deploys) | https://github.com/alfastore-apolo/Bibliajm/actions |
| Firebase Console | https://console.firebase.google.com/project/bibliajem |

---

## ✅ Checklist para publicar na Play Store

- [ ] APK gerado com Bubblewrap
- [ ] SHA-256 no assetlinks.json
- [ ] Ícones 192px e 512px no repo
- [ ] manifest.json com `display: standalone`
- [ ] Conta Google Play Developer ($25)
- [ ] Screenshots do app (mínimo 2)
- [ ] Texto de descrição do app
