# Guia de Configuração e Manutenção do Perfil (`domingossss`)

Este documento descreve a organização do repositório de perfil do GitHub, como ativar a automação da animação de contribuições (Snake) e como atualizar ou expandir cada recurso com segurança.

---

## 1. Estrutura de Arquivos

```text
domingossss/
├── README.md                              # Apresentação principal exibida no perfil do GitHub
├── SETUP.md                               # Este guia de configuração e manutenção
├── .github/
│   └── workflows/
│       └── snake.yml                      # Workflow diário e manual da animação de contribuições
└── assets/
    ├── banner.svg                         # Banner principal (tema escuro)
    ├── banner-light.svg                   # Banner principal (tema claro)
    ├── typing-dark.svg                    # Animação de digitação em SVG nativo (tema escuro)
    ├── typing-light.svg                   # Animação de digitação em SVG nativo (tema claro)
    ├── divider-dark.svg                   # Divisor visual entre seções (tema escuro)
    ├── divider-light.svg                  # Divisor visual entre seções (tema claro)
    ├── footer.svg                         # Rodapé visual (tema escuro)
    ├── footer-light.svg                   # Rodapé visual (tema claro)
    ├── icons/                             # Chips e ícones padronizados de tecnologias e contato
    └── projects/                          # Prévias vetoriais dos projetos (dark e light)
        ├── clinica-ikigai-dark.svg
        ├── clinica-ikigai-light.svg
        ├── vittaia-dark.svg
        └── vittaia-light.svg
```

---

## 2. Publicação Inicial e Ativação do Workflow Snake

A animação Snake é gerada pelo GitHub Actions e publicada automaticamente na branch dedicada `output`, mantendo o histórico da branch `main` limpo.

### Passo a passo após o primeiro push

1. **Verifique as permissões do GitHub Actions no repositório:**
   - Acesse **Settings -> Actions -> General** no repositório `domingossss/domingossss`.
   - Em **Workflow permissions**, confirme que o workflow tem permissão para gravar conteúdos (o próprio `snake.yml` já declara `permissions: contents: write` com escopo mínimo, mas as políticas da conta devem permitir ações com permissão de escrita).
2. **Publique os arquivos na branch `main`:**
   - O arquivo `.github/workflows/snake.yml` possui um gatilho `push` filtrado exclusivamente para alterações no próprio workflow. No primeiro push para `main`, ele será disparado automaticamente e criará a branch `output` com:
     - `github-contribution-grid-snake-dark.svg`
     - `github-contribution-grid-snake.svg`
3. **Execução manual (quando desejar atualizar na hora):**
   - Abra a aba **Actions** do repositório.
   - Selecione **Generate Snake Contribution Graph**.
   - Clique em **Run workflow** -> **Run workflow**.
4. **Atualização automática:**
   - O workflow está agendado via `cron: "0 6 * * *"` (diariamente às 06:00 UTC / 03:00 BRT).

### Segurança do workflow

- Utiliza exclusivamente o `GITHUB_TOKEN` efêmero fornecido pelo GitHub Actions; não exige Personal Access Token (PAT) nem secrets manuais.
- Opera com permissão mínima (`contents: write`).
- Utiliza actions oficiais/mantidas em runtime Node 24 (`Platane/snk/svg-only@v3` e `crazy-max/ghaction-github-pages@v5`).

---

## 3. Como Personalizar os Recursos Locais

### Identidade visual e paleta

Os SVGs seguem a mesma paleta técnica em versões escura e clara:

| Função | Tema Escuro | Tema Claro |
| :--- | :--- | :--- |
| Fundo principal | `#0D1117` | `#F8FAFC` / `#FFFFFF` |
| Superfície / card | `#111827` / `#161B22` | `#FFFFFF` / `#F1F5F9` |
| Borda técnica | `#1E293B` | `#CBD5E1` |
| Azul destaque | `#3B82F6` | `#2563EB` |
| Ciano destaque | `#22D3EE` | `#0891B2` |
| Texto primário | `#F8FAFC` | `#0F172A` |
| Texto secundário | `#94A3B8` | `#475569` |

### Banner e rodapé

- Edite `assets/banner.svg` e `assets/banner-light.svg` diretamente em qualquer editor de código para atualizar formação, subtítulo ou elementos gráficos.
- Edite `assets/footer.svg` e `assets/footer-light.svg` para manter o rodapé alinhado ao cabeçalho.

### Typing Animation (sem dependência externa)

- A animação de digitação é renderizada localmente em `assets/typing-dark.svg` e `assets/typing-light.svg` usando SVG SMIL (`<textPath>` + `<animate>`), sem depender de serviços externos.
- Para alterar as frases, substitua o texto dentro dos cinco elementos `<textPath>` nos dois arquivos. Caso altere a quantidade de frases, ajuste o encadeamento `begin="0s;aN.end"` no primeiro `<animate>`.

