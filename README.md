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

Initializing the backend...

Initializing provider plugins...

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration, with the constraint strings
suggested below.

* provider.aws: version = "~> 2.65"


Warning: Interpolation-only expressions are deprecated

  on bucket-s3.tf line 3, in resource "aws_s3_bucket" "website":
   3:   bucket = "${var.domain-name}"

Terraform 0.11 and earlier required all non-constant expressions to be
provided via interpolation syntax, but this pattern is now deprecated. To
silence this warning, remove the "${ sequence from the start and the }"
sequence from the end of this expression, leaving just the inner expression.

Template interpolation syntax is still used to construct strings from
expressions when the template includes multiple interpolation sequences or a
mixture of literal strings and interpolations. This deprecation applies only
to templates that consist entirely of a single interpolation sequence.

(and 10 more similar warnings elsewhere)

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.

Mostra uma visão de todos os componentes que serão gerados na stack e suas possíveis modificações 

> $ terraform plan

Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

data.aws_route53_zone.route53zone: Refreshing state...
aws_cloudfront_origin_access_identity.access_identity: Refreshing state... [id=E1LT7UQPM9H4A1]
aws_s3_bucket.website: Refreshing state... [id=pocdeinfra.com]
aws_cloudfront_distribution.distribution: Refreshing state... [id=EOWUMTMDXRG0H]
aws_route53_record.www-domain: Refreshing state... [id=Z01448342YUL4LKJ1PF7C_www.pocdeinfra.com_A]
aws_route53_record.domain: Refreshing state... [id=Z01448342YUL4LKJ1PF7C_pocdeinfra.com_A]

------------------------------------------------------------------------

No changes. Infrastructure is up-to-date.

This means that Terraform did not detect any differences between your
configuration and real physical resources that exist. As a result, no
actions need to be performed.

Warning: Interpolation-only expressions are deprecated

  on bucket-s3.tf line 3, in resource "aws_s3_bucket" "website":
   3:   bucket = "${var.domain-name}"

Terraform 0.11 and earlier required all non-constant expressions to be
provided via interpolation syntax, but this pattern is now deprecated. To
silence this warning, remove the "${ sequence from the start and the }"
sequence from the end of this expression, leaving just the inner expression.

Template interpolation syntax is still used to construct strings from
expressions when the template includes multiple interpolation sequences or a
mixture of literal strings and interpolations. This deprecation applies only
to templates that consist entirely of a single interpolation sequence.

(and 10 more similar warnings elsewhere)

Vai aplicar o conteúdo visualizado no terraform plan  e gerar o tfstate que é uma espécie de log da stack.

> terraform apply   

