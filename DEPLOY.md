# Deploy do zero + sincronização

Guia completo, do nada até o app funcionando no celular e no PC com os dados compartilhados.

---

## Parte 1 — Criar o repositório no GitHub

1. Acesse <https://github.com/new>.
2. **Repository name:** `studyhub`
3. **Public** — obrigatório para o GitHub Pages funcionar em conta grátis.
   Isso é seguro: o código não contém nenhum dado seu. Suas faltas ficam no navegador
   e no Gist secreto; o token nunca entra no repositório.
4. **Não** marque "Add a README", "Add .gitignore" nem licença. O repositório precisa
   nascer vazio, senão o primeiro `push` dá conflito.
5. **Create repository**.

Na tela seguinte, copie a URL que aparece (algo como
`https://github.com/SEU-USUARIO/studyhub.git`).

---

## Parte 2 — Enviar os arquivos

Abra o terminal na pasta do projeto e rode, uma linha de cada vez:

```bash
cd "C:\Users\marco\OneDrive\Documentos\usp\studyhub"
git init
git add .
git commit -m "studyhub: controle de frequencia"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/studyhub.git
git push -u origin main
```

Troque `SEU-USUARIO` pelo seu usuário do GitHub.

No `git push` vai abrir uma janela do navegador pedindo login no GitHub
(é o Git Credential Manager, que já vem com o Git para Windows). Autorize —
isso acontece só na primeira vez, depois fica salvo.

**Se der erro:**

| Erro | Causa | Solução |
|---|---|---|
| `remote origin already exists` | rodou `git remote add` duas vezes | `git remote set-url origin <URL>` |
| `failed to push some refs` | o repositório foi criado com README | `git pull --rebase origin main` e repita o push |
| `Authentication failed` | login cancelado | rode o push de novo e complete no navegador |

---

## Parte 3 — Ligar o GitHub Pages

1. No repositório: aba **Settings** (engrenagem, no topo).
2. Menu lateral esquerdo: **Pages**.
3. **Source:** `Deploy from a branch`
4. **Branch:** `main` · pasta `/ (root)` → **Save**.
5. Espere ~1 minuto e recarregue a página. Vai aparecer no topo:
   *"Your site is live at `https://SEU-USUARIO.github.io/studyhub/`"*

Dois endereços passam a existir:

| | |
|---|---|
| Hub (lista de ferramentas) | `https://SEU-USUARIO.github.io/studyhub/` |
| Frequência | `https://SEU-USUARIO.github.io/studyhub/frequencia/` |

---

## Parte 4 — Gerar o token do GitHub

O token é o que permite a página ler e escrever no Gist onde ficam seus dados.

1. Acesse <https://github.com/settings/tokens/new?scopes=gist&description=Frequencia>
   (o link já vem com o escopo certo marcado).
2. **Expiration:** escolha `Custom` e ponha uma data depois do fim do semestre —
   ou `No expiration` se preferir não mexer nisso de novo.
3. **Confira que só `gist` está marcado.** Nenhum outro escopo é necessário.
   Com só esse escopo, o token não alcança seus repositórios.
4. **Generate token** e copie o valor (`ghp_...`).
   **Ele só aparece uma vez** — se fechar a página sem copiar, gere outro.

---

## Parte 5 — Conectar o PC

1. Abra `https://SEU-USUARIO.github.io/studyhub/frequencia/`
2. Aba **Ajustes** (a última, embaixo).
3. Cole o token no campo **Token do GitHub**.
4. Deixe o campo **ID do Gist** vazio e clique em **Criar**.
5. Vai aparecer um alerta com o ID do gist (32 caracteres). **Anote esse ID** —
   você vai precisar dele no celular. Ele não é secreto, pode mandar por onde quiser.
6. Clique em **Conectar**.
7. Confira o indicador no canto superior direito: deve dizer `sincronizado HH:MM`.

Agora cadastre suas matérias e as datas do semestre normalmente.

---

## Parte 6 — Conectar o celular

1. Abra `https://SEU-USUARIO.github.io/studyhub/frequencia/` no celular.
   Use a URL da Frequência, não a do hub — o ícone vai abrir direto no controle de
   faltas, que é o que você quer com o celular na mão. (Se preferir o menu, aponte
   para o hub; nada impede ter os dois ícones.)
2. Adicione à tela de início:
   - **iPhone (Safari):** compartilhar → *Adicionar à Tela de Início*
   - **Android (Chrome):** menu ⋮ → *Adicionar à tela inicial*
3. Abra pelo ícone → **Ajustes**.
4. **Gere um segundo token direto no celular**, pelo mesmo link da Parte 4.

   > Não mande o token do PC por WhatsApp/e-mail para si mesmo. É uma credencial;
   > deixá-la numa conversa é criar uma cópia que você não controla. Gerar um token
   > por aparelho é grátis, leva 30 segundos, e permite revogar só o do celular
   > se você perder o aparelho.

5. Cole esse token e **o mesmo ID de gist** da Parte 5.
6. **Conectar**.

Pronto. Marque uma falta no celular, volte para a aba do PC, e ela aparece —
a página puxa da nuvem toda vez que você volta para ela.

---

## Parte 7 — Atualizar o app depois

Sempre que você mexer em qualquer arquivo do projeto — ou adicionar uma ferramenta nova:

```bash
cd "C:\Users\marco\OneDrive\Documentos\usp\studyhub"
git add .
git commit -m "descricao do que mudou"
git push
```

O Pages republica em ~1 minuto. Se o navegador insistir na versão antiga,
force o recarregamento com `Ctrl+Shift+R` (no celular, feche e reabra o app).

Seus dados **não** são afetados por atualizações do código — eles vivem no
`localStorage` e no Gist, não no repositório.

---

## Onde cada coisa mora

| O quê | Onde | Público? |
|---|---|---|
| Código da página | repositório `studyhub` | sim (e tudo bem) |
| Suas faltas e matérias | Gist secreto + `localStorage` | não listado, mas quem tiver a URL lê |
| Token | só no `localStorage` de cada aparelho | nunca sai daí, exceto para `api.github.com` |

Para revogar um token: **Settings → Developer settings → Personal access tokens →
Tokens (classic)** → `Delete` no token correspondente. A página do aparelho revogado
passa a mostrar `erro: token inválido ou revogado`, e os dados dele continuam salvos
localmente.
