# Como hospedar o Dashboard TMB no Vercel

O dashboard é um site estático (`index.html`) que lê os 3 CSVs desta pasta.
O Vercel serve tudo por HTTPS.

## Arquivos que vão para o deploy (todos nesta pasta)

- `index.html`  ← o dashboard (confira: o título deve ser `TMB — Dashboard`)
- `vercel.json`
- `Base dashboard TMB - Base Meta.csv`
- `Base dashboard TMB - Base Google.csv`
- `Base dashboard TMB - Base leads.csv`

> Os arquivos `CONTEXT.md`, `DEPLOY-VERCEL.md` e `RESUMO-*.md` não afetam o site — pode subir ou não, tanto faz.

---

## Caminho recomendado — GitHub + painel do Vercel

### Etapa 1 — Subir os arquivos no GitHub
1. Em **github.com**, faça login. Clique no **+** (canto superior direito) → **New repository**.
2. Nome: `dashboard-tmb` · deixe **Public** · **não** marque "Add a README" · **Create repository**.
3. Na página do repositório vazio, role até a caixa **"Quick setup"** e clique em **"uploading an existing file"**
   (ou vá direto em `https://github.com/SEU-USUARIO/dashboard-tmb/upload/main`).
4. Arraste os **5 arquivos** da pasta **Dashboard TMB** (lista acima).
5. Clique no botão verde **Commit changes**.

### Etapa 2 — Conectar no Vercel
6. Em **vercel.com**, clique **Sign Up / Login** → **Continue with GitHub**.
7. **Add New… → Project** → localize `dashboard-tmb` → **Import**.
8. Deixe o padrão: *Framework Preset* = **Other**, campos de Build/Output **vazios** → **Deploy**.
9. Aguarde ~1 min. A URL final fica tipo `https://dashboard-tmb.vercel.app`.

---

## Atualizar os dados depois

O dashboard lê os **CSVs** do repositório. Para atualizar os números:
1. No GitHub, **Add file → Upload files**.
2. Suba os CSVs novos (mesmo nome → sobrescreve).
3. **Commit changes**. O Vercel republica sozinho em ~1 min.

Não precisa mexer no Vercel a cada atualização — só quando mudar o `index.html`.

---

## ⚠️ Se o site aparecer como a TMR (sem as mudanças)

Isso quase sempre é o **`index.html` errado** (o antigo da TMR) subido por engano.

1. No repositório, clique em `index.html` e olhe a 1ª linha:
   - Certo: `<title>TMB — Dashboard</title>`
   - Errado: `<title>TMR Saúde…`
2. Se estiver errado, **Add file → Upload files** e arraste o `index.html` da pasta **Dashboard TMB**
   (título `TMB — Dashboard`). Mesmo nome → sobrescreve → **Commit changes**.
3. Espere o redeploy e no navegador dê um **hard refresh**: **⌘ + Shift + R** (limpa o cache).

Sinais de que está o dashboard certo (TMB):
- Título da aba: **TMB — Dashboard**
- Barra no topo do Perpétuo com **Consolidado / Meta / Google**
- Aparecem dados de **Google Ads** (a TMR só tinha Meta)

---

## Abrir localmente (sem hospedar)

Ao dar duplo clique, o navegador pode bloquear a leitura dos CSVs por segurança.
Para testar local, rode um servidor simples na pasta:

```bash
cd "Dashboard TMB"
python3 -m http.server 8000
```

E abra `http://localhost:8000`.

---

## Futuro — leitura ao vivo do Google Sheets

Hoje a fonte são os CSVs. Para ligar a leitura **ao vivo** do Google Sheets TMB
(`1eCoQ3W5vMt_D0Hlx8LBhj8lr6cJKqiPD_Ko3dmyDO8Q`) é preciso padronizar as abas da planilha
com os nomes/colunas das bases Meta, Google e Leads — aí reativamos o bloco `fetchSheet` no `index.html`.
Importante: manter os **IDs de UTM como Texto** na planilha (como número o Sheets arredonda >15 dígitos e quebra o matching lead→anúncio).
