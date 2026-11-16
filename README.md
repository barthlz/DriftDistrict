# Drift District: Industrial Drift

Projeto acadêmico: jogo 3D de drift em Unity e C#, com pequeno mundo aberto, duas provas e pontuação.

> **Estado atual:** documentação inicial. O projeto Unity e as mecânicas ainda serão construídos. Não apresentar este repositório como protótipo pronto.

- [GDD inicial](Docs/GDD.md)
- [Plano de entregas](Docs/PLANO_DE_ENTREGAS.md)

## Como iniciar o projeto com segurança

1. Pelo **Unity Hub**, crie um projeto **3D** chamado `DriftDistrict` em uma pasta local, por exemplo `C:\UnityProjetos\DriftDistrict`.
2. Abra a pasta criada e confirme que ela contém `Assets/`, `Packages/` e `ProjectSettings/`.
3. Copie o conteúdo deste kit (`Docs/`, `README.md` e `.gitignore`) para **dentro da pasta do projeto Unity**, ao lado dessas três pastas.
4. Abra um terminal nessa pasta e execute `git init`, `git branch -M main`, `git add Docs README.md .gitignore` e `git commit -m "docs: criar GDD v0.1 do Drift District"`.
5. Crie um **repositório remoto vazio** no GitHub (sem criar README adicional). Use seu endereço real nos comandos `git remote add origin URL_DO_REPOSITORIO` e `git push -u origin main`.
6. Quando o primeiro protótipo for jogável, inclua os arquivos Unity (`Assets/`, `Packages/`, `ProjectSettings/` e `.meta`) em novo commit de marco.
7. Nunca faça commit das pastas `Library/`, `Temp/`, `Logs/` e de outros arquivos gerados pela Unity.

## Pastas previstas dentro de Assets

```text
Assets/
  Scenes/          MainMenu.unity, IndustrialDistrict.unity
  Scripts/         Controllers/, Gameplay/, UI/, Audio/
  Prefabs/         CarPlayer, TrafficCone, DriftCheckpoint, GateBarrier
  Materials/
  Models/
  Animations/
  Audio/
  UI/
```

As pastas e os arquivos da Unity devem ser criados ou importados **pelo Editor da Unity** para manter os metadados `.meta` corretos. O kit contém documentação e configuração Git, **não** um projeto Unity executável.