data.aws_route53_zone.route53zone: Refreshing state...

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # aws_cloudfront_distribution.distribution will be created
  + resource "aws_cloudfront_distribution" "distribution" {
      + active_trusted_signers         = (known after apply)
      + aliases                        = [
          + "pocdeinfra.com",
          + "www.pocdeinfra.com",
        ]
      + arn                            = (known after apply)
      + caller_reference               = (known after apply)
      + default_root_object            = "index.html"
      + domain_name                    = (known after apply)
      + enabled                        = true
      + etag                           = (known after apply)
      + hosted_zone_id                 = (known after apply)
      + http_version                   = "http2"
      + id                             = (known after apply)
      + in_progress_validation_batches = (known after apply)
      + is_ipv6_enabled                = false
      + last_modified_time             = (known after apply)
      + price_class                    = "PriceClass_All"
      + retain_on_delete               = false
      + status                         = (known after apply)
      + wait_for_deployment            = true

      + default_cache_behavior {
          + allowed_methods        = [
              + "DELETE",
              + "GET",
              + "HEAD",
              + "OPTIONS",
              + "PATCH",
              + "POST",
              + "PUT",
            ]
          + cached_methods         = [
              + "GET",
              + "HEAD",
            ]
          + compress               = false
          + default_ttl            = 3600
          + max_ttl                = 86400
          + min_ttl                = 0
          + target_origin_id       = "origin"
          + viewer_protocol_policy = "redirect-to-https"

          + forwarded_values {
              + query_string = false

              + cookies {
                  + forward = "none"
                }
            }
        }

      + origin {
          + domain_name = (known after apply)
          + origin_id   = "origin"

          + s3_origin_config {
              + origin_access_identity = (known after apply)
            }
        }

      + restrictions {
          + geo_restriction {
              + locations        = [
                  + "BR",
                  + "CA",
                  + "DE",
                  + "GB",
                  + "US",
                ]
              + restriction_type = "whitelist"
            }
        }

      + viewer_certificate {
          + acm_certificate_arn      = "arn:aws:acm:us-east-1:492909730045:certificate/8c1845c3-5ace-4e56-9738-1393967afbe9"
          + minimum_protocol_version = "TLSv1"
          + ssl_support_method       = "sni-only"
        }
    }

  # aws_cloudfront_origin_access_identity.access_identity will be created
  + resource "aws_cloudfront_origin_access_identity" "access_identity" {
      + caller_reference                = (known after apply)
      + cloudfront_access_identity_path = (known after apply)
      + comment                         = "Access for cloudfront"
      + etag                            = (known after apply)
      + iam_arn                         = (known after apply)
      + id                              = (known after apply)
      + s3_canonical_user_id            = (known after apply)
    }

  # aws_route53_record.domain will be created
  + resource "aws_route53_record" "domain" {
      + allow_overwrite = (known after apply)
      + fqdn            = (known after apply)
      + id              = (known after apply)
      + name            = "pocdeinfra.com"
      + type            = "A"
      + zone_id         = "Z01448342YUL4LKJ1PF7C"

      + alias {
          + evaluate_target_health = false
          + name                   = (known after apply)
          + zone_id                = (known after apply)
        }
    }

  # aws_route53_record.www-domain will be created
  + resource "aws_route53_record" "www-domain" {
      + allow_overwrite = (known after apply)
      + fqdn            = (known after apply)
      + id              = (known after apply)
      + name            = "www.pocdeinfra.com"
      + type            = "A"
      + zone_id         = "Z01448342YUL4LKJ1PF7C"

      + alias {
          + evaluate_target_health = false
          + name                   = (known after apply)
          + zone_id                = (known after apply)
        }
    }

  # aws_s3_bucket.website will be created
  + resource "aws_s3_bucket" "website" {
      + acceleration_status         = (known after apply)
      + acl                         = "public-read"
      + arn                         = (known after apply)
      + bucket                      = "pocdeinfra.com"
      + bucket_domain_name          = (known after apply)
      + bucket_regional_domain_name = (known after apply)
      + force_destroy               = false
      + hosted_zone_id              = (known after apply)
      + id                          = (known after apply)
      + policy                      = jsonencode(
            {
              + Statement = [
                  + {
                      + Action    = [
                          + "s3:GetObject",
                        ]
                      + Effect    = "Allow"
                      + Principal = {
                          + AWS = "*"
                        }
                      + Resource  = [
                          + "arn:aws:s3:::pocdeinfra.com/*",
                        ]
                      + Sid       = "AllowPublicRead"
                    },
                ]
              + Version   = "2008-10-17"
            }
        )
      + region                      = (known after apply)
      + request_payer               = (known after apply)
      + website_domain              = (known after apply)
      + website_endpoint            = (known after apply)

      + cors_rule {
          + allowed_headers = [
              + "*",
            ]
          + allowed_methods = [
              + "PUT",
              + "POST",
            ]
          + allowed_origins = [
              + "*",
            ]
          + expose_headers  = [
              + "ETag",
            ]
          + max_age_seconds = 3000
        }

      + versioning {
          + enabled    = (known after apply)
          + mfa_delete = (known after apply)
        }

      + website {
          + error_document = "index.html"
          + index_document = "index.html"
        }
    }

Plan: 5 to add, 0 to change, 0 to destroy.


Warning: Interpolation-only expressions are deprecated

  on bucket-s3.tf line 3, in resource "aws_s3_bucket" "website":
   3:   bucket = "${var.domain-name}"

Terraform 0.11 and earlier required all non-constant expressions to be
provided via interpolation syntax, but this pattern is now deprecated. To
silence this warning, remove the "${ sequence from the start and the }"
sequence from the end of this expression, leaving just the inner expression.

Template interpolation syntax is still used to construct strings from
expressions when the template includes multiple interpolation sequences or a
mixture of literal strings and interpolations. This deprecation applies only
to templates that consist entirely of a single interpolation sequence.

(and 10 more similar warnings elsewhere)

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes


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

## Configuração das Pipelines do Jenkins

A pipeline a seguir faz o deploy da index.html no bucket S3

pipeline {
  agent any
  stages {
    stage("Acessa o bucket") {
      steps {
        sh 'aws s3 ls pocdeinfra.com'
      }
    }
    
    stage("Remove versao anterior") {
      steps {
        sh 'aws s3 rm s3://pocdeinfra.com/index.html --recursive'
      }
    }
    
    stage("Deploy") {
      steps {
        sh 'aws s3 cp /home/ezequias/Documentos/projeto-infra/index.html s3://pocdeinfra.com/'
      }
    }
  }
}



A pipeline a seguir faz a criação da infra-estrutura invocando os scripts gerados no Terraform

pipeline {
  agent any
 
  stages {
    stage('Terraform Init') {
      steps {
        sh "terraform init /home/ezequias/Documentos/projeto-infra"
      }
    }
    
    stage('Terraform Plan') {
      steps {
        sh "terraform plan /home/ezequias/Documentos/projeto-infra "
      }
    }
    
    stage('Terraform Apply') {
      steps {
        input 'Apply Plan'
        sh "terraform apply -auto-approve -input=false /home/ezequias/Documentos/projeto-infra"
      }
    }
    
  }
}


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

