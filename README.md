# serpro

Cliente em [Sea](https://github.com/dopsilva) para as APIs **Consulta CPF** e **Consulta CNPJ** do Serpro.

```sea
local serpro = require("serpro")
local c = serpro.new({key = "consumer-key", secret = "consumer-secret"})

local pessoa = c:cpf("111.444.777-35")             -- dados cadastrais do CPF
local empresa = c:cnpj("00.000.000/0001-91")       -- "basica" (padrão)
local qsa = c:cnpj("00000000000191", "qsa")        -- "basica", "qsa" ou "empresa"
if is_error(pessoa) then print(pessoa.message) end
```

- Token OAuth2 (`client_credentials`) pedido sozinho, guardado e renovado (também após um 401).
- Falhas voltam como valor de erro (`is_error`, `.message`); `?` repassa.
- CPF e CNPJ são validados antes da chamada (dígitos verificadores; CNPJ alfanumérico incluso).
- `serpro.valid_cpf`, `valid_cnpj`, `clean_cpf`, `clean_cnpj` também servem sozinhos.

## Opções de `serpro.new`

| opção | padrão |
|---|---|
| `key`, `secret` | consumer key/secret do contrato (ou `token`, um bearer pronto) |
| `trial` | `false`; `true` usa os serviços `-trial` (dados fictícios) |
| `base_url` | `https://gateway.apiserpro.serpro.gov.br` |
| `token_url` | `base_url/token` |
| `cpf_path` | `/consulta-cpf-df/v1/cpf` |
| `cnpj_path` | `/consulta-cnpj-df/v2` |
| `timeout` | `30000` ms |

O Serpro tem mais de um gateway (o catálogo do gov.br Conecta usa outro host e outro
endpoint de token); se o seu contrato usa um deles, ajuste `base_url`, `token_url` e os paths.
Os padrões não foram testados contra o Serpro real, só contra um servidor simulado.

## Testes

    sea test
