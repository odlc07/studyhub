# frequency

Ferramentas da faculdade, em páginas estáticas hospedadas no GitHub Pages.
Sem build, sem dependências, sem servidor — cada ferramenta é um `index.html` na
própria pasta.

```
studyhub/
├── index.html          página inicial que lista as ferramentas
├── frequencia/
│   └── index.html      controle de faltas do semestre
├── README.md
└── DEPLOY.md           passo a passo do deploy e da sincronização
```

Endereços depois do deploy:

| | |
|---|---|
| Hub | `https://SEU-USUARIO.github.io/studyhub/` |
| Frequência | `https://SEU-USUARIO.github.io/studyhub/frequencia/` |

## Adicionar uma ferramenta nova

1. Crie uma pasta com um `index.html` dentro (ex.: `notas/index.html`).
2. Copie um bloco `<a class="tool">` no [index.html](index.html) da raiz apontando
   para ela.
3. `git add . && git commit -m "nova ferramenta" && git push`.

Não há roteador nem configuração: o GitHub Pages serve `pasta/` como
`pasta/index.html` automaticamente.

---

# Frequência

Controle de faltas por matéria, com cálculo de quantas aulas você ainda pode perder.

## Como funciona a conta

- Você cadastra cada matéria com os **dias da semana** e **quantas aulas** tem em cada dia.
- A página varre o intervalo do semestre, pula os feriados cadastrados e soma o total de aulas.
- Limite de faltas = `floor(total × (100 − presença mínima) / 100)`. Com o padrão da USP
  (70%), são 30% do total.
- O número grande em cada card é **quantas aulas você ainda pode perder**.

Se a secretaria informar o total exato de aulas, dá para preencher o campo
*Total de aulas no semestre* na matéria e ele passa a valer no lugar do cálculo por datas.

## Sincronizar entre celular e PC

Por padrão os dados ficam só no navegador em que você usou. Para sincronizar, a página
guarda o estado num **Gist secreto** da sua própria conta do GitHub. O passo a passo
está no [DEPLOY.md](DEPLOY.md), partes 4 a 6.

Depois de conectado, a página envia sozinha 1,5 s após qualquer alteração e puxa da
nuvem toda vez que você volta para a aba ou reabre o app. O indicador no canto superior
direito mostra o estado (`sincronizado`, `enviando…`, `erro: …`).

### Como o conflito é resolvido

Não é "o último que salvar vence" — os dois lados são **fundidos**:

- faltas e matérias são unidas por `id`, então nada registrado num aparelho é apagado
  pelo outro;
- em caso de edição do mesmo item nos dois lados, vence a versão mais recente;
- exclusões viajam como *lápides* (`deleted`), para que apagar algo no celular também
  apague no PC — em vez de o item voltar do nada na próxima sincronização;
- lápides com mais de 180 dias são descartadas para o arquivo não crescer sem fim.

### Segurança

O token fica no `localStorage` do navegador e só é enviado para `api.github.com`.
Com escopo `gist` ele não alcança seus repositórios. Se perder o celular, revogue em
**Settings → Developer settings → Personal access tokens**, e pronto.

O Gist é *secreto*, não criptografado: não aparece no seu perfil nem em buscas, mas
quem tiver a URL consegue ler.

## Backup manual

Em **Ajustes → Backup**: *Baixar backup* / *Copiar* / *Importar*. Serve como cópia de
segurança mesmo com a sincronização ligada — importar **substitui** os dados atuais,
ao contrário da sincronização, que funde.
