## Especificação Técnica da Camada de dados - Lopes Novo Portal
Última atualização: 15/03/2020 <br />


## Objetivo
Este documento tem como objetivo instruir a implementação da camada de dados e de data attributes para utilização de recursos de monitoramento do Google Analytics referentes ao ambiente de  (Lopes Portal - lopes.com.br).

## Overview e Descrições Técnicas

### Camada de dados (DataLayer)

> É um array de objetos javascript utilizado pelo Google Tag Manager para receber em seus atributos, dados importantes do site.
Para implementar o dataLayer no site, o desenvolvedor pode utilizar formas diferentes para preencher os dados. Essas formas são dependentes da ação estabelecida na documentação e também do nível da interação.

**Instalação**<br />
Inserir a camada de dados antes do snippet de instalação do Google Tag Manager. Exemplo:

```html
<script>
	window.dataLayer = window.dataLayer || [];
	window.dataLayer = [{
		'atributo1': '[[valor1]]',
		'atributo2': '[[valor2]]'
	}];
</script>
```

OU

```html
<script>
	window.dataLayer = window.dataLayer || [];
	window.dataLayer.push({
		'atributo1': '[[valor1]]',
		'atributo2': '[[valor2]]'
	});
</script>
```

### Atributos HTML (Data Attributes)

> São atributos customizados inseridos nos elementos HTML da página, permitindo a inclusão de dados adicionais.

**Instalação**
1. Elementos: ```<div>Elemento</div>``` <br />
Todos os elementos do html que serão clicados, deverão ser mapeados recebendo os atributos com sua estrutura no item.

```html
<div 	
   	data-gtm-event-category="[[exemplo:valor-categoria]]"
 	data-gtm-event-action="[[exemplo:valor-acao]]"
 	data-gtm-event-label="[[exemplo:valor-rotulo]]"
>
	Texto do elemento
</div>
```

#### Importante:
> Todos itens especificados devem ter os data-attributes `data-gtm-event-category`, `data-gtm-event-action` e `data-gtm-event-label`. Preenchidos conforme instruções específicas.


## Implementação


### Especificações Globais:

**Itens Gerais:**<br />
Todas as informações entre colchetes `[[  ]]` são variáveis dinâmicas que devem ser preenchidas com seus respectivos valores; <br />
Todos os valores enviados ao Google Analytics devem estar sanitizados, ou seja, sem espaços, acentuação ou caracteres especiais; <br />
Caso a informação solicitada não estiver disponível retornar tipagem `undefined`.

### Dimensões Globais:

**Dimensões Customizadas para todas as páginas:**<br />
Deve ser disparado um push de dataLayer no momento de carregamento de todas as páginas do site (Considerar também todas as trocas de Path, quando o conteúdo da página é alterado mas a página não recarrega).<br />

```html
<script>
	window.dataLayer = window.dataLayer || [];
	window.dataLayer.push({
		'event':'global'
		'dimension1': '[[UserID]]',
		'dimension4': '[[Tipo-perfil]]',
		'dimension5': '[[Status-Login]]'
	});
</script>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[UserID]]			| '1234' e etc 				| ID único de usuário defeinido após o cadastro	|
| [[Tipo-perfil]]			| 'cliente', 'corretor'  e etc	| Tipo do perfil do usuário logado	|
| [[Status-Login]]			| 'logado', 'deslogado'				| Status do cliente no site	|

---

### Especificação de páginas:

**Na troca de páginas ou rotas em sites Single Page Application (SPA)**

- **Onde:**  No carregamento da troca da rota

```html
<script>
	window.dataLayer = window.dataLayer || [];
	window.dataLayer.push({
		'event':'vpageview-route'
		'page': '[[Page Path]]',
	});
</script>
```

| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[Page Path]]			| '/path/pagina.html?chave=valor#fragment' 				| Caminho da página completo com querystring e fragment |




### Especificação de Conversões e Micro-conversões:

## Gerais

**No clique dos botões do chat**

- **Onde:**  No icone e links para abrir o chat que aparecem em todas as páginas do site
- **Título ou nome do botão/link:**  Chat

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div
     data-gtm-event-category="lopes:geral" 
     data-gtm-event-action="clique" 
     data-gtm-event-label="chat"
>
Botão
</div>
```

<br />

**No carregamento de erros genéricos no site**
- **Onde:** Nos modais de erros em todas as páginas do site

