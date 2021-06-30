## Aprendendo utilizar lambda sem servidor oflline
junto com tutorial: https://how.kovi.work/construindo-uma-aplica%C3%A7%C3%A3o-serverless-do-zero-cd0d70527d61


#Antes precisamos conhecer nossas dependências e o que precisaremos para iniciar nosso projeto:
- NodeJS (12.18.3)
- Yarn (1.22.5)
- AWS Client devidamente instalado e configurado

Primeiramente iremos rodar o seguinte comando instalando algumas dependências:
...
- $ yarn add serverless serverless-offline -D
...
  
## Após a instalação iremos criar um novo arquivo handler.js e nele colocaremos o seguinte código:



...

'use strict';

module.exports.hello = async event => {
  return {
    statusCode: 200,
    body: JSON.stringify(
      {
        message: 'It Works!',
      },
      null,
      2
    ),
  };
};

...

Também iremos adicionar um arquivo chamado serverless.yml e nele o seguinte conteúdo:

...
service: node-serverless-lambda-api


provider:
  name: aws
  runtime: nodejs12.x

package:
  excludeDevDependencies: true
  individually: true
  
plugins:
  - serverless-offline

functions:
  hello:
    handler: handler.hello
    events:
        - http:
            path: /hi
            method: any
...            

 ### Agora vamos entender o que esse arquivo faz:

-Primeiramente damos o nome ao serviço (Linha 2)
-Também informamos onde esse arquivo estará rodando e qual o runtime

### Package:
- A opção excludeDevDependencies indica se o serverless deve automaticamente excluir as dependências de desenvolvimento.
- A opção individually habilita fazer o empacotamento por cada função. Caso verdadeiro deveremos informar um package para cada função.
### Plugins:
Antes de continuarmos precisamos fazer um adendo, o serverless nada mais é do que um grupo de plugins que fazem parte do core do produto. Sendo assim os plugins nada mais são do que códigos JavaScript que adicionam ou estendem comandos dentro do serverless. Caso queira aprofundar um pouco mais recomendo a leitura da documentação.
- Estaremos inserindo o plugin serverless-offline para que possamos fazer todos os processos que o serverless executa para deploy mas local.
  
### Functions:
Dentro de functions informamos primeiramente o nome da nossa primeira function hello
handler basicamente este apontamento indica onde está o código que será executado.
events o evento que irá realizar o trigger dessa lambda. Podendo ser por exemplo um upload de arquivo para o S3, um Stream do Kinesis, um stream do DynamoDB. No nosso caso estaremos utilizando um Api Gateway (Request HTTP) onde definimos o path da request e o method que poderia ser put, get, post. Nesse caso vamos utilizar any.   

### Rodando nossa API
Para executarmos nossa api será necessário executar o comando 
...
$ sls offline 
...
no diretório em que se encontrar o arquivo serverless.yml e deve-se ter um resultado parecido com o seguinte:

<img src="https://miro.medium.com/max/2000/1*AyIwKGjSPtm9XKDwF8_9uA.png"/>

Ao acesso a rota que a CLI nos informa (http://localhost:3000/dev/hi) teremos o seguinte resultado:

<img src="https://miro.medium.com/max/1400/1*rMimnjq68XoC7MRMQl8ydg.png"/>
