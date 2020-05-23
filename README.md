# Terraform 101 

![logo](img/logo-2.png)

## Links

**Documentação**

* [terraform.io](https://www.terraform.io/)
* [learn-terraform](https://learn.hashicorp.com/terraform)

**cursos**

* [Alura - infraestrutura-como-codigo](https://cursos.alura.com.br/category/infraestrutura#infraestrutura-como-codigo)
* [Alura - terraform](https://cursos.alura.com.br/course/terraform)
* [Linuxacademy - Managing Applications and Infrastructure with Terraform](https://linuxacademy.com/cp/modules/view/id/175)

## Generate apresentation

```sh
docker run -v $PWD:/src afonsoaugusto/markdown-to-slides \
    README.md -d -o presentation.html; \
    chown :$USER presentation.html
rm -rf presentation.html
```

## Agenda

* Fundamentos de IaC
* Terraform Basics
  * Variaveis
  * Outupts
  * Funções
* Providers
* Resources
* Data
* Modulos
* Locals
* Workspaces
* Backend - TFSTATE
  * Estrutura do bucket s3 utilizado
* Tarraform CLI (apply, destroy, state, , taint, import)
* Terraform Best Pratices

## Fundamentos de IaC

### Infrastructure as Code

[Infraestrutura como código](https://pt.wikipedia.org/wiki/Infraestrutura_como_C%C3%B3digo) (em inglês: infrastructure as code, ou IaC) é o processo de gerenciamento e provisionamento de [centros de processamentos dados](https://pt.wikipedia.org/wiki/Centro_de_processamento_de_dados) usando [arquivos](https://pt.wikipedia.org/wiki/Arquivo_de_computador) de configuração ao invés de configurações físicas de hardware ou [ferramentas de configuração interativas](https://pt.wikipedia.org/wiki/Interface_gr%C3%A1fica_do_utilizador).

### IaC

![IaC](img/IaC.png)

### [Dry - Don't repeat yourself](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)

Quando dizemos “como código”, queremos dizer que todas as boas práticas que aprendemos no mundo do software devem ser aplicadas à infraestrutura.

Uso do controle de versão, adesão ao princípio DRY, modularização, manutenção e uso de testes e implantação automatizados são práticas fundamentais.

* [technology-radar-vol-22-pt](https://assets.thoughtworks.com/assets/technology-radar-vol-22-pt.pdf)

### [Casos de Uso](https://www.terraform.io/intro/use-cases.html)

* Heroku App Setup
* Multi-Tier Applications
* Self-Service Clusters
* Multi-Cloud Deployment

## Terraform

### Terraform-1

Terraform é uma ferramenta open source de **provisionamento** de infraestrutura, criada pela [HashiCorp](https://www.hashicorp.com/), que permite que definamos nossa infraestrutura como código([IaC](https://en.wikipedia.org/wiki/Infrastructure_as_Code)), usando uma linguagem simples e declarativa.

### Terraform-flow

![Terraform](img/terraform-flow.png)

### Arquivos

* *.tf
* *.tfvars
* *.plan
* *.TFSTATE

## Perguntas