```html
<script>
	window.dataLayer.push({
		'event': 'event',
		'eventCategory': 'lopes:geral',
		'eventAction': 'erro:modal',
		'eventLabel': '[[tipo-erro]]',
		'noInteraction': '1'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[tipo-erro]]			| 'campos incorretos - email', 'erro de envio' e etc				| Deve retornar os erros na tentativa de envio do formulário	|

<br />

**No carregamento de página de conteúdos 'not found'.**
- **Onde:**  Nas páginas 404 do site

```html
<script>
	window.dataLayer.push({
		'event': 'event',
		'eventCategory': 'lopes:geral',
		'eventAction': 'erro:pagina',
		'eventLabel': '404',
		'noInteraction': '1'
	});
</script>
```

<br />

**No clique dos botões de simular financiamento**
- **Onde:**  No botões para transferir pro site de financiamento nas páginas de ficha de imóvel
- **Título ou nome do botão/link:**  Simule Financiamento

```html
<!-- Use se os atributos no elemento a ser clicado -->
<div
     data-gtm-event-category="lopes:ficha-imovel" 
     data-gtm-event-action="clique" 
     data-gtm-event-label="simular-financiamento"
>
Botão
</div>
```

## Credenciais - Login e Criar Conta

**No callback de sucesso do formulário para login do usuário no site.**
- **Onde:** Na seção de login de corretor ou cliente.

```html
<script>
	window.dataLayer.push({
		'event': 'login',
		'eventCategory': 'lopes:credenciais',
		'eventAction': 'callback:login',
		'eventLabel': 'sucesso',
		'dimension1': '[[UserID]]',
		'dimension4': '[[Tipo-perfil]]',
		'dimension5': '[[Status-Login]]'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[UserID]]			| '1234' e etc 				| ID único de usuário defeinido após o cadastro	|
| [[Tipo-perfil]]			| 'cliente', 'corretor'  e etc	| Tipo do perfil do usuário logado	|
| [[Status-Login]]			| 'logado', 'deslogado'				| Status do cliente no site	|

<br />

**No callback de sucesso do formulário para cadastro do usuário no site.**
- **Onde:** Na seção de criar conta de corretor ou cliente.

```html
<script>
	window.dataLayer.push({
		'event': 'conversion',
		'eventCategory': 'lopes:credenciais',
		'eventAction': 'callback:cadastro',
		'eventLabel': 'sucesso',
		'dimension1': '[[UserID]]',
		'dimension4': '[[Tipo-perfil]]',

	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[UserID]]			| '1234' e etc 				| ID único de usuário defeinido após o cadastro	|
| [[Tipo-perfil]]			| 'cliente', 'corretor'  e etc	| Tipo do perfil do usuário logado	|

<br />

## Enhanced E-commerce - Imóvel e Lead Fale com Corretor

**Na troca de conteudo (onChange) do campo nome do formulário 'Fale com o Corretor'**
- **Onde:** No formulário Fale com o Consultor, nas páginas de ficha de imóvel.

```html
<script>
	window.dataLayer.push({
		'event': 'addToCart',
		'eventCategory': 'lopes:ecommerce:[[nome-formulario]]',
		'eventAction': 'addToCart',
		'eventLabel': undefined,
		'ecommerce': {
			'add': {
				'products': [{
					'dimension7': '[[imovel-end-completo]]',
					'id': '[[id-produto]]',
					'name': '[[nome-produto]]',
					'price': '[[preco-produto]',
					'category': '[[categoria-produto]]',
					'brand': '[[marca-produto]]',
					'variant': '[[variacao-produto]]',
					'quantity': '[[quantidade-produto]]'
				}]
			}
		}
	});
</script>
```

| Variável                 | Exemplo                                    | Descrição                                                            |
|--------------------------|--------------------------------------------|----------------------------------------------------------------------|
| [[nome-formulario]]    | 'fale-com-corretor'e etc  | Deve retornar o nome do formulário.                                       |
| [[imovel-end-completo]]          | 'QnJhc2lsL1NQL1PDo28gUGF1bG8vQXZlbmlkYSBGYXJpYSBMaW1hLCAyMDAw'          | Endereço completo do imóvel no padrão Pais/UF/Cidade/Logradouro hashed em base64|
| [[id-produto]]    | '123456789'e etc  | ID do produto (imóvel).                                       |
| [[nome-produto]]         | 'Belint Bela Vista' e etc | Nome do produto (imóvel).                                     |
| [[preco-produto] | '650000.00' e etc                        | Preço do produto (imóvel)                           |
| [[categoria-produto]]    | 'Lançamento - comercialização', 'Lançamento - breve', 'Lançamento - futuro', 'Pronto pra morar', 'Aluguel' e etc  | Categorização do produto (imóvel)                                |
| [[marca-produto]]        | 'Lopes Prime', 'Lopes São Paulo' e etc                             | Nome da empresa comercializadora (imóvel).                                    |
| [[variacao-produto]]     | 'SP/São Paulo/Zona Oeste/Pinheiros'        | UF/Cidade/Região/Bairro do produto (imóvel).      |
| [[quantidade-produto]]   | '1' e etc                                  | Quantidade do produto (imóvel).                               |

<br />

**No clique do botão enviar do formulário 'Fale com o Corretor', mesmo que não esteja totalmente preenchido e correto**
- **Onde:** No formulário Fale com o Consultor, nas páginas de ficha de imóvel.

```html
<script>
dataLayer.push({
  'event': 'checkout',
  'eventCategory': 'lopes:ecommerce:[[nome-formulario]]',
  'eventAction': 'checkout',
  'eventLabel': 'checkout-etapa-[[checkout-index]]',
  'ecommerce': {
    'checkout': {
      'actionField': {'step': '[[checkout-index]]'},
      'products': [{
      	'dimension7': '[[imovel-end-completo]]',
     	'id': '[[id-produto]]',
		'name': '[[nome-produto]]',
		'price': '[[preco-produto]',
		'category': '[[categoria-produto]]',
		'brand': '[[marca-produto]]',
		'variant': '[[variacao-produto]]',
		'quantity': '[[quantidade-produto]]'
      }]
    }
  }
});
</script>
```

| Variável                 | Exemplo                                    | Descrição                                                            |
|--------------------------|--------------------------------------------|----------------------------------------------------------------------|
| [[nome-formulario]]    | 'fale-com-corretor'e etc  | Deve retornar o nome do formulário.                                       |
| [[checkout-index]]    | '1' e etc  | Retornar "1","2" e etc de acordo com a etapa do funil Ex: Clique no botão do formulário = 1.                                       |
| [[imovel-end-completo]]          | 'QnJhc2lsL1NQL1PDo28gUGF1bG8vQXZlbmlkYSBGYXJpYSBMaW1hLCAyMDAw'          | Endereço completo do imóvel no padrão Pais/UF/Cidade/Logradouro hashed em base64|
| [[id-produto]]    | '123456789'e etc  | ID do produto (imóvel).                                       |
| [[nome-produto]]         | 'Belint Bela Vista' e etc | Nome do produto (imóvel).                                     |
| [[preco-produto] | '650000.00' e etc                        | Preço do produto (imóvel)                           |
| [[categoria-produto]]    | 'Lançamento - comercialização', 'Lançamento - breve', 'Lançamento - futuro', 'Pronto pra morar', 'Aluguel' e etc  | Categorização do produto (imóvel)                                |
| [[marca-produto]]        | 'Lopes Prime', 'Lopes São Paulo' e etc                             | Nome da empresa comercializadora (imóvel).                                    |
| [[variacao-produto]]     | 'SP/São Paulo/Zona Oeste/Pinheiros'        | UF/Cidade/Região/Bairro do produto (imóvel).      |
| [[quantidade-produto]]   | '1' e etc                                  | Quantidade do produto (imóvel).                               |

<br />

**No callback de sucesso ou na tela de sucesso antes de transferir o usuário para falar com o consultor.**
- **Onde:** No formulário Fale com o Consultor, nas páginas de ficha de imóvel.

```html
<script>
dataLayer.push({
  'event': 'purchase',
  'eventCategory': 'lopes:ecommerce:[[nome-formulario]]',
  'eventAction': 'purchase',
  'eventLabel': undefined,
  'dimension6': '[[canal-de-contato]]',
  'ecommerce': {
    'purchase': {
      'actionField': {
        'id': '[[id-transacao]]',   //ID da transação é obrigatório
        'revenue': '[[valor-total-transacao]]',
        'affiliation': '[[nome-formulario]]',
      },
      'products': [{
  		'dimension7': '[[imovel-end-completo]]',
        'id': '[[id-produto]]',
        'name': '[[nome-produto]]',   //Nome ou ID do produto é obrigatório
        'price': '[[preco-produto]]',
        'category': '[[categoria-produto]]',
        'brand': '[[marca-produto]]',
        'variant': '[[variacao-produto]]',
        'quantity': '[[quantidade-produto]]',
      }]
    }
  }
});
</script>
```

*Descrição Purchase:*

| Variável                 | Exemplo                                    | Descrição                                                            |
|--------------------------|--------------------------------------------|----------------------------------------------------------------------|
| [[nome-formulario]]    | 'fale-com-corretor'e etc  | Deve retornar o nome do formulário.                                       |
| [[canal-de-contato]]    | 'whatsapp', 'email', 'celular' e etc  | Canal de contato selecionado no formulário de Fale com o Consultor.                                       |
| id      | [[id-transacao]]      | '11652'        | ID único do lead.                 |
| revenue     | [[valor-total-transacao]] | '650000.00'        | Receita do produto (imóvel)  |
| affiliation | [[nome-formulario]]    | 'fale-com-corretor'e etc        | Nome do formulário.            |

<br />
 
*Descrição Produtos:*
 
| Variável                 | Exemplo                                    | Descrição                                                            |
|--------------------------|--------------------------------------------|----------------------------------------------------------------------|
| [[imovel-end-completo]]          | 'QnJhc2lsL1NQL1PDo28gUGF1bG8vQXZlbmlkYSBGYXJpYSBMaW1hLCAyMDAw'          | Endereço completo do imóvel no padrão Pais/UF/Cidade/Logradouro hashed em base64|
| [[id-produto]]    | '123456789'e etc  | ID do produto (imóvel).                                       |
| [[nome-produto]]         | 'Belint Bela Vista' e etc | Nome do produto (imóvel).                                     |
| [[preco-produto] | '650000.00' e etc                        | Preço do produto (imóvel)                           |
| [[categoria-produto]]    | 'Lançamento - comercialização', 'Lançamento - breve', 'Lançamento - futuro', 'Pronto pra morar', 'Aluguel' e etc  | Categorização do produto (imóvel)                                |
| [[marca-produto]]        | 'Lopes Prime', 'Lopes São Paulo' e etc                             | Nome da empresa comercializadora (imóvel).                                    |
| [[variacao-produto]]     | 'SP/São Paulo/Zona Oeste/Pinheiros'        | UF/Cidade/Região/Bairro do produto (imóvel).      |
| [[quantidade-produto]]   | '1' e etc                                  | Quantidade do produto (imóvel).                               |

**No erro do formulário de lead Fale com o consultor**
- **Onde:** Nas páginas da ficha de imóvel

```html
<script>
	window.dataLayer.push({
		'event': 'event',
		'eventCategory': 'lopes:formulario',
		'eventAction': 'erro:fale-corretor',
		'eventLabel': '[[tipo-erro]]'
	});
</script>
```


| Variável 				| Exemplo 				| Descrição 									|
| :--------------------	| :-------------------- | :-------------------------------------------	|
| [[tipo-erro]]			| 'campos incorretos - email', 'erro de envio' e etc				| Deve retornar os erros na tentativa de envio do formulário										|

<br />

---
## Outros Formulários

**No callback de sucesso do formulário para receber a newsletter**
- **Onde:** No rodapé de todas as páginas

```html
<script>
	window.dataLayer.push({
		'event': 'conversion',
		'eventCategory': 'lopes:formulario',
		'eventAction': 'callback:newsletter',
		'eventLabel': 'sucesso'
	});
</script>
```

**No callback de sucesso do formulário para entrar em contato via Fale Conosco**
- **Onde:** Na seção de fale conosco

```html
<script>
	window.dataLayer.push({
		'event': 'conversion',
		'eventCategory': 'lopes:formulario',
		'eventAction': 'callback:fale-conosco',
		'eventLabel': 'sucesso'
	});
</script>
```

<br />

## Considerações Finais:

> Link de referência: [Documentação Oficial Google Tag Manager](https://developers.google.com/tag-manager/quickstart)

<script>
  document.addEventListener("DOMContentLoaded", function(event) {
    document.querySelectorAll("h1 a")[0].style.display = 'none';
  });
</script>
