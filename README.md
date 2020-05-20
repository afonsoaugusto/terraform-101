# Terraform 101

## Links

* [terraform.io](https://www.terraform.io/)
* [learn-terraform](https://learn.hashicorp.com/terraform)

## Generate apresentation

```sh
docker run -v $PWD:/src afonsoaugusto/markdown-to-slides README.md -d -o presentation.html
chown :$USER presentation.html
rm -rf presentation.html
```

### Agenda

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



### [Dry - Don't repeat yourself](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)

Quando dizemos “como código”,
queremos dizer que todas as boas práticas
que aprendemos no mundo do software
devem ser aplicadas à infraestrutura. Uso
do controle de origem, adesão ao princípio
DRY, modularização, manutenção e uso
de testes e implantação automatizados
são práticas fundamentais.

* [technology-radar-vol-22-pt](https://assets.thoughtworks.com/assets/technology-radar-vol-22-pt.pdf)

## Perguntas