# Desafio  

Considerando as aplicações presentes neste repositório detalhadas abaixo, precisamos de uma stack que permita a disponibilidade de um site
estático em um ambiente de alta disponbilidade. 
  

*  *Aplicação Frontend* - Um default Hello World em HTML
  

*  *Aplicação Backend* - Infra-estrutura automatizada via terraform gerando os seguintes componentes: Bucket S3 no modo site estático, 
configuração da zona de DNS, atacch do sertificado SSL ao domínio utilizado na configuração e a criação da distribuição de cache no Amazon cloudFront.
  

## Como entregar sua solução?

 Pré-Requisitos para utilizar a solução: 
 
 > Instalação do Terraform 12.0.26 a partir do site terraform.io
 > Ter uma conta no serviço da Amazon AWS. 
 > Criar um usuário a partir do serviço IAM e atribuir a ele as permissões de administrador, porém sem acesso a console e 
 com acesso disponível para executar apenas chamadas de API. 
 > Imputar no terraform como variáveis de ambiente a access key e secretkey do usário que será usado no terraform.
 > Ter o git instalado 
 > Realizar a instalação do aws cli tendo como usuário o mesmo que fara as execuções do Terraform
 
 
  
1) Clone do repositório 

> Executar o comando git clone preferencialmente a partir de uma máquina linux  :
git clone https://github.com/EzequiasEvangelista/case-infra-itau.git

Este comando fará o download do ambiente da aplicação para sua máquina local.
 
  
2) Realize as alterações necessárias para construção/automação da stack. A configuração das variáveis de ambiente contendo
a secret key e a access key gerada no painel da AWS. 

  

3) Para prosseguir com a criação da infra via Terraform, execute os seguintes comandos : 

Realizar instalação dos plugins do Terraform

> $ terraform init     

> $ terraform plan    vai exibir todos os componentes que serão gerados na stack e suas possíveis modificações 

> terraform apply   vai aplicar o conteúdo visualizado no terraform plan  e gerar o tsstate que é uma espécie de log da stack.

> Caso queira se desfazer desta infra, poderá ainda executar o terraform destroy, porém com um adendo. Como se trata de um site estático
em um bucket S3, é necessário acessar o bucket para apagar o index.html que está dentro dele devido a uma limitação da AWS
que não permite apagar um bucket que tenha algum conteúdo dentro dele. 



## Tecnologias Envolvidas para criação e execução da aplicação : 

> Desktop com Linux Ubuntu 18
> Editor de textos VsCode
> Terraform para criar a infra como código 
> Infra estrutra de nuvem Pública criada na Amazon Web Services
> Servidores de CI/CD Jenkins 
> Repositório de código GitHub
> Compra de um registro DNS para o nome do site (opcional)


* Sinta-se a vontade para realizar melhorias no código das aplicações, caso julgue necessário.  

## Dúvidas  

Entre em contato e nos questione.

## Outras alternativas para execução deste projeto, com um adendo para a elevação de custos *

* Este mesmo projeto poderia ser criado em um ElasticBeanStalk na AWS caso fosse uma aplicação dinâmica e que uma volumetria de
acessos considerável. Teria recursos como auto-scaling (up e down de máquinas), enviroment para os mais diversos tipos de aplicação
tais como Java, php, node, e etc . Também poderia ser criado via automação do CloudFormation ou também com módulos do Terraform.


* Poderia tambér ser, em caso de aplicação dinâmica, um cluster de instâncias EC2 configuradas atrás de um balanceador de carga
em zonas de disponibilidade distintas, com auto-scaling configurado, com security grou aplicados, com segregação das instâncias 
em VPC's e subnestes e com trigger do cloudWatch habilitada para fazer o scale up down do ambiente e tendo seus estados mantidos
por uma automação Ansible ou Puppet. 

* Bom, como se tratava de uma aplicação pequena e visando alta disponibilidade mas com a variável preço influênciando na decisão, 
optei pela alta disponibilidade do S3 e a aplicação de uma camada de cache via cloudFront para que as requisições não cheguem 
o tempo todo no S3 uma vez que o serviço é bilhetado por quantidade de requisições e não por ocupação do espaço que disponibiliza. 

