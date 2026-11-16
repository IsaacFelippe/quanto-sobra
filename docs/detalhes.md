# Quanto Sobra: detalhes do projeto

[Voltar pro README](../README.md)

## Regra central

A IA interpreta, a fórmula calcula e o dono confirma. A IA nunca produz um número final sozinha.

## Decisões de produto

- **Cupom no lugar da SEFAZ.** Os portais da SEFAZ bloqueiam acesso automatizado e cada estado tem um portal. Lendo o cupom impresso, o MVP funciona em qualquer estado e sem depender desse acesso.
- **O QR Code é o fiscal da IA.** O total que vem do QR confere a leitura e a chave da nota evita lançar o mesmo cupom duas vezes.
- **Trava de preço sem IA.** Se um preço vier muito diferente do último (ex.: R$ 42 virar R$ 4,20/kg), o item cai desmarcado com alerta. Um preço errado contaminaria todos os pratos.
- **A cozinha mede em fatia, o sistema em grama.** O dono diz "2 fatias", o app usa um peso padrão (20 g pra queijo) que ele ajusta uma vez.
- **App e balcão separados.** No balcão não tem taxa nem cupom. Mesmo cobrando R$ 5 a menos, o X-Salada de exemplo deixa R$ 4,15 a mais no balcão.
- **Combo com duas comparações.** O combo quase sempre ganha de "só o lanche" e perde de "tudo separado". O app mostra as duas e o dono decide.
- **Descontos como na nota.** Desconto de item fica no item. Desconto da compra toda é dividido entre os itens pelo valor de cada um.
- **Cupom de desconto por prato.** O dono informa o cupom de cada prato, em R$ ou em %, e ele começa em zero. A conta supõe que todo pedido daquele prato usa o cupom: é o pior cenário, mais fácil de entender e mais seguro pra decidir preço. A taxa do app é calculada sobre o preço cheio.
- **Lucro é por pedido, não por item.** Um lanche barato com cupom pode dar prejuízo sozinho e lucro dentro de um combo, porque o mesmo desconto se dilui num pedido maior. Por isso o combo mostra duas comparações, e não um veredito.
- **Embalagem por tipo de prato.** Embalagem é um insumo comprado como qualquer outro, com tipo (lanche, pizza, porção, bebida). Ela entra sozinha em todos os pratos daquele tipo e cada prato pode tirar a sua. Quando a caixa sobe na compra, todos os pratos do tipo recalculam juntos.
- **Cadastro rápido.** Receitas-modelo (X-Burguer, X-Salada) e cadastro de ingrediente novo sem sair do lançamento.

## Erros que o produto trata

| Situação | O que acontece |
|---|---|
| Foto ilegível (3 tentativas) | Lançamento manual ou por áudio no WhatsApp |
| Soma não bate com o total do QR | Aviso no topo da conferência |
| Preço muito diferente do último | Item desmarcado com alerta |
| Cupom repetido | Bloqueado pela chave da nota |
| Item que não é ingrediente | "Custo diverso" ou "compra pessoal" |
| Câmera negada | Caminho pros Ajustes, ou foto pelo WhatsApp |
| Prato sem ingredientes | "Receita incompleta", fora das médias |

Também testei o layout com dados extremos (nomes enormes, palavra sem espaço, valores na casa do milhar). Isso revelou dois problemas reais que foram corrigidos.

## Engenharia e custo

- **Leitura do QR:** biblioteca comum, sem custo.
- **Leitura dos itens:** modelo pequeno com visão, com saída em JSON (ingrediente, quantidade, unidade, confiança). Modelo maior só quando a confiança é baixa.
- **Dicionário por restaurante:** cada item confirmado vira regra ("QJ MUSS FAT" = queijo). Com o tempo, a maioria das linhas nem passa pela IA. O custo cai e o acerto sobe.
- **Cálculos, rateio e travas:** código comum, previsível e testável.

| Peça | Custo estimado |
|---|---|
| IA por cupom | ~US$ 0,001 ([preços](https://docs.claude.com/en/docs/about-claude/pricing)) |
| Áudio, quando usado | menos de US$ 0,01 por minuto |
| WhatsApp | ~R$ 0,04 a 0,05 por mensagem ([Meta](https://developers.facebook.com/docs/whatsapp/pricing)) |
| **Por restaurante** | **menos de R$ 5 por mês**, com ~15 cupons |

O maior custo é o WhatsApp, não a IA. Valores de nov/2026, sujeitos a mudança.

Os dados de custo só seriam usados com consentimento explícito do restaurante (LGPD).

## Riscos

- **Cupom apagado ou amassado:** saída manual ou por áudio.
- **Erro de leitura da IA:** confiança, trava de preço, conferência da soma e confirmação humana.
- **Regras do WhatsApp:** custo por mensagem e mensagens pré-aprovadas fora da janela de 24h.
- **Adoção:** o maior risco. Ainda não validei com donos de restaurante.

## Evoluções da feature

- **Consulta oficial na SEFAZ** pela chave do QR, via API licenciada, pra ter os dados oficiais da nota.
- **NF-e de atacado automática:** compras com CNPJ baixadas da SEFAZ com o certificado digital do restaurante, sem foto.
- **Dados que o iFood já tem:** volume de pedidos (pra média ponderada por vendas), cardápio (pra cadastrar os pratos de uma vez) e taxa do contrato.
- **Cupom real, com dados do iFood:** a porcentagem de pedidos que usou cupom em cada prato, pra trocar o pior cenário pela média de verdade, e os cupons do pedido inteiro (com pedido mínimo), divididos entre os itens.
- **Alertas de compra:** avisar quando um ingrediente sobe e quais pratos foram afetados.
- **Custo da loja:** aluguel, contas e equipe, com o número de vendas pra empatar no mês.

## O que eu ainda não sei

O protótipo nasceu de pesquisas com áreas de negócio no trabalho, não de entrevistas com donos de lanchonete. O próximo passo seria conversar com 5 deles e perguntar três coisas: se sabem quanto sobra no prato mais vendido, como decidiram o preço e se guardam os cupons das compras.

Também preciso validar como os descontos funcionam na prática:

- **Tipos de cupom.** Existe o cupom do pedido inteiro, com pedido mínimo, e o desconto em pratos selecionados. O protótipo só trata o segundo. Quanto cada um pesa no caixa de uma lanchonete pequena?
- **Quem paga.** Alguns cupons são pagos pelo iFood e não deveriam entrar na conta. O dono sabe separar isso no extrato?
- **Taxa sobre o desconto.** A comissão incide sobre o preço cheio ou sobre o valor com desconto? Isso muda alguns centavos por prato.
