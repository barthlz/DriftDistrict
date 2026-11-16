# GDD — Drift District: Industrial Drift

> **Projeto de Jogos Digitais II — Unity + C# — Jogo 3D**  
> **Versão do documento:** 0.1 (concepção)  
> **Data:** 16/11/2026  
> **Estado:** planejamento; funcionalidades descritas abaixo ainda precisam ser implementadas e testadas.

## 1. Identificação

- **Nome:** Drift District: Industrial Drift
- **Gênero:** direção arcade 3D / drift / desafios de pontuação
- **Plataforma:** PC (Windows), teclado
- **Engine:** Unity 6 (ou versão instalada definida pela equipe)
- **Linguagem:** C#
- **Equipe:** até 5 integrantes (preencher nomes e responsabilidades reais)
- **Integrantes e papéis:**
  - Luiz Gustavo Silva Bartholo — programação do carro e física
  - Luiz Gustavo Silva Bartholo — programação dos desafios, pontuação e QA
  - [Nome] — cenário, pista, câmera e iluminação
  - [Nome] — modelos, materiais, animação e áudio
  - Felipe de Souza Santos — interface, documentação e integração
- **Referências de gênero:** jogos de drift arcade e pistas industriais; usar apenas assets próprios ou com licença de uso adequada e crédito.

## 2. Conceito / visão geral

O jogador dirige um único carro em uma **área industrial aberta, pequena e interconectada**. Pode explorar ruas curtas, um pátio e uma garagem sem cronômetro. Ao entrar em portais de dois desafios, inicia eventos de drift com regras diferentes. O objetivo principal é concluir os dois desafios sem perder o controle do veículo, acumulando pontos pelas derrapagens válidas.

**Frase de apresentação:** “Explore um distrito industrial, aprenda a controlar o carro de lado e vença dois desafios de drift antes de terminar a sessão.”

### 2.1 Público-alvo e duração

- Público: jogadores casuais que gostam de carros e drift.
- Sessão demonstrável: 3 a 6 minutos.
- Proposta: acessível no teclado, com física perceptível e placar explicável.

### 2.2 Escopo mínimo obrigatório

- 1 carro controlável; 1 ambiente aberto compacto; 2 desafios na mesma cena de jogo;
- 2 cenas distintas (menu e mundo/jogo); câmera que acompanha o carro;
- HUD com velocidade, pontos, tempo e estado dos desafios;
- vitória, derrota e botão de reiniciar que restaura o estado inicial;
- física 3D com Rigidbody, colisões 3D, pelo menos um prefab usado em 2+ instâncias;
- 1 AnimationClip controlado por Animator; música ambiente e 1 efeito acionado por evento;
- GDD atualizado, código C#, instruções de execução e créditos.

## 3. Experiência e loop principal

**Fluxo:** Menu inicial → entrar no mundo → explorar livremente → aproximar-se do portal do desafio → pressionar `E` → realizar drift válido e cumprir a meta → receber resultado → tentar o outro desafio → vencer após completar ambos; se falhar em um evento, mostrar derrota e permitir nova tentativa.

- A exploração livre não possui tempo limite nem gera pontuação oficial.
- A entrada em qualquer desafio é opcional enquanto o jogador explora; a ordem é livre.
- Os objetivos e estados são sinalizados por mensagens claras no Canvas.
- A sessão não pode ficar presa: os portais dos eventos são acessíveis desde o início e o reinício sempre funciona.

## 4. Controles

| Tecla | Ação |
| --- | --- |
| `W` | Acelerar |
| `S` | Frear / dar ré quando parado |
| `A` / `D` | Virar à esquerda / direita |
| `Espaço` | Freio de mão: reduz temporariamente a aderência traseira e facilita drift |
| `E` | Iniciar um desafio quando perto do seu portal |
| `R` | Reposicionar carro se capotar ou ficar preso; zera o combo atual |
| `Esc` | Pausar / voltar ao menu (se implementado até a beta) |
| Mouse | Clicar nos botões do menu e dos resultados |

## 5. Mecânicas e programação

### 5.1 Direção e física

O carro usa **Rigidbody 3D**, colliders e forças aplicadas na simulação física. O comportamento será **arcade controlado**, para manter a dirigibilidade com teclado. O freio de mão aumenta a tendência de deslizar lateralmente. Gravidade, inércia, choque contra obstáculos e atrito influenciam o movimento. A câmera acompanha o carro com suavização.

