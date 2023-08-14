# ARQUITETURA CORPORATIVA

Nessa instrução, vamos explorar a arquitetura corporativa (apresentada na imagem a seguir), que suporta aplicações Web escaláveis de forma a atender milhares a milhões de usuários contemplando também a segurança destas aplicações utilizando um único ponto de contato (load balancer), e como fazer o gerenciamento de fluxos de trabalho escaláveis (load balancer).

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/ARQUITETURA/blob/main/imgs/arquitetura_avan%C3%A7ada.png">
   <img alt="Arquitetura Inicial" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/ARQUITETURA/blob/main/imgs/arquitetura_avan%C3%A7ada.png)">
</picture>


Nos auto-estudos, o aluno terá visto:
Amazon Route 53
Cloud Front
Redes VPC

# Por que criar uma VPC?

A VPC é uma rede virtual definida por você dentro da infra da AWS que permite executar, praticamente, todos os recursos disponíveis na AWS. Contudo, por exemplo, Amazon Route 53, Amazon CloudFront e AWS Direct Connect geralmente ficam de fora da VPC porque são serviços que se conectam para fora da AWS.

Uma VPC pode abranger várias zonas de disponibilidade.

Dentro da VPC, vamos criar um IGW (Internet Gateway), que permite a comunicação entre instâncias na VPC e a Internet externa.

Depois de criar a VPC, vamos adicionar sub-redes dentro de uma mesma zona de disponibilidade. Se o tráfego de uma sub-rede for roteado para um gateway da Internet, a sub-rede será chamada de *sub-rede pública*. Se a sub-rede não tiver uma rota para o gateway da Internet, ele será chamada de *sub-rede privada*.

O assistente também criará um gateway NAT (Network Address Translation ou Conversão de Endereços de Rede) que é usado para fornecer conectividade com a Internet para instâncias do EC2 nas sub-redes privadas, mas a Internet não consegue acessar a instância EC2.

# Por que criar um AWS Route 53?

É um **serviço** com 3 importantes funções:

## 1. Registra nomes de domínio
Seu site precisa de um nome, como example.com. O Route 53 permite que você registre um nome para seu site ou aplicação Web, conhecido como um nome de domínio.

## 2. Roteia tráfego de Internet para os recursos do seu domínio
Quando um usuário abre um navegador da Web e informa seu nome de domínio (example.com) ou nome de subdomínio (acme.example.com) na barra de endereços, o Route 53 ajuda a conectar o navegador com o site ou a aplicação Web.

## 3. Verifica a integridade de seus recursos
O Route 53 envia solicitações automáticas através da Internet a um recurso, como um servidor Web, para verificar se está acessível, disponível e funcional. Você também pode optar por receber notificações quando um recurso se tornar indisponível e optar por desviar o tráfego da Internet dos recursos não íntegros.

# Por que criar uma instância S3?

# Por que criar um Amazon Load Balance?

# Por que criar um Bastion Host?

# Por que criar um EC2?

# Por que criar um RDS?

# Fluxo:
## Criar um domínio (exemplo, goDaddy ou [RegistroBr](https://registro.br/)
## Registrar o domínio recém criado no Zonas de Hospedagem do Route 53 (custo de U$0,50/mês)
## Regitrar os nomes dos servidores Route 53 no GoDaddy
## Criar um certificado público https no Certificate Manage para o domínio goDaddy
## Abrir um serviço S3 para um site estático
## CloudFront


# Passo-01:

a) Busque por VPC no console da AWS;

b) Clique no botão laranja CRIAR;

c) Selecione **VPC e muito mais**

d) No campo **Gerar Automaticamente** digite **ArquiteturaCorp** e pule algumas configurações

e) Bloco CIDR IPV4 deixa o 10.0.0.0/16 --> agora pense! O que isso significa?

f) Na opção Bloco CIDR IPv6, deixe como **Nenhum bloco IPV6** que significa que não vamos criar subrede usando IPV6

g) Em locação, deixe **padrão**

h) Em **Número de zonas de disponibilizadas (AZs)** deixe em **2** e clicando em **Personalizar AZs**, note que as duas zonas disponíveis para você: **us-east-1a** e **us-east-1b**.

i) Em **Número de sub-redes públicas** e **sub-redes privadas**, deixe **2** em cada. Note que à direita, você tem 4 sub-redes, sendo 2 privadas e 2 públicas e elas estão equitariamente distribuídas.
em
j) Em **Gateway NAT**, você pode escolher **nehum**, **Em 1 AZ** e **1 por AZ**. Esse item é cobrado já de cara. Então, vamos adotar o **Em 1 AZ**.

k) Em **Endpoints da VPC**, você pode escolher o **Gateway S3** para reduzir a cobrança sobre o NAT.

l) Você verá um fluxo de trabalho VPC que é na verdade um checklist com vários itens sendo criado.


# Passo-02: Criação do Route 53 (essa etapa não poderá ser executada no Leaner Lab)

