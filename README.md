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

# Como criar uma VPC e por que?

A VPC é uma rede virtual definida por você dentro da infra da AWS que permite executar, praticamente, todos os recursos disponíveis na AWS. Contudo, por exemplo, Amazon Route 53, Amazon CloudFront e AWS Direct Connect geralmente ficam de fora da VPC porque são serviços que se conectam para fora da AWS.

Uma VPC pode abranger várias zonas de disponibilidade.
VPC
