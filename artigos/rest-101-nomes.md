<p align="center">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/cafecomtestes.png" alt="Café com Testes">
  </a>
</p>

# Rest 101: Nomes

Em REST, os recursos são descritos como substantivos ao invés de verbos. Os verbos, que descrevem a ação, ficam nos verbos do HTTP (GET, POST, PUT, DELETE, etc.).

Os substantivos também devem ser escritos no plural e em caso de mais de um nome, preferencialmente separados por "-" (dash) e não "_" (underscore).

### Exemplo de má prática

```
POST https://api.pagar.me/create_customer
POST https://api.pagar.me/customer
GET https://api.pagar.me/payment_link
```

### Exemplo de boa prática:

```
POST http://api.pagar.me/customers
POST http://api.pagar.me/payment-links
GET http://api.pagar.me/customers/{id}
```
