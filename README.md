# RookOverlay Analytics

**Status:** protótipo em desenvolvimento; solicitação de API de produção da Riot Games enviada e URL verificada, com aprovação ainda pendente.

RookOverlay Analytics é um projeto para Windows com módulos em C++ e Python, voltado à **análise retrospectiva de partidas concluídas de VALORANT**. A ferramenta permite importar um JSON de partida obtido por meios autorizados e gerar relatórios locais.

## O que o protótipo faz hoje

- Importa JSON no formato de resposta de partida `VAL-MATCH-V1` contendo `matchInfo`, `players` e, quando disponíveis, `teams`.
- Produz `relatorio.html`, `jogadores.csv` e `resumo.json` com dados da partida, eliminações, mortes, assistências, K/D e pontuação média por rodada *estimada* (`score/roundsPlayed`).
- Inclui opção de consultar uma partida pelo ID e região **somente quando** houver chave e permissões válidas da API de produção da Riot.
- Mantém, como módulo separado, um overlay de área de trabalho que recebe dados externos por CSV. Esse overlay **não obtém dados do VALORANT** e não é usado para apresentar informações ocultas da partida.

## Fluxo do usuário

1. O jogador obtém dados de partida concluída de fonte autorizada.
2. No Windows, executa `IMPORTAR_PARTIDA.bat` e informa o caminho de um arquivo JSON de partida.
3. O script `historico_valorant.py` valida o formato e processa as estatísticas.
4. O jogador abre `Relatorios/relatorio.html` no navegador para revisar a própria partida e pode exportar CSV/JSON.
5. Após eventual aprovação de produção e integração Riot Sign On (RSO), o aplicativo deverá solicitar consentimento explícito (*opt-in*) do jogador antes de buscar ou apresentar dados de sua conta.

## Situação de integração com a Riot

A integração com a API de produção e com o Riot Sign On **ainda não está ativa**. Nenhum jogador deve ser convidado a fornecer senha, chave de API ou token diretamente ao aplicativo ou a terceiros. A consulta de partidas por API depende da autorização da Riot e das permissões do usuário.

O protótipo **não disponibiliza ESP de inimigos, posições ocultas em tempo real, automação de decisões durante partidas nem bypass de Vanguard**.

## Execução local do módulo de análise

Requisitos: Windows e Python 3.10+ (testado localmente com Python 3.14 para o verificador; a compilação C++ deve ser feita no Windows).

```powershell
python historico_valorant.py --arquivo "C:\caminho\para\partida.json" --saida Relatorios
```

O JSON deve ter sido obtido de maneira permitida. O arquivo de código não deve conter credenciais. Para API, após aprovação:

```powershell
$env:RIOT_API_KEY="SUA_CHAVE_PRIVADA"
python historico_valorant.py --partida "ID_DA_PARTIDA" --regiao br --saida Relatorios
```

Não publique nem faça commit de chaves, tokens, arquivos pessoais de jogadores ou relatórios privados.

## Privacidade e conformidade

- O acesso a dados de jogadores via Riot Sign On requer opt-in e o fluxo OAuth oficial quando autorizado.
- Dados privados de jogadores que não deram consentimento não devem ser publicados.
- Este projeto é independente e não é endossado, patrocinado ou afiliado à Riot Games.
- Não usa logotipos oficiais da Riot ou VALORANT.

## Documentação

- [Riot Developer Portal — VALORANT](https://developer.riotgames.com/docs/valorant)
- [Riot Developer Portal](https://developer.riotgames.com/)

## Próximas etapas

1. Apresentar o protótipo e este fluxo à equipe Riot Developer Relations.
2. Obter análise/aprovação para a chave de produção e iniciar a integração RSO se autorizado.
3. Adicionar interface de seleção de partida e relatórios autorizados por conta.
4. Realizar testes com dados próprios mediante consentimento.