### Tecnologias e ícones (`assets/icons/`)

- Cada chip possui altura padronizada de `30px` (`viewBox="0 0 <largura> 30"`), fundo `#111827`, borda `#1E293B` e filete lateral em `#3B82F6` (software) ou `#22D3EE` (automação industrial / dados).
- Para adicionar uma nova tecnologia:
  1. Duplique um arquivo semelhante em `assets/icons/`.
  2. Ajuste o atributo `width`, `viewBox`, `<title>`, `aria-label`, o texto do rótulo e o ícone vetorial dentro de `<g transform="translate(11, 7)">`.
  3. Adicione a tag `<img alt="Nome" src="./assets/icons/novo-arquivo.svg" height="30">` na categoria correspondente do `README.md`.

### Projetos em destaque (`assets/projects/`)

- Cada projeto possui duas prévias vetoriais (`*-dark.svg` e `*-light.svg`) em `880x400`, dimensionadas para leitura nítida em largura total (`width="100%"`) no perfil do GitHub.
- A alternância entre tema escuro e claro no `README.md` utiliza os fragmentos `#gh-dark-mode-only` e `#gh-light-mode-only` nos links (`href`), o que acompanha diretamente o tema configurado no GitHub (`data-color-mode`: `dark`, `light` ou `auto`), mesmo quando o tema do sistema operacional é diferente.
- Se preferir substituir uma prévia vetorial por uma captura de tela real da aplicação no futuro, salve um `.png` ou `.webp` otimizado (recomendado 1280x640, abaixo de 250 KB) em `assets/projects/` e atualize os caminhos `src` no `README.md`.

### Adicionar e-mail ou portfólio futuramente

Quando desejar incluir um e-mail profissional ou site de portfólio na seção **Redes Sociais e Contato**:

1. Crie um botão SVG seguindo o padrão `44px` de altura de `assets/icons/linkedin.svg`.
2. Adicione o link correspondente (`mailto:seu-email@dominio.com` ou `https://seu-portfolio.com`) no bloco centralizado de contato do `README.md`.

---

## 4. Manutenção dos Serviços Externos de Analytics

Os blocos de **GitHub Analytics** utilizam endpoints verificados com suporte simultâneo aos temas claro e escuro do GitHub:

| Componente | Serviço Atual | Observação de Manutenção |
| :--- | :--- | :--- |
| GitHub Stats | `github-readme-stats.vercel.app/api` | Se sofrer limite de requisições públicas, é possível hospedar instância própria gratuita na Vercel a partir de `anuraghazra/github-readme-stats`. |
| Top Languages | `github-readme-stats.vercel.app/api/top-langs/` | Compartilha a mesma instância do GitHub Stats. |
| GitHub Streak | `streak-stats.demolab.com` | Alternativa oficial mantida: `github-readme-streak-stats.herokuapp.com`. |
| Activity Graph | `github-readme-activity-graph.vercel.app/graph` | Mantido em `Ashutosh00710/github-readme-activity-graph`. |
| GitHub Trophies | `github-profile-trophy-beta.vercel.app` | Utiliza `rank=-?` para ocultar categorias sem pontuação. O domínio legado `github-profile-trophy.vercel.app` retornava HTTP 402 durante a configuração; caso o domínio beta oscile, considere fazer deploy próprio de `ryo-ma/github-profile-trophy`. |
| Snake Contribution | Branch `output` gerada via GitHub Actions | Sem dependência de servidor externo em tempo de visualização; servida diretamente pelo GitHub. |

---

## 5. Recomendações de Melhoria nos Repositórios Públicos

Durante a análise inicial do perfil, foram observadas melhorias rápidas que aumentam ainda mais a qualidade percebida por recrutadores:

1. **Corrigir a descrição do repositório `Clinica_Ikigai`:** ajustar a grafia em *About* para *"Sistema web full-stack de gestão clínica desenvolvido no Projeto Integrador do 3º semestre de Gestão da Tecnologia da Informação (FATEC Guaratinguetá)."* e adicionar `topics` como `react`, `typescript`, `nodejs`, `express`, `mysql`, `tailwindcss`.
2. **Corrigir a descrição do repositório `VITTAIA`:** ajustar a grafia em *About* para *"Landing page responsiva desenvolvida para o plano de negócios da marca fictícia VITTAIA (FATEC Guaratinguetá)."* e adicionar `topics` como `html5`, `css3`, `tailwindcss`, `javascript`, `esbuild`, `viacep`.
3. **Fixar (Pin) os dois repositórios no perfil:** na página inicial do GitHub (`github.com/domingossss`), clique em **Customize your pins** e fixe `Clinica_Ikigai` e `VITTAIA` logo abaixo do README.
4. **Atualizar o campo Website/Bio do perfil GitHub:** incluir o link do LinkedIn (`https://www.linkedin.com/in/domingos-bastos`) nas configurações do perfil para reforçar consistência com o README.