**Implementação inicial:** corpo simples feito com cubos, Rigidbody e BoxCollider; movimento com AddForce / AddTorque e controle da aderência lateral. A escolha final entre controlador arcade e WheelColliders será registrada no histórico do GDD; WheelColliders não são obrigatórios para atender ao requisito de uma mecânica com Rigidbody.

### 5.2 Drift válido e pontuação

O jogo detecta drift quando **todas** as condições são verdadeiras:

1. Existe um desafio ativo.
2. O carro está avançando a pelo menos **20 km/h**.
3. O ângulo horizontal entre a frente do carro e a direção do deslocamento está entre **15° e 65°**.
4. O carro está no solo e não está em estado de colisão penalizada.

**Regra inicial a testar:** adicionar aproximadamente `10 pontos por 0,1 s` de drift válido, com multiplicador de combo até `x3` ao manter derrapagens contínuas. A pontuação deve ser calculada em C# e exibida no Canvas. As constantes serão ajustadas por playtest para que as metas sejam alcançáveis.

**Colisão com obstáculos:** interrompe o combo e desconta 100 pontos do evento (mínimo zero). O mesmo impacto não pode pontuar ou penalizar repetidamente sem controle; usar estado/cooldown. Reposicionar com `R` também limpa o combo. Pontos nunca são gerados simplesmente por girar o carro parado.

### 5.3 Desafio A — Pátio Circular

- **Ambiente:** asfalto amplo com duas ilhas de pneus/cones formando círculos.
- **Regra:** manter drifts em torno das ilhas, sem depender de seguir uma sequência fixa.
- **Meta inicial:** conseguir **1.200 pontos em 60 segundos**.
- **Vitória local:** atingir a meta dentro do tempo.
- **Falha:** terminar o tempo abaixo da meta.
- **Diferença de design:** favorece ângulo contínuo e controle em curvas prolongadas.

### 5.4 Desafio B — Zigue-zague de Precisão

- **Ambiente:** corredor com cones e **três portais/checkpoints 3D** que devem ser atravessados em ordem.
- **Regra:** atravessar os três checkpoints durante o evento e somar pontos por drift.
- **Meta inicial:** **1.000 pontos + 3 checkpoints válidos em 75 segundos**.
- **Vitória local:** cumprir ambas as condições dentro do tempo.
- **Falha:** tempo esgotado com pontuação ou checkpoints insuficientes.
- **Diferença de design:** exige alternância de direção e passagem por alvos, não apenas deslizar em círculos.

### 5.5 Vitória, derrota e nova tentativa

- **Vitória geral:** concluir A e B, em qualquer ordem. Exibir tela/painel “Vitória” com pontuação final.
- **Derrota:** não atingir a meta de um desafio até o fim do cronômetro. Exibir tela/painel “Derrota” com motivo claro.
- **Reinício:** botão “Jogar novamente” recarrega a cena de jogo ou reinicializa por completo carro, tempos, pontuações, checkpoints, obstáculos e estados dos desafios.
- **Menu:** botão “Jogar” transita logicamente de `MainMenu` para `IndustrialDistrict`.

## 6. Mapa, cenário e visual

### 6.1 Estrutura do mapa (escopo pequeno, sensação de mundo aberto)

**Tamanho-base estimado:** terreno de **220 × 220 metros**, ajustável após teste de escala.

```text
+--------------------------------------------------+
| Rua periférica                     Desafio B      |
|   +------------+                [ZIGUE-ZAGUE]    |
|   | Galpões    |                      |           |
|   +------------+------- Rua ---------+           |
|           |                                      |
|        [GARAGEM / SPAWN] ---- [PÁTIO LIVRE]      |
|                    |                    |         |
|               Rua circular     [DESAFIO A]       |
|                                PÁTIO CIRCULAR    |
+--------------------------------------------------+
```

A circulação entre as zonas é livre, sem telas de carregamento nem portões de progresso. Cercas e estruturas delimitam naturalmente as bordas e impedem cair para fora da área.

### 6.2 Direção visual

