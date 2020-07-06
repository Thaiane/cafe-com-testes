<p align="center">
  <a href="https://github.com/pagarme/cafe-com-testes">
    <img src="../.github/cafecomtestes.png" alt="Café com Testes">
  </a>
</p>

# Evite seeds de dados globais, adicione os dados por teste

Para beneficiar a performance dos testes, muitas vezes adotamos a abordagem de criar seeds para popular o banco de dados antes de executar a suíte de testes. Embora performance seja um ponto importante, essa decisão pode aumentar a complexidade do processo de teste, dado que todos os testes vão utilizar os dados desse seed e isso pode causar dependência, falsos negativos e intermitência. 

O ideal é que cada teste seja explicitamente responsável pelos dados que precisa. Isso contribui para legibilidade dos testes e facilita o entendimento de quais dados estão sendo manipulados. Você pode também utilizar uma abordagem híbrida fazendo o seed global de dados que não serão modificados durante o teste, por exemplo.

### Utilizando seed global:

```js
before(async () => {
  //todos os dados que serão utilizados no teste estão em um arquivo externo.
  await db.addSeedDataFromJson('seed.json');
});

it("When updating site name, get successful confirmation", async () => {
  //preciso ter certeza que o site “portal” existe no seed, senão meu teste vai quebrar
  const siteToUpdate = await site.getSiteByName("Portal");
  const updateNameResult = await site.changeName(siteToUpdate, "newName");
  expect(updateNameResult).to.be(true);
});

it("When querying by site name, get the right site", async () => {
   //preciso ter certeza que o site “portal” existe no seed, senão meu teste vai quebrar
  const siteToCheck = await site.getSiteByName("Portal");
  expect(siteToCheck.name).to.be.equal("Portal"); //Esse teste falha porque o teste anterior alterou o dado do seed
});```


### Manipulando os dados por teste:

```js
it("When updating site name, get successful confirmation", async () => {
  //o teste adiciona os dados que precisa
  const siteUnderTest = await db.site.add({
    name: "siteForUpdateTest"
  });

  const updateNameResult = await site.changeName(siteUnderTest, "newName");

  expect(updateNameResult).to.be(true);
});```