Cada domínio criado no AWS gera um custo real de U$0,50 e por isso, não está disponível no Leaner Lab. Já no domínio particular da AWS, sim é possível criar um Route 53, mas precisa ter um cartão de crédito cadastrado para pagar os 50 centavos de dolar.

Embora seja pago, é importante você conhecer as etapas desse importante passo.

Usando o site [goDaddy](https://www.godaddy.com/) (que é um site criador de domínios pagos), o professor criou o domínio **aulaarquitetura.com** para testes.

# Passo-03: Criando um serviço S3
## Por padrão, todo S3 é totalmente bloqueado. Sua função agora é liberar as devidas funções dele.

a) Digite S3 na lupa do console.

b) Clique em **Criar bucket** (botão laranja).

c) No campo **Nome do bucket** digite **arquiteturacorp**. Mas atenção: não pode ter letras maiúsculas e nem começar o nome com número.

d) Em região, aponte para o **us-east-1**.

e) Deixa todas as demais configurações básicas como estão e clique no botão laranja **Criar bucket**.

f) Retorne em suas instâncias de buckets e clique no bucket que você acabou de criar (link azul) para configurá-los. Note que ele está como **Bucket e objetos não públicos**.

f.1) Dentro das configurações do Bucket, procure pela aba **Propriedades**, vá até o final da página e em **Hospedagem de site estático**, clique em **Editar** e depois, **ativar**.

f.2) Em **Tipo de hospedagem** deixe em **Hospedar um site estático**.

f.3) Em **Documento de índice** coloque o nome do arquivo-fonte do seu site, que nesse caso, pode ser **index.html**. Você deve salvar esse código abaixo em arquivo texto tipo *html* e vai usar numa etapa futura, não agora. Então, apenas salve esse código num arquivo **index.html** aí no seu HD local.

```
<!DOCTYPE html>
<html lang="pt-br">
	<head>
		<meta charset="utf-8">
		<title>Calculadora</title>		
		<script src="calculadora.js"></script>
	</head>
	<body bgcolor="beige">
		<h1>Calculadora 1.0</h1>
		<input type="text" id="txtValor1">
			<select id="operadores" onchange="ControleDeSelecao();">
				<optgroup label="Basico">
					<option value="+">	+(somar)</option>
					<option value="-">	-(subtrair)</option>
					<option value="*">	*(multiplicar)</option>
					<option value="/">	/(dividir)</option>
				</optgroup>
				<optgroup label="Outros">
					<option value="raiz">	 	  Raiz	</option>
					<option value="potencia">	Potência</option>
					<option value="fatorial">	Fatorial</option>
					<option value="fibonacci">Fibonaci</option>					
					<option value="porcento">	Porcentagem</option>
					<option value="media">		Média</option>
					<option value="calc">		  Develop Calc</option>
				</optgroup>
			</select>
		<input type="text" id="txtValor2" size="5"><br>		
		<input type="button" onclick="Calcular('txtValor1', 'txtValor2')" value="Calcular">
		<input type="button" onclick="Limpar('txtValor1', 'txtValor2')" value="Limpar">
		<p id="saida">Resultado:</p>
		<hr>		
	</body>
</html>
```

f.4) No campo **Documento de erro - opcional** você pode deixar em branco ou elaborar uma página personalizada para quando der um erro em seu site ou até usar a mesma arquivo **index.html**. Para hoje, deixe esse campo em branco

f.5) Clique no botão laranja no final da página para confirmar.

f.6) Se você retornar em **Propriedades** após a confirmação, você terá o link do seu S3 instanciado, algo assim: **http://arquiteturacorp.s3-website-us-east-1.amazonaws.com/**, e se você clicar nesse link, constará **erro 403 Forbidden, Code: AccessDenied**.

g) Você pode carregar um arquivo qualquer no seu S3, clicando em **Objetos**. E ainda pode criar uma pasta qualquer chamada **Teste**. Então, suba um arquivo qualquer e crie uma pasta qualquer em seu S3. Veja a figura a seguir que demonstra como fica o seu HD virtual lá na AWS.


<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/ARQUITETURA/blob/main/imgs/objetos.png">
   <img alt="Objetos" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/ARQUITETURA/blob/main/imgs/objetos.png)">
</picture>


h) Nessa etapa, vamos definir as permissões de acesso ao S3. Por padrão, ele totalmente bloqueado. Clique na aba **Permissões** e perceba que em **Visão geral das permissões** que está como **bucket e objetos não públicos**. Isso significa que o seu S3 ou site não está visível externamente. Então, em **Bloquear acesso público**, você clica em **Editar** e desmarque a opção **Bloquear todo o acesso público** e clique no botão laranja **Salvar alterações**. Note que aparecerá uma tela de confirmação novamente, conforme abaixo, onde você vai ter que digitar **confirmar**. Note que os vazamentos de dados ocorrem devido às configurações erradas e não *sem querer querendo*.


<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/ARQUITETURA/blob/main/imgs/confirmacao_desbloqueio.png">
   <img alt="Confirmação" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/ARQUITETURA/blob/main/imgs/confirmacao_desbloqueio.png)">
