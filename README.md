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

Porque ele possui 3 importantes funções.

## 1. Registra nomes de domínio
Seu site precisa de um nome, como example.com. O Route 53 permite que você registre um nome para seu site ou aplicação Web, conhecido como um nome de domínio.

## 2. Roteia tráfego de Internet para os recursos do seu domínio
Quando um usuário abre um navegador da Web e informa seu nome de domínio (example.com) ou nome de subdomínio (acme.example.com) na barra de endereços, o Route 53 ajuda a conectar o navegador com o site ou a aplicação Web.

## 3. Verifica a integridade de seus recursos
O Route 53 envia solicitações automáticas através da Internet a um recurso, como um servidor Web, para verificar se está acessível, disponível e funcional. Você também pode optar por receber notificações quando um recurso se tornar indisponível e optar por desviar o tráfego da Internet dos recursos não íntegros.

# Por que criar uma instância S3?

# Por que criar um LoudBalance?

# Por que criar um Bastion Host?

# Por que criar um EC2?

# Por que criar um RDS?

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

a) 

# Passo-03: Criar um certificado https (essa etapa não poderá ser executada no Leaner Lab)

a) Digite **certificate manager** na lupa do console da AWS

b) No menu vertical esquerdo, clique em **Solicitar certificado** e depois clique em **Solicitar um certificado público**

c) No campo **Nome de domínio totalmente qualificado**, você vai colar o domínio que criaria na etapa Passo-02, que nesse exemplo foi **aulaarquitetura.com**



