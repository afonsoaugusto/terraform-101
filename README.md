# Terraform 101 

![logo](img/logo-2.png)

## Links

**Documentação**

* [terraform.io](https://www.terraform.io/)
* [learn-terraform](https://learn.hashicorp.com/terraform)
* [Documentation](https://www.terraform.io/docs/configuration)

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
  * Locals
  * Funções
* Providers
* Resources
* Data
* Modulos
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

O terraform é desenvolvido em GO e é [openSource](https://github.com/hashicorp/terraform/blob/master/LICENSE)

### Terraform-flow

![Terraform](img/terraform-flow.png)

### Arquivos

* *.tf
* *.tfvars
* *.plan
* *.TFSTATE

## Terraform Basics

### Variables

[Variaveis](https://www.terraform.io/docs/configuration/variables.html) são parametros para um modulo de Terraform, permitem que aspectos da implementação seja customizado sem que seu codigo seja alterado.

Temos 3 tipos de uso de variaveis:
* Input variables are like function arguments
* Output values are like function return values
* Local values are like a function's temporary local variables. 

#### Declarando uma variavel de input

```hcl
variable "image_id" {
  type = string
}
```
```hcl
variable "availability_zone_names" {
  type    = list(string)
  default = ["us-west-1a"]
}
```
```hcl
variable "docker_ports" {
  type = list(object({
    internal = number
    external = number
    protocol = string
  }))
  default = [
    {
      internal = 8300
      external = 8300
      protocol = "tcp"
    }
  ]
}
```
```hcl
variable "image_id" {
  type        = string
  description = "The id of the machine image (AMI) to use for the server."
}
```
```hcl
variable "image_id" {
  type        = string
  description = "The id of the machine image (AMI) to use for the server."

  validation {
    condition     = length(var.image_id) > 4 && substr(var.image_id, 0, 4) == "ami-"
    error_message = "The image_id value must be a valid AMI id, starting with \"ami-\"."
  }
}
```

#### Precedencia de carregamento

* Environment variables
* The terraform.tfvars file, if present.
* The terraform.tfvars.json file, if present.
* Any *.auto.tfvars or *.auto.tfvars.json files, processed in lexical order of their filenames.
* Any -var and -var-file options on the command line, in the order they are provided. (This includes variables set by a Terraform Cloud workspace.)

### Locals

Um Local é um valor que recebe um nome a partir de uma expressão ou valor.

```hcl
locals {
  # Ids for multiple sets of EC2 instances, merged together
  instance_ids = concat(aws_instance.blue.*.id, aws_instance.green.*.id)
}

locals {
  # Common tags to be assigned to all resources
  common_tags = {
    Service = local.service_name
    Owner   = local.owner
  }
}
```

### Output

Output é uma forma de expor o valor, seja como retorno de um modulo ou imprimindo como retorno do root.

```hcl
output "instance_ip_addr" {
  value = aws_instance.server.private_ip
}
```

```hcl
output "instance_ip_addr" {
  value       = aws_instance.server.private_ip
  description = "The private IP address of the main server instance."
}
```

```hcl
output "db_password" {
  value       = aws_db_instance.db.password
  description = "The password for logging in to the database."
  sensitive   = true
}
```

```hcl
output "instance_ip_addr" {
  value       = aws_instance.server.private_ip
  description = "The private IP address of the main server instance."

  depends_on = [
    # Security group rule must be created before this IP address could
    # actually be used, otherwise the services will be unreachable.
    aws_security_group_rule.local_access,
  ]
}
```

### [Type Constraints](https://www.terraform.io/docs/configuration/variables.html#type-constraints)

São os tipos que as variaveis podem receber como argumentos na declaração.

#### Simples

* string
* number
* bool

#### Complexos

* list(<TYPE>)
* set(<TYPE>)
* map(<TYPE>)
* object({<ATTR NAME> = <TYPE>, ... })
* tuple([<TYPE>, ...])

### [Functions](https://www.terraform.io/docs/configuration/functions.html)

* Numeric Functions
* String Functions
* Collection Functions
* Encoding Functions
* Filesystem Functions
* Date and Time Functions
* Hash and Crypto Functions
* IP Network Functions
* Type Conversion Functions

#### Principais funções

##### [Format](https://www.terraform.io/docs/configuration/functions/format.html)

`format(spec, values...)`

```hcl
> format("Hello, %s!", "Ander")
Hello, Ander!
> format("There are %d lights", 4)
There are 4 lights
```
```hcl
> format("Hello, %s!", var.name)
Hello, Valentina!
> "Hello, ${var.name}!"
Hello, Valentina!
```

##### [Join](https://www.terraform.io/docs/configuration/functions/join.html)

`join(separator, list)`

```hcl
> join(", ", ["foo", "bar", "baz"])
foo, bar, baz
> join(", ", ["foo"])
foo
```

##### [Split](https://www.terraform.io/docs/configuration/functions/split.html)

`split(separator, string)`

```hcl
> split(",", "foo,bar,baz")
[
  "foo",
  "bar",
  "baz",
]
> split(",", "foo")
[
  "foo",
]
> split(",", "")
[
  "",
]
```

##### [Upper](https://www.terraform.io/docs/configuration/functions/upper.html), [lower](https://www.terraform.io/docs/configuration/functions/lower.html), [title](https://www.terraform.io/docs/configuration/functions/title.html)

```hcl
> upper("hello")
HELLO
> upper("алло!")
АЛЛО!
```

```hcl
> lower("HELLO")
hello
> lower("АЛЛО!")
алло!
```

```hcl
> title("hello world")
Hello World
```

##### [Element](https://www.terraform.io/docs/configuration/functions/element.html) [Index](https://www.terraform.io/docs/configuration/functions/index.html)

`element(list, index)`

```hcl
> element(["a", "b", "c"], 1)
b
```

`index(list, value)`

```hcl
> index(["a", "b", "c"], "b")
1
```

##### [Map](https://www.terraform.io/docs/configuration/functions/map.html)

* From Terraform v0.12, the Terraform language has built-in syntax for creating maps using the { and } delimiters. Use the built-in syntax instead. The map function will be removed in a future version of Terraform.

```hcl
> map("a", "b", "c", "d")
{
  "a" = "b"
  "c" = "d"
}
```

```hcl
> {"a" = "b", "c" = "d"}
{
  "a" = "b"
  "c" = "d"
}
```

## Modulos para analise

* msk
* s3
* cloudfront-distribution
* spotinst-elastigroup
* secrets-manager
* documentdb
* dynamodb-table

## Perguntas
