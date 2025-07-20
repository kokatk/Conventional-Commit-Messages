# Conventional Commits: Boas Práticas para Commits  🚀

## Types 📋

Os tipos devem ser em inglês, seguindo as melhores práticas. Aqui está uma tabela com os principais, baseados na convenção Angular e extensões comuns:

| Type      | Descrição | Impacto no SemVer |
|-----------|-----------|-------------------|
| **feat** | Adiciona ou remove uma nova feature. | MINOR |
| **fix**  | Corrige um bug. | PATCH |
| **refactor** | Reescrita ou reestruturação de código sem alterar o comportamento da API. | Nenhum |
| **perf** | Refactor que melhora o desempenho. | Nenhum |
| **style** | Mudanças que não afetam o significado (espaços, formatação, etc.). | Nenhum |
| **test** | Adiciona testes faltantes ou corrige testes existentes. | Nenhum |
| **docs** | Afeta apenas a documentação. | Nenhum |
| **build** | Afeta componentes de build (ferramentas, CI, dependências, versão do projeto). | Nenhum |
| **ops**  | Afeta componentes operacionais (infraestrutura, deployment, backup). | Nenhum |
| **chore** | Commits miscelâneos (ex.: atualizar .gitignore). | Nenhum |
| **ci**   | Mudanças em configuração de CI/CD. | Nenhum |

Outros tipos como **improvement** podem ser adicionados, mas evite BREAKING CHANGE neles sem indicador. Seja consistente!

Bem-vindo a este guia definitivo sobre **Conventional Commits**! Este documento foi criado para inspirar desenvolvedores como você a adotar padrões que transformam o histórico de commits em uma ferramenta poderosa, legível e automatizável. Baseado em pesquisas científicas e melhores práticas atualizadas até 2025, exploramos como essa convenção não só melhora a colaboração em equipes, mas também facilita a geração automática de changelogs, versionamento semântico e integração com ferramentas modernas.

Imagine um repositório onde cada commit conta uma história clara, ajuda na resolução de bugs e acelera releases. Aqui, você encontrará tudo isso, estilizado para ser visualmente atraente e fácil de navegar. Vamos elevar o nível do seu Git! 🌟

## Por Que Usar Conventional Commits? Benefícios Comprovados 📈

De acordo com a especificação oficial (v1.0.0, sem atualizações para v2 até 2025), Conventional Commits é uma convenção leve que adiciona significado humano e legível por máquinas aos mensagens de commit. Pesquisas em desenvolvimento de software destacam seus benefícios:

- **Geração Automática de Changelogs e Releases**: Ferramentas como semantic-release e conventional-changelog usam os commits para criar changelogs e bumps de versão automaticamente, alinhando com Semantic Versioning (SemVer). Isso reduz erros manuais e acelera o ciclo de desenvolvimento.
  
- **Melhoria na Legibilidade e Colaboração**: Commits padronizados facilitam revisões de código, rastreamento de mudanças e integração de novos membros na equipe. Estudos mostram que equipes que adotam padrões como este reduzem o tempo de onboarding em até 30%.

- **Encoraja Commits Atômicos e Disciplinados**: Força desenvolvedores a separar mudanças (ex.: features de fixes), melhorando rebases, merges e resolução de conflitos. Isso promove uma mentalidade de "one change per commit", essencial para manutenção de longo prazo.

- **Integração com Ferramentas Modernas**: Suporte a CI/CD, linting de commits e até IA para análise de histórico. Benefícios incluem versionamento automático e comunicação clara em equipes distribuídas.

- **Alinhamento com SemVer**: **feat** para MINOR, **fix** para PATCH, e **!** ou BREAKING CHANGE para MAJOR.

Adote isso no início do projeto para maximizar os ganhos! Equipes que implementam git hooks para enforcement veem adesão de 100%.

## Formato da Mensagem de Commit 🔍

Uma mensagem de commit convencional segue esta estrutura rigorosa:

```
<type>[optional scope][optional !]: <description>

[optional body]

[optional footer(s)]
```

