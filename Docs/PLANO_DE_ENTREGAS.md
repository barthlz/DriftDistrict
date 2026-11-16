# Plano de entregas — Drift District

O objetivo é **ter um commit de marco para cada uma das quatro entregas**, além dos pequenos commits de trabalho que forem necessários. Commits só devem indicar funcionalidades realmente implementadas e testadas. Git não é requisito de pontuação nas imagens do professor; neste projeto será nossa rotina de organização.

## Antes de tudo — GDD inicial

- [ ] Criar repositório `drift-district-unity` no GitHub.
- [ ] Criar projeto Unity 3D na raiz clonada do repositório.
- [ ] Colocar este GDD em `Docs/GDD.md` e a `.gitignore` na raiz.
- [ ] Confirmar versão da Unity e nomes da equipe no GDD.
- [ ] Fazer primeiro commit: `docs: criar GDD v0.1 do Drift District`.

## Entrega 1 — 12/11 — v0.1 — Prototipagem

- [ ] Cena `IndustrialDistrict` com terreno, rua/pista provisória, paredes e cones.
- [ ] Carro simples com Rigidbody, Collider, controle por teclado e câmera.
- [ ] Pelo menos uma interação/colisão ou demonstração simples da física.
- [ ] Criar `MainMenu` se houver tempo, mas não deixar a mecânica principal incompleta.
- [ ] Atualizar GDD com aquilo que realmente funciona e registrar limitações.
- [ ] Commit de marco: `feat: entrega 1 prototipo dirigivel`.
- [ ] Tag opcional: `entrega-1-prototipo`.

## Entrega 2 — 19/11 — v0.2 — Trecho integrado

- [ ] Física e colisões afetam gameplay.
- [ ] Prefab reutilizado em pelo menos dois locais.
- [ ] Música, evento sonoro e Animator com AnimationClip.
- [ ] Canvas com pontuação e outro estado (velocidade/tempo).
- [ ] Menu inicia a cena do jogo; condições iniciais de vitória e derrota.
- [ ] GDD atualizado.
- [ ] Commit de marco: `feat: entrega 2 integracao do drift e interface`.
- [ ] Tag opcional: `entrega-2-integracao`.

## Entrega 3 — 26/11 — v0.3 — Beta

- [ ] Todo o fluxo de exploração livre, desafios A e B, vitória/derrota e reinício.
- [ ] Duas situações de desafio com regras efetivamente diferentes.
- [ ] Teste com outra pessoa; documentar bugs e correções.
- [ ] Manter os requisitos completos, sem adicionar escopo dispensável.
- [ ] Commit de marco: `feat: entrega 3 beta com desafios completos`.
- [ ] Tag opcional: `entrega-3-beta`.

## Entrega 4 — 03/12 — v1.0 — Final

- [ ] Projeto Unity abre e roda no computador da apresentação.
- [ ] Sem erro bloqueante de Console; estado de jogo e reinício testados.
- [ ] Dois integrantes ou mais conseguem demonstrar scripts e explicar como funcionam; idealmente todos dominam sua parte.
- [ ] GDD, créditos, fontes e instruções atualizados.
- [ ] Projeto-fonte preservado (`Assets`, `Packages`, `ProjectSettings` e arquivos `.meta`).
- [ ] Commit de marco: `release: entrega 4 versao final apresentacao`.
- [ ] Tag opcional: `v1.0-final`.

## Alertas

- A data de 12/11/2026 é anterior ao início deste GDD em 16/11; não alterar a data de commits.
- A avaliação pelas imagens soma 20 verificações de 0,5 = 10 pontos. Git é recomendado pelo professor, e adotado aqui como compromisso da equipe.
- Não basta GDD no commit da Entrega 1: é preciso ter protótipo em Unity executável.
- Não colocar somente `.exe` no GitHub no lugar do projeto-fonte.
