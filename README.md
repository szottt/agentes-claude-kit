# Agentes para o Claude Code

Este kit é a versão genérica dos agentes que uso no meu app. São 17 especialistas em quatro times. Os de design,
UX e segurança **só leem e opinam**; os de homologação rodam verificações no ambiente local. Quem implementa é
sempre o Claude principal, que junta as respostas.

## Os times

### 🎨 Creative Studio
| Agente | Papel |
|---|---|
| `creative-director` | Lidera. Interpreta o pedido, cria **3 conceitos realmente diferentes**, recomenda um e diz quem chamar. |
| `art-director` | Transforma o conceito em composição, tipografia, cor, hierarquia e medidas. |
| `brand-designer` | Levanta o que a marca já é, aponta inconsistências e cuida do mascote. |
| `social-card-designer` | Peças compartilháveis e de conquista (stories, posts, medalhas, ranking). |

### 📱 UX/UI
| Agente | Papel |
|---|---|
| `ux-lead` | Fluxo, navegação, organização das telas e hierarquia da informação. |
| `mobile-ux` | Toque, uso com uma mão, teclado, botão Voltar, PWA × app, offline. |
| `visual-reviewer` | Revisão final de cor, tipografia, contraste, acessibilidade e temas. |

### 🔒 Segurança
| Agente | Papel |
|---|---|
| `security-lead` | Entra **antes** de funcionalidade com dados pessoais ou permissões. Modela ameaças e prioriza riscos. |
| `database-security` | Banco: RLS, grants, funções, `security definer`, SQL Injection, isolamento entre usuários. Escrito para Postgres/Supabase. |
| `appsec` | Front: XSS, login, sessão, admin, cabeçalhos, armazenamento local, segredos expostos. |
| `security-testing` | Acha lacunas nos testes de segurança e desenha cenários negativos. |

### ✅ Homologação (antes de publicar)
| Agente | Papel |
|---|---|
| `homolog-seguranca` | Confere segurança e LGPD. É o portão de publicação. |
| `homolog-exploratorio` | Usa o app como cada tipo de usuário, seguindo roteiros e fugindo deles. |
| `homolog-visual` | Prints em várias larguras, navegadores e temas, comparados com a rodada anterior. |
| `homolog-desempenho` | Abertura em rede lenta, offline, peso e travadas. |
| `homolog-lojas` | Checklists da Play Store e da App Store com as regras atuais. |
| `homolog-apk` | Testes no Android: Voltar, gestos, teclado, notch, offline. |

## Passo 1 — Instalar

No Claude Code:

```
/plugin marketplace add szottt/agentes-claude-kit
/plugin install design@agentes-claude-kit
```

Troque `design` por `ux`, `seguranca` ou `homologacao` para os outros pacotes. Instale só os que fizerem sentido.
Sugestão para começar: `ux` + `design`. Se o app tem login e banco, adicione `seguranca`.

**Para adaptar ao seu projeto**, os agentes precisam estar dentro dele: copie os arquivos de
`plugins/<pacote>/agents/` para `.claude/agents/` do seu projeto e siga o Passo 2. Instalado como plugin, o agente
recebe as atualizações, mas fica com os marcadores genéricos.

## Passo 2 — Adaptar ao seu projeto (o Claude faz isso por você)

Os arquivos têm marcadores entre colchetes, como `[PROJETO]` e `[PASTA DE MIGRATIONS]`. Abra o Claude Code no
projeto e cole:

> Em `.claude/agents/` há agentes com marcadores entre colchetes. Investigue meu projeto (código, banco, cores,
> fontes, telas, testes) e substitua cada marcador por informação real, citando os arquivos. O que não achar, me
> pergunte em vez de inventar. Tire as partes que não se aplicam (mascote, temas, app Android etc.). Depois crie
> `docs/identidade-visual.md` com o que a marca é hoje e aponte os agentes de design para ele.

Os agentes de **homologação** precisam de um checklist por área (`seguranca.md`, `visual.md` etc.). Peça ao Claude:
*"Crie os checklists de homologação para os agentes homolog-\*, com base no meu projeto."*

## Passo 3 — Usar

- Peça pelo nome: *"Use o ux-lead para revisar a tela de cadastro."*
- **Tarefa pequena:** chame só o especialista certo. Não precisa do time inteiro.
- Criativo grande: comece pelo `creative-director`. Mudança no banco: `database-security`.

## O que funcionou para mim

- **Somente leitura** (`tools: Read, Grep, Glob`) deixa os consultores focados em pensar, e a implementação fica
  num lugar só.
- **"Leia o código e cite arquivo:linha"** evita opinião genérica.
- **"3 conceitos realmente diferentes"** tira o design do óbvio.
- **Confiança (Confirmada/Provável/Hipótese)** nos de segurança impede alarme falso.
- **Problema, ganho, complexidade (P/M/G) e prioridade** em cada recomendação facilita decidir.
- Modelos: `opus` onde precisa de mais raciocínio (diretor de criação, banco), `sonnet` no geral e `haiku` nas
  checagens repetitivas. Ajuste como preferir.