</picture>

Agora, vá novamente na sua lista de Buckets e veja como está o **Acesso** na frente do seu bucket. Ele deve estar assim: **Os objetos podem ser públicos**, igual ao da imagem abaixo, então o S3 ainda não é público, mas pode ser...

<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/ARQUITETURA/blob/main/imgs/podem_ser_publicos.png">
   <img alt="Podem Ser Públicos" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/ARQUITETURA/blob/main/imgs/podem_ser_publicos.png)">
</picture>

i) Agora, vamos gerar uma **Apólice do bucket**, que vai liberar os acessos ao seu S3. Para isso, clique em **Permissões** do seu bucket e em, **Política do bucket**, clique em **Editar** e depois **Gerador de Apólices**. Daí você vai ver uma tela como a seguir:


<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/ARQUITETURA/blob/main/imgs/gerador_politica.png">
   <img alt="Gerador de Política" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/ARQUITETURA/blob/main/imgs/gerador_politica.png)">
</picture>


j) Em **Step 1 - Select Policy Type**, selecione **S3 Bucket Policy**, em **Step 2 - Add Statements(s)**, marque **Allow** em **Principal**, coloque asterisco para indicar qualquer objeto. Em **Actions** escolha **GetObjects** que serve para liberar acesso aos objetos do seu site, para aparecer na sua tela do navegador. Mas note que não será possível deletar, atualizar ou colocar nada (do CRUD, somente o R - Read estará liberado). No campo **ARN (Amazon Resource Name)** você pega lá no seu bucket na aba **Propriedades**. Tem que ser algo do tipo *arn:aws:s3:::arquiteturacorp*. MAS ATENÇÃO! Adicione um /* logo após seu ARN. Então ficaria algo assim:


### arn:aws:s3:::arquiteturacorp/*

Por fim, clique em **Add Statement** e depois, confirme em **Generate Policy** para gerar sua política. O resultado será um arquivo JSON com sua apólice, algo do tipo:
```
{
  "Id": "Policy1692046481274",
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1692046458290",
      "Action": [
        "s3:GetObject"
      ],
      "Effect": "Allow",
      "Resource": "arn:aws:s3:::arquiteturacorp/*",
      "Principal": "*"
    }
  ]
}
```

### Observe que seu bucket agora está público, e com um ícone de atenção.

Para fazer um teste, acesse o link do seu S3 e verá que o erro terá mudado agora. Ele está acessível externamente, mas não tem o arquivo **index.html**. Veja a imagem a seguir para entender melhor.


<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/ARQUITETURA/blob/main/imgs/erro404-sem-indexHTML.png">
   <img alt="Gerador de Política" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/ARQUITETURA/blob/main/imgs/erro404-sem-indexHTML.png)">
</picture>


K) Agora, basta você colocar um arquivo index.html dentro do seu S3 como se fosse um objeto. Arraste o seu index.html para dentro e salva. Veja a imagem de como fica no final:


<picture>
   <source media="(prefers-color-scheme: light)" srcset="https://github.com/agodoi/ARQUITETURA/blob/main/imgs/index-html-S3.png">
   <img alt="Gerador de Política" src="[YOUR-DEFAULT-IMAGE](https://github.com/agodoi/ARQUITETURA/blob/main/imgs/index-html-S3.png)">
</picture>

Agora você pode atualizar o seu link do S3 que o seu site estático estará no ar.


## Agora você poderia entrar no serviço Route 53...

para apontar o endereço http://arquiteturacorp.s3-website-us-east-1.amazonaws.com/ para o endereço pago **aulaarquitetura.com**. Note também que o site está como HTTP e não HTTPS. Para tornar o seu seu site seguraro com o protocolo HTTPS, precisa criar uma certificação...

# Passo-extra: Criar um certificado https (essa etapa não poderá ser executada no Leaner Lab)

a) Digite **certificate manager** na lupa do console da AWS

b) No menu vertical esquerdo, clique em **Solicitar certificado** e depois clique em **Solicitar um certificado público**

c) No campo **Nome de domínio totalmente qualificado**, você vai colar o domínio que criaria na etapa Passo-02, que nesse exemplo foi **aulaarquitetura.com**


# Passo-04: Criando uma CloudFront
## Serve para manter dados do site em cache, aumentar velocidade e minimizar latências ao redor do mundo

Sua principal função é melhorar a velocidade e a disponibilidade de entrega de conteúdo, como imagens, vídeos, scripts e outros arquivos, aos usuários finais em diferentes regiões do mundo.

Ao usar o CloudFront, os arquivos são armazenados em pontos de presença globais da AWS, chamados de "edge locations". Quando um usuário solicita um arquivo, o CloudFront direciona essa solicitação para o servidor de *edge location* mais próximo do usuário, o que reduz a latência e melhora a velocidade de carregamento do conteúdo. Além disso, o CloudFront ajuda a reduzir a carga nos servidores de origem, pois ele armazena em cache os arquivos e responde diretamente a solicitações subsequentes que podem ser atendidas a partir desses caches.


