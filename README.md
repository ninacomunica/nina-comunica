# Nina Comunica – UBS Nina Liberato

Site/PWA informativo da UBS Nina Liberato, Caruaru – PE.
Desenvolvido por estudantes de Medicina da UFPE-CAA no módulo PIESC 2026.

---

## Estrutura dos arquivos

```
nina-comunica/
├── index.html               ← página principal (todo o site está aqui)
├── vercel.json              ← configuração do Vercel
├── README.md                ← este guia
└── public/
    ├── manifest.json        ← configuração do PWA (app no celular)
    ├── sw.js                ← service worker (funcionamento offline)
    └── icons/               ← ícones do app (você adiciona)
        ├── icon-192.png     ← 192 × 192 px
        └── icon-512.png     ← 512 × 512 px
```

---

## PARTE 1 — Subir o site no GitHub

### Passo 1: Criar conta no GitHub
1. Acesse **github.com** e clique em **Sign up**.
2. Preencha e-mail, senha e nome de usuário. Confirme o e-mail.

### Passo 2: Criar o repositório
1. Após entrar, clique no botão verde **New** (canto superior esquerdo).
2. Em **Repository name**, digite: `nina-comunica`
3. Deixe marcado como **Public**.
4. Clique em **Create repository**.

### Passo 3: Fazer upload dos arquivos
1. Na página do repositório recém-criado, clique em **uploading an existing file** (link no centro da tela).
2. Arraste **todos os arquivos e a pasta `public`** para a área de upload.
   - `index.html`
   - `vercel.json`
   - `README.md`
   - pasta `public/` (com `manifest.json`, `sw.js` e `icons/`)
3. No campo **Commit changes**, deixe a mensagem padrão.
4. Clique em **Commit changes** (botão verde).

> ✅ Seus arquivos já estão no GitHub.

---

## PARTE 2 — Publicar no Vercel (hospedagem gratuita)

### Passo 1: Criar conta no Vercel
1. Acesse **vercel.com** e clique em **Sign Up**.
2. Escolha **Continue with GitHub** — isso conecta as duas contas automaticamente.

### Passo 2: Importar o repositório
1. No painel do Vercel, clique em **Add New → Project**.
2. Na lista de repositórios do GitHub, localize **nina-comunica** e clique em **Import**.
3. Não altere nenhuma configuração — o Vercel detecta automaticamente que é um site estático.
4. Clique em **Deploy**.

> Em menos de 1 minuto o site estará no ar em um endereço `.vercel.app` (ex: `nina-comunica.vercel.app`).

### Passo 3: Testar o site
- Acesse o link `.vercel.app` gerado e verifique se tudo está funcionando.

---

## PARTE 3 — Conectar o domínio ninacomunica.com

### Passo 1: Adicionar o domínio no Vercel
1. No painel do Vercel, clique no projeto **nina-comunica**.
2. Vá em **Settings → Domains**.
3. No campo, digite `ninacomunica.com` e clique em **Add**.
4. O Vercel vai mostrar dois registros DNS que precisam ser configurados:
   - Um registro **A** apontando para o IP do Vercel
   - Um registro **CNAME** para `www`

### Passo 2: Configurar o DNS no seu registrador
1. Acesse o painel de onde você comprou o domínio (ex: Registro.br, GoDaddy, Hostinger).
2. Localize a seção **Zona DNS** ou **Gerenciar DNS**.
3. Adicione os dois registros exatamente como o Vercel indicou.
4. Salve.

> ⏳ A propagação do DNS pode levar de 5 minutos até 24 horas.
> O HTTPS (cadeado verde) é ativado automaticamente pelo Vercel, sem custo.

---

## PARTE 4 — Atualizar o site depois

Sempre que quiser alterar algo no `index.html`:

1. Abra o repositório no **github.com/seu-usuario/nina-comunica**.
2. Clique no arquivo `index.html`.
3. Clique no ícone de **lápis** (editar).
4. Faça a alteração direto no navegador.
5. Clique em **Commit changes**.

> O Vercel detecta a mudança automaticamente e atualiza o site em 1 a 2 minutos.

---

## PARTE 5 — Ícones do app (PWA)

Para o site aparecer com o ícone correto quando salvo no celular:

1. Use o ícone do Nina Comunica nos tamanhos:
   - `icon-192.png` → 192 × 192 px
   - `icon-512.png` → 512 × 512 px
2. O site **realfavicongenerator.net** gera os dois tamanhos automaticamente a partir de qualquer imagem.
3. Coloque os arquivos gerados dentro da pasta `public/icons/` no GitHub.

---

## Custo estimado

| Item | Custo |
|------|-------|
| GitHub | Gratuito |
| Vercel (hospedagem + HTTPS) | Gratuito |
| Domínio ninacomunica.com | ~R$ 40–70 / ano |
| **Total recorrente** | **~R$ 40–70 / ano** |
