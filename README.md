# Venha para Recomb

**Documentação da solução de Nathan Garcia**
Neste documento estará a descrição dos componentes e das funcionalidades do sistema.
## Lógica de funcionamento e componentes 
Iniciando pela lógica do fluxo entre as páginas desejadas, adicionei uma etapa adicional a esse fluxo,
que consiste na escolha dos itens a serem comprados pelo usuário do sistema (já logado). O fluxo fica da seguinte maneira: 
Usuário escolhe os produtos e vai pagar -> Seleciona a forma de pagamento -> Preenche dados necessários -> Finaliza o pagamento 
-> Vê resumo do pedido.

## Home e Navbar
O usuário vê a página principal com itens a disposição para serem selecionados e comprados, 
sendo cada item dentro de um card com seu nome, descrição e uma foto (Home.vue). Ao longo de todas 
as páginas haverá também uma Navbar que mostrará o nome do sistema e indicando que "o Usuário está logado". 
A navbar também possuirá o botão de "Pagar" e de "Resumo da Compra" apenas nas páginas necessárias (Navbar.vue).

## Seleção de método de pagamento
Após decidir pagar e clicar no botão, o usuário é redirecionado para a aba de pagamento, escolhendo entre
os três campos referidos na descrição do sistema: Cartão de Crédito, Pix ou Boleto (SelectPayment.vue). Após selecionar,
o usuário é redirecionado para a forma de pagamento selecionado.

## Cartão de Crédito
Formulário para preencher informações necessárias: Bandeira e preenchimento das informações do titular 
(Número de cartão, código CVV, data de vencimento mm/aaaa e Nome do titular, CPF/CNPJ) e número de parcelas (CreditCardInfo.vue). 
Após efetuar confirmar as informações e estiverem válidas, aparecerá uma tela mostrando que o sistema está aguardando a 
confirmação da operadora (CreditCardFinishPayment.vue). Nessa mesma página, há o botão de Resumo da compra, que será detalhado a frente.

## Boleto
Formulário para preencher informações necessárias: CPF, Nome, Sobrenome e e-mail (BankingBilletInfo.vue). 
Após efetuar confirmar as informações e estiverem válidas, aparecerá uma tela mostrando um código de barras para 
efetuar o pagamento do boleto (imagem fictícia), junto com a expiração do mesmo e um botão para baixar o PDF 
do boleto (CreditCardFinishPayment.vue). Nessa mesma página, há o botão de Resumo da compra, que será detalhado a frente.

## Pix
Como não necessita de informações adicionais, o usuário é levado diretamente para a finalização do pagamento,
onde haverá um QR Code com um tempo de expiração para o mesmo (PixFinishPayment.vue).

## Resumo
Apresenta as seguintes informações (Summary.vue):
* Produtos comprados (foto, nome e descrição)
* Método de pagamento escolhido
* Data da compra
* Previsão de entrega 

## Execução
Entrar na página recomb-frontend e rodar o comando:
```
npm install
```
Logo após, apenas executa, no mesmo diretório, o comando:
```
npm run dev
```
E entre no link: 'http://localhost:5173/'

## Diferenciais
- Implementar usando Vue