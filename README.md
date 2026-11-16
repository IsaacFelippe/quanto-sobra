# Quanto Sobra

**Quanto sobra de verdade em cada prato que o restaurante vende?**

Conceito de feature para restaurantes pequenos parceiros do iFood: o dono manda a foto do cupom da compra pelo WhatsApp e o app calcula o custo real de cada prato e quanto sobra depois de ingredientes, embalagem, taxa e cupom.

> Projeto independente de portfólio. Não é afiliado ao iFood. Todos os dados são fictícios.

[Protótipo navegável](LINK_DO_PROTOTIPO) · [Vídeo de 2 minutos](https://youtu.be/S490nYsu4s0) · [Detalhes do projeto](docs/detalhes.md)

[![Tela do Quanto Sobra: custo real de cada prato](assets/capa.png)](https://youtu.be/S490nYsu4s0)

---

## O problema

O dono de lanchonete vende bem no app e não sabe se o X-Salada de R$ 34,90 deixa R$ 11 ou R$ 0,50 no bolso. Faturamento parece lucro até o fim do mês.

O próprio iFood aponta que cerca de 70% dos empreendedores cuidam pessoalmente das finanças e mais de 77% deles nunca fizeram um curso de gestão financeira ([fonte](https://blog-parceiros.ifood.com.br/painel-financeiro/)).

## O que já existe

O iFood já oferece uma [calculadora de precificação](https://blog-parceiros.ifood.com.br/materiais-gratuitos/calculadora-de-precificacao-ifood/) e um [Painel Financeiro](https://blog-parceiros.ifood.com.br/painel-financeiro/). A calculadora parte do preço que o dono já pratica e o painel mostra o que entrou e foi repassado. Nenhum dos dois responde **quanto custa fazer cada prato**.

O Quanto Sobra cobre esse espaço e pode alimentar a calculadora com um custo calculado direito.

## Pra quem

Lanchonete de bairro com 3 a 6 funcionários, que vende no balcão e no delivery e compra no atacado e no mercado. Gestão no caderno ou numa planilha.

## Como funciona

1. O dono manda **a foto do cupom** da compra pelo WhatsApp.
2. O sistema lê o **QR Code** do cupom e pega a chave da nota e o total da compra.
3. A **IA lê os itens impressos** e identifica cada ingrediente ("QJ MUSS FAT KG" vira "Queijo muçarela").
4. A **soma dos itens tem que bater com o total do QR Code**. Se não bater, o app avisa.
5. O dono **confere e confirma**. Só então os preços mudam.
6. Cada prato recalcula na hora e o app mostra quanto sobra, no app e no balcão, e se um combo vale a pena.

## E quando a IA errar?

Ela não muda nenhum preço sozinha. A soma precisa bater com o total do QR Code, preços muito diferentes do último chegam desmarcados com alerta, e o dono confirma tudo antes. [Como cada erro é tratado](docs/detalhes.md#erros-que-o-produto-trata).

## Valor pro iFood

A proposta é ganha-ganha: o restaurante que sabe a margem sobrevive mais, e o iFood mantém o parceiro e o volume de pedidos.

| Hipótese | Como medir |
|---|---|
| Menos restaurantes fechando | Churn de quem usa vs. quem não usa |
| Pedidos maiores com combos bem precificados | Ticket médio |
| Base melhor pra crédito do iFood Pago | Inadimplência de quem usa |

Decisões de produto, erros tratados, engenharia e custo, riscos e próximos passos estão em [detalhes do projeto](docs/detalhes.md).

---

Feito por Isaac Melo · [LinkedIn](LINK_DO_LINKEDIN)