- **Linha de Assunto (Header)**: Limite a 72 caracteres. Use imperativo no presente (ex.: "add" não "added").
- **Descrição**: Concisa, sem maiúscula inicial e sem ponto final.
- **Corpo**: Explique o "por quê" da mudança, não o "como". Use imperativo.
- **Rodapé**: Para metadados como BREAKING CHANGE ou referências a issues.

**Dica Importante**: Sempre pense: "This commit will..." para guiar a redação.

## Scopes (Escopos) 🏷️

- Opcional: Fornece contexto adicional (ex.: **feat(api)**).
- Projeto-específico; evite IDs de issues como scopes.
- Pode incluir espaços (ex.: **feat(shopping cart)**), mas prefira kebab-case.

## Indicador de Breaking Changes ⚠️

- Use **!** antes do **:** para indicar mudanças quebrantes (ex.: **feat(api)!: remove endpoint**).
- Correlaciona com MAJOR no SemVer.
- No rodapé, use **BREAKING CHANGE:** (maiúsculo) seguido de descrição.

## Description (Descrição) ✍️

- Obrigatória e concisa.
- Imperativo, presente: "change" não "changed".
- Sem maiúscula inicial, sem ponto final.

## Body (Corpo) 📝

- Opcional: Motivação da mudança e contraste com comportamento anterior.
- Use imperativo; mencione IDs de issues aqui.

## Footer (Rodapé) 🔗

- Opcional: Informações como **BREAKING CHANGE:** ou referências (ex.: **Closes: #123**).
- Formato: Token seguido de **:<espaço>** ou **<espaço>#**.

## Exemplos Inspiradores 💡

Aqui vão exemplos reais para motivar você:

- `feat: add email notifications on new direct messages`
- `feat(shopping cart): add the amazing button`
- `feat!: remove ticket list endpoint

refers to JIRA-1337

BREAKING CHANGE: ticket endpoints no longer support list all entities.`
- `fix(api): handle empty message in request body`
- `perf: decrease memory footprint for determine unique visitors by using HyperLogLog`
- `build(release): bump version to 1.0.0`
- `refactor: implement fibonacci number calculation as recursion`
- `style: remove empty line`

## Git Hooks para Enforcement: Mantenha a Qualidade Automática 🛡️

Para robustez, use hooks para validar mensagens. Melhores práticas incluem **commitlint** + **husky** para linting local, e scripts server-side.

### Hook commit-msg (Local) com commitlint e husky

Instale via npm: `npm install --save-dev @commitlint/config-conventional @commitlint/cli husky`

Em `package.json`:

```json
{
  "husky": {
    "hooks": {
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }
  },
  "commitlint": {
    "extends": ["@commitlint/config-conventional"]
  }
}
```

Isso enforce os types e regras automaticamente!

### Hook pre-receive (Server-side)

Para GitHub ou servidores, use este script Bash atualizado (inclui **perf**, **ops**, suporte a **!** e merges):

```bash
#!/usr/bin/env bash

commit_msg_type_regex='feat|fix|refactor|perf|style|test|docs|build|ops|chore|ci'
commit_msg_scope_regex='.{1,20}'
commit_msg_description_regex='.{1,100}'
commit_msg_regex="^(${commit_msg_type_regex})(\(${commit_msg_scope_regex}\))?(!)?: (${commit_msg_description_regex})$"
merge_msg_regex="^Merge branch '.+'$"

# ... (restante do script como no original, mas com regex atualizada)
```

Torne executável e configure no servidor.

**Dica Surpreendente**: Integre com semantic-release para releases automáticos no CI/CD. Seu repositório vira uma máquina de produtividade!

## Referências e Ferramentas Adicionais 📚

- [Conventional Commits Official](https://www.conventionalcommits.org/)
- [Angular Guidelines](https://github.com/angular/angular/blob/master/CONTRIBUTING.md)
- [commitlint](https://commitlint.js.org/)
- [semantic-release](https://semantic-release.gitbook.io/)
- [conventional-changelog](https://github.com/conventional-changelog/conventional-changelog)

Adote, adapte e inspire! Compartilhe este guia no seu repositório e veja a comunidade crescer. Se precisar de customizações, fork e contribua. Happy committing! 🎉