- Visual 3D estilizado de baixa/média complexidade; foco em leitura e estabilidade.
- Asfalto escuro, faixas claras, cones laranja, barreiras de pneus, galpões e containers.
- Câmera em terceira pessoa, distância suficiente para enxergar os obstáculos.
- Materiais distintos para carro, pista, checkpoints e objetos interativos.
- Iluminação inicialmente diurna para garantir legibilidade; pôr do sol é opcional.

### 6.3 Assets, prefabs e animação

- **`CarPlayer`:** carro (corpo, rodas visuais, collider, Rigidbody, scripts).
- **`TrafficCone`:** prefab reutilizado em pelo menos 8 posições da pista.
- **`DriftCheckpoint`:** prefab reutilizado nos três portais do desafio B.
- **`GateBarrier`:** obstáculo visual com Animator e AnimationClip `GateOpen` que toca após desafio concluído (ou ao iniciar desafio, conforme integração).
- Se modelos externos forem usados, registrar nome, autor, licença e link na seção de créditos.

## 7. Áudio e interface

- Música instrumental ambiente em loop durante a partida, com licença adequada.
- Efeito de derrapagem ativado/desativado com base na condição de drift (se viável).
- Efeito obrigatório por evento: som ao atravessar checkpoint, bater em cone ou concluir desafio. Repetir evento deve reproduzir o som novamente.
- UI em **Canvas**:
  - Menu inicial com botões “Jogar” e “Sair”.
  - Velocímetro em km/h; pontos; multiplicador; tempo restante.
  - Nome do desafio ativo e estado A/B.
  - Mensagens de instrução/resultado; botões “Jogar novamente” e “Menu”.

## 8. Arquitetura planejada de C#

| Script | Responsabilidade |
| --- | --- |
| `CarController.cs` | Entradas, aceleração, direção, freio de mão, Rigidbody |
| `FollowCamera.cs` | Posicionar e suavizar câmera de perseguição |
| `DriftScore.cs` | Detectar velocidade/ângulo/tempo, acumular combo e pontos |
| `ChallengeTrigger.cs` | Identificar entrada em região de início e uso de `E` |
| `ChallengeManager.cs` | Estados livre / desafio A / desafio B / vitória / derrota; tempos e metas |
| `Checkpoint.cs` | Validar passagem por portais em ordem durante o desafio B |
| `ObstacleCollision.cs` | Aplicar consequência de colisão sem punição duplicada |
| `UIManager.cs` | Atualizar Canvas e mensagens com valores reais |
| `GameMenu.cs` | Carregar cena e acionar reinício |
| `AudioManager.cs` | Música e efeitos relacionados a eventos |

**Observação:** os nomes são um plano de organização. A arquitetura pode ser simplificada durante a implementação, desde que as funcionalidades e a autoria sejam compreensíveis.

## 9. Cenas e fluxo

1. `MainMenu.unity`: título, controles e botão de jogar.
2. `IndustrialDistrict.unity`: mundo 3D, carro, HUD, desafios e painéis de vitória/derrota.

Os painéis de resultado não precisam ser cenas separadas, pois as duas cenas obrigatórias já existem. A mudança de menu para jogo será realizada via código.

## 10. Cronograma e entregas

| Data | Versão | Entregável verificável | Marcador Git sugerido |
| --- | --- | --- | --- |
| 12/11/2026 | v0.1 | GDD inicial; greybox; carro e câmera; colisão/mecânica inicial | `entrega-1-prototipo` |
| 19/11/2026 | v0.2 | Física/colisões integradas; prefab 2+ instâncias; animação, música, efeito, Canvas, duas cenas; condições iniciais de vitória/derrota | `entrega-2-integracao` |
| 26/11/2026 | v0.3 | Dois desafios completos; percurso inteiro; telas de vitória/derrota; reinício; testes e correções | `entrega-3-beta` |
| 03/12/2026 | v1.0 | Versão estável; GDD e créditos atualizados; projeto-fonte e demonstração presencial | `entrega-4-final` |

**Nota de calendário:** o GDD foi iniciado em 16/11/2026, portanto a data de 12/11 já passou. Não retroagir commits. A primeira etapa só estará completa quando o protótipo também estiver funcional.

## 11. Critérios de avaliação e como demonstrar

### 11.1 Mecânicas e programação (2 pontos)

