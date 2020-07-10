<p align="center">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/cafecomtestes.png" alt="Café com Testes">
  </a>
</p>

# Estrutura dos testes

Mantenha uma estrutura lógica para seus testes, seguindo a sequência: Configuração, ação e resultado esperado.

Se precisar misturar essa sequência ou tiver multiplas ações, provavelmente você possui mais de um comportamento que deveria ser testado separadamente.


## Exemplo misturando os blocos
```javascript
// O que esse teste realmente testa ?
it('should withdraw money', () => {
  user.setBalance(2000);
  
  expect(user.getBalance()).toBe(2000);  
  
  const money = user.withdraw(1000);
  
  // Se essa validação falhar, a execução irá parar
  // E o segundo cenário não será testado.
  expect(money).toBe(1000);
  expect(user.getBalance()).toBe(1000);
  
  // A primeira parte do teste pode impactar nesse segundo cenário
  const money2 = user.withdraw(1001);
  
  expect(money2).toBe(null);
  expect(user.getBalance()).toBe(1000);
});
```


## Exemplo mantendo a estrutura e separando os múltiplos cenários
```javascript

it('should allow money withdraw when the user has the necessary balance', () => {
  // Configuração
  user.setBalance(150);
  
  // Ação
  const money = user.withdraw(120);
  
  // Resultado esperado;
  expect(money).toBe(120);
  expect(user.getBalance()).toBe(30);
});

it('should not allow money withdraw when the requested amount is above the current balance', () => {
  // Configuração
  user.setBalance(1000);
  
  // Ação
  const money = user.withdraw(15000);
  
  // Resultado esperado;
  expect(money).toBe(null);
  expect(user.getBalance()).toBe(1000);
});
```
