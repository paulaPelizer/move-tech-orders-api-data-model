# Modelagem de Dados

## Visão geral

A aplicação possui duas entidades principais:

- **Pedido (`orders`)**
- **Item (`items`)**

Um pedido pode possuir vários itens, enquanto cada item pertence a somente um pedido.

## Entidades

### Pedido (`orders`)

| Coluna | Tipo | Restrições | Descrição |
|---|---|---|---|
| `id` | VARCHAR / UUID | Chave primária | Identificador único do pedido, gerado automaticamente como UUID. |
| `customer` | VARCHAR | Obrigatório | Nome do cliente responsável pelo pedido. |
| `status` | VARCHAR | Padrão: `open` | Estado atual do pedido. |
| `created_at` | TIMESTAMP com fuso horário | Gerado automaticamente | Data e hora de criação do pedido. |

### Item (`items`)

| Coluna | Tipo | Restrições | Descrição |
|---|---|---|---|
| `id` | VARCHAR / UUID | Chave primária | Identificador único do item, gerado automaticamente como UUID. |
| `order_id` | VARCHAR / UUID | Chave estrangeira, obrigatório | Referência ao campo `orders.id`. |
| `sku` | VARCHAR | Obrigatório | Código de identificação do item. |
| `description` | VARCHAR | Obrigatório | Descrição do produto ou item. |
| `quantity` | INTEGER | Obrigatório | Quantidade do item incluída no pedido. |

## Relacionamento

O relacionamento entre `orders` e `items` é do tipo **um para muitos (1:N)**:

- Um registro da tabela `orders` pode estar relacionado a vários registros da tabela `items`.
- Cada registro da tabela `items` pertence a apenas um registro da tabela `orders`.
- A coluna `items.order_id` é uma chave estrangeira que referencia `orders.id`.
- O relacionamento utiliza exclusão em cascata: ao remover um pedido, seus itens associados também são removidos.

```text
orders
  1
  |
  | possui
  |
  N
items
```

## Regras principais

- Todo item deve estar associado a um pedido existente.
- O cliente do pedido é obrigatório.
- O status inicial de um pedido é `open`.
- Os identificadores são gerados automaticamente no formato UUID.
- A data de criação é preenchida automaticamente no momento do cadastro.