- Dirigir com comandos documentados.
- Concluir a ação central: drift e atingir meta.
- Explicar interação em C#: checkpoint, início de desafio, obstáculo.
- Exibir atualização real de pontos sem contar o mesmo evento duas vezes.

### 11.2 Física, colisões e reutilização (2 pontos)

- Collider 3D impede atravessar o chão e ao menos um obstáculo.
- Trigger ou colisão muda o estado: penalidade / checkpoint.
- Rigidbody influencia aceleração, inércia, gravidade ou impacto do carro.
- `TrafficCone` ou `DriftCheckpoint` aparece em duas ou mais instâncias na cena.

### 11.3 Cenas, interface e resultados (2 pontos)

- Existem `MainMenu` e `IndustrialDistrict`, com transição funcional.
- Botão “Jogar” do menu em Canvas inicia a partida.
- HUD de Canvas apresenta pontos e pelo menos um indicador vivo (tempo / velocidade / status).
- Vitória e derrota são alcançáveis jogando e exibem resultados distintos.

### 11.4 Animação, áudio e leitura visual (2 pontos)

- Música de fundo audível durante a partida.
- Som por evento ocorre e pode acontecer outra vez ao repetir a ação.
- Animator reproduz um AnimationClip verificável (barreira, portão ou bandeira).
- Câmera, materiais e cenário permitem distinguir carro, obstáculos e objetivos.

### 11.5 Game design e jogabilidade (2 pontos)

- Regra própria: pontuação baseada em velocidade, ângulo e manutenção do drift.
- Há dois desafios diferentes: círculo contínuo e zigue-zague com checkpoints.
- É possível percorrer o início ao fim sem travamentos de progressão.
- Reiniciar após vitória ou derrota restaura carro, metas, cronômetro e pontos.

## 12. Testes e critérios de conclusão

- [ ] Menu → jogo sem erros de Console.
- [ ] Carro acelera, vira, freia e derrapa de modo controlável.
- [ ] Rigidbody sofre gravidade e colide com chão / obstáculos.
- [ ] Drift não pontua com o carro parado ou sem desafio ativo.
- [ ] Colisões e checkpoints não são contados múltiplas vezes indevidamente.
- [ ] Dois portais de desafio acessíveis no mundo livre.
- [ ] Desafio A pode ser vencido e perdido.
- [ ] Desafio B pode ser vencido e perdido.
- [ ] Vitória geral somente após completar A e B.
- [ ] Botão de reinício restaura todos os estados.
- [ ] Cones/checkpoints são instâncias de prefab reutilizado.
- [ ] Animator toca um clip visível.
- [ ] Música e efeito de evento audíveis.
- [ ] HUD apresenta os números e estados corretos.
- [ ] Projeto abre com cenas, scripts, prefabs e arquivos `.meta` preservados.

## 13. Escopo opcional (somente se sobrar tempo)

Fumaça de pneus, marcas no asfalto, personalização do carro, mapa maior, circuito adicional, mudança de câmera, minimapa, tráfego ou controle por volante. **Nenhum desses recursos substitui requisitos obrigatórios.**

## 14. Fontes, créditos e instruções de execução

- **Ferramentas previstas:** Unity + editor C#, Git/GitHub; Blender e editor de áudio são opcionais.
- **Fonte de assets, música e efeitos:** `[PREENCHER ao importar, com autor + licença + URL]`.
- **Como executar (após criação do projeto):** abrir o projeto pelo Unity Hub usando a versão registrada; abrir `Assets/Scenes/MainMenu.unity`; pressionar Play; iniciar pelo botão “Jogar”. Incluir ajustes específicos caso existam.
- **Créditos:** `[PREENCHER nomes e responsabilidades reais]`.
- **Materiais externos:** `[PREENCHER atribuições, permissões e referências]`.

## 15. Histórico de decisões — GDD vivo

| Versão / data | Decisão | Efeito no jogo |
| --- | --- | --- |
| v0.1 — 16/11/2026 | Definir jogo de drift 3D com bairro industrial aberto compacto e duas provas | Controla escopo, permite dois desafios, vitória/derrota e apresentação curta |
| v0.2 — [data] | [registrar mudança real] | [registrar efeito real] |
| v0.3 — [data] | [registrar mudança real] | [registrar efeito real] |
| v1.0 — [data] | [registrar mudança real] | [registrar efeito real] |
