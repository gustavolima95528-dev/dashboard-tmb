# Como hospedar o Dashboard TMR no Vercel

O dashboard é um site estático (`index.html`). O Vercel serve ele por HTTPS,
o que faz a conexão com o Google Sheets funcionar ao vivo (a planilha já está
pública). Os CSVs da pasta ficam como fallback automático.

Arquivos que precisam ir para o deploy (todos já estão nesta pasta):
- `index.html`  ← o dashboard
- `vercel.json` ← configuração (não precisa mexer)
- `Base dashboard TMR - Base Meta.csv`
- `Base dashboard TMR - Base leads.csv`
- `Base dashboard TMR - Base seguidores (1).csv`

---

## Opção A — Vercel CLI (mais rápido)

No terminal, dentro da pasta `Dashboard TMR`:

```bash
npm i -g vercel        # instala o Vercel (só na primeira vez)
cd "caminho/para/Dashboard TMR"
vercel                 # faz login e sobe uma prévia
vercel --prod          # publica a versão de produção
```

Na primeira vez ele pede:
- login (e-mail / GitHub / Google)
- "Set up and deploy?" → **Y**
- "Which scope?" → sua conta
- "Link to existing project?" → **N**
- nome do projeto → ex: `dashboard-tmr`
- diretório do código → **.** (ponto, a pasta atual)

No fim ele mostra a URL, algo como `https://dashboard-tmr.vercel.app`.

---

## Opção B — GitHub + painel do Vercel (recomendada para atualizar fácil)

1. Crie um repositório no GitHub e suba esta pasta (os 5 arquivos acima).
2. Entre em https://vercel.com → **Add New… → Project**.
3. **Import** o repositório do GitHub.
4. Framework Preset: **Other** · Build Command: *(vazio)* · Output Directory: *(vazio)*.
5. **Deploy**.

Depois disso, todo `git push` atualiza o site automaticamente.

---

## Como os dados se atualizam

- Ao abrir/atualizar o dashboard, ele lê a planilha pública via `gviz` em tempo real.
- Botão **↻ Atualizar** força uma nova leitura.
- O selo no cabeçalho mostra a fonte: **● Google Sheets** (ao vivo) ou **● CSV local** (fallback).
- Não precisa republicar no Vercel quando a planilha muda — só quando mudar o `index.html`.

## Importante
- A planilha `1OdHqcet8LxvhDNMn23PvPVUsxKU0OtLb7Ufn2udOG3E` precisa continuar
  com acesso **"Qualquer pessoa com o link → Leitor"**. Se voltar a ser privada,
  o dashboard cai no CSV local (dados congelados na data do arquivo).
