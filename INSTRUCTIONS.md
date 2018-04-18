# Instruções

## Dependências
- Puppet
- Ruby >= 2.1

## GitLab-CI

Adicionar ao arquivo .gitlab-ci.yaml:

```
puppet-retrospec:
  stage: test
  before_script:
    - "[ -f 'data/spec.yaml' ] && echo 'Arquivo spec.yaml encontrado' || { echo 'Arquivo spec.yaml não encontrado'; exit 1; }"
  script:
    - gem install bundle
    - gem install puppet
    - gem install puppet-retrospec
    - rm -rf spec/
    - retrospec puppet -s https://github.com/Igorolivei/retrospec-templates
    - bundle install
    - bundle exec rake spec
```

## Hiera

Para utilizar dados do Hiera, deve haver um arquivo `data/spec.yaml` contendo os pares chave/valor.
É importante que a chave tenha o mesmo nome do parâmetro da classe principal, do contrário, o valor não será encontrado.
Ex.:
O manifest init.pp tem como parâmetros as variáveis `key_str` (string) e `key_int` (integer).
O arquivo `spec.yaml` será:
```
---
key_str: 'value'
key_int: 250
```

## Custom facts

Para utilizar fatos customizados, deve have um arquivo `data/custom_facts.yaml` contendo os pares chave/valor. Ex.:

```
---
:fact_one: 'value_one'
# Hash
:fact_two:
  fact_two_one: 'value_two'
  fact_two_two: 'value_three'
```
