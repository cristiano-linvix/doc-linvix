# Rota Emitir (NFe/NFCe)

**POST** /nfe/emitir

**POST** /nfce/emitir

Visualize a documentação completa com todos os campos disponiveis no layout

[Campos NFe](NFe.pdf)

[Campos NFCe](NFCe.pdf)


## Envio

Os payloads podem ser criados usando arrays, exemplo:

```php
$post = [
    "ambiente" => "P",
    "natureza_operacao" => "VENDA DENTRO DO ESTADO",
    "serie" => "1",
    "numero" => "1035",
    "data_emissao" => "2020-10-15T03:00:00-03:00",
    "data_entrada_saida" => "2020-10-15T03:00:00-03:00",
    "tipo_operacao" => "1",
    "local_destino" => "1",
    "finalidade_emissao" => "1",
    "consumidor_final" => "0",
    "presenca_comprador" => "9",
    "notas_referenciadas" => [],
    "destinatario" => [
        "cnpj" => "15493535500128",
        "nome" => "EMPRESA MODELO",
        "indicador_inscricao_estadual" => "1",
        "inscricao_estadual" => "212055510",
        "endereco" => [
            "logradouro" => "AVENIDA TESTE",
            "numero" => "444",
            "bairro" => "CENTRO",
            "codigo_municipio" => "2408003",
            "nome_municipio" => "Mossoro",
            "uf" => "RN",
            "cep" => "59653120",
            "codigo_pais" => "1058",
            "nome_pais" => "BRASIL",
            "telefone" => "8499995555"
        ]
    ],
    "itens" => [
        [
            "numero_item" => "1",
            "codigo_produto" => "000297",
            "descricao" => "SAL GROSSO 50KGS",
            "codigo_ncm" => "55110011",
            "cfop" => "5102",
            "unidade_comercial" => "SC",
            "quantidade_comercial" => 10,
            "valor_unitario_comercial" => "22.45",
            "valor_bruto" => "224.50",
            "unidade_tributavel" => "SC",
            "quantidade_tributavel" => "10.00",
            "valor_unitario_tributavel" => "22.45",
            "origem" => "0",
            "inclui_no_total" => "1",
            "imposto" => [
                "valor_total_tributos" => 9.43,
                "icms" => [
                    "situacao_tributaria" => "101",
                    "modalidade_base_calculo" => "3",
                    "valor_base_calculo" => "0.00",
                    "modalidade_base_calculo_st" => "4",
                    "aliquota_reducao_base_calculo" => "0.00",
                    "aliquota" => "0.00",
                    "aliquota_final" => "0.00",
                    "valor" => "0.00",
                    "margem_valor_adicionado_st" => "0.00",
                    "reducao_base_calculo_st" => "0.00",
                    "base_calculo_st" => "0.00",
                    "aliquota_st" => "0.00",
                    "valor_st" => "0.00"
                ],
                "pis" => [
                    "situacao_tributaria" => "01",
                    "valor_base_calculo" => 224.5,
                    "aliquota" => "1.65",
                    "valor" => "3.70"
                ],
                "cofins" => [
                    "situacao_tributaria" => "01",
                    "valor_base_calculo" => 224.5,
                    "aliquota" => "7.60",
                    "valor" => "17.06"
                ]
            ],
            "valor_desconto" => 0,
            "valor_frete" => 0,
            "valor_seguro" => 0,
            "valor_outras_despesas" => 0,
            "informacoes_adicionais_item" => "Valor aproximado tributos R$: 9,43 (4,20%) Fonte: IBPT"
        ]
    ],
    "icms_base_calculo" => 0,
    "icms_valor_total" => 0,
    "valor_produtos" => 224.5,
    "valor_frete" => 0,
    "valor_seguro" => 0,
    "valor_desconto" => 0,
    "valor_pis" => 3.7,
    "valor_cofins" => 17.06,
    "valor_outras_despesas" => 0,
    "valor_total" => 224.5,
    "frete" => [
        "modalidade_frete" => "0",
        "volumes" => [
            [
                "quantidade" => "10",
                "especie" => null,
                "marca" => "TESTE",
                "numero" => null,
                "peso_liquido" => 500,
                "peso_bruto" => 500
            ]
        ]
    ],
    "cobranca" => [
        "fatura" => [
            "numero" => "1035.00",
            "valor_original" => "224.50",
            "valor_desconto" => "0.00",
            "valor_liquido" => "224.50"
        ]
    ],
    "pagamento" => [
        "formas_pagamento" => [
            [
                "meio_pagamento" => "01",
                "valor" => "224.50",
                "tipo_integracao" => "2"
            ]
        ]
    ],
    "informacoes_adicionais_contribuinte" => "PV: 3325 * Rep: DIRETO * Motorista:  * Forma Pagto: 04 DIAS * teste de observação para a nota fiscal * Valor aproximado tributos R$9,43 (4,20%) Fonte: IBPT",
    "pessoas_autorizadas" => [
        [
            "cnpj" => "96256273000170"
        ], [
            "cnpj" => "80681257000195"
        ]
    ]
];
```

```json
{
  "ambiente": "P",
  "natureza_operacao": "VENDA DENTRO DO ESTADO",
  "serie": "1",
  "numero": "1035",
  "data_emissao": "2020-10-15T03:00:00-03:00",
  "data_entrada_saida": "2020-10-15T03:00:00-03:00",
  "tipo_operacao": "1",
  "local_destino": "1",
  "finalidade_emissao": "1",
  "consumidor_final": "0",
  "presenca_comprador": "9",
  "notas_referenciadas": [],
  "destinatario": {
    "cnpj": "15493535500128",
    "nome": "EMPRESA MODELO",
    "indicador_inscricao_estadual": "1",
    "inscricao_estadual": "212055510",
    "endereco": {
      "logradouro": "AVENIDA TESTE",
      "numero": "444",
      "bairro": "CENTRO",
      "codigo_municipio": "2408003",
      "nome_municipio": "Mossoro",
      "uf": "RN",
      "cep": "59653120",
      "codigo_pais": "1058",
      "nome_pais": "BRASIL",
      "telefone": "8499995555"
    }
  },
  "itens": [
    {
      "numero_item": "1",
      "codigo_produto": "000297",
      "descricao": "SAL GROSSO 50KGS",
      "codigo_ncm": "55110011",
      "cfop": "5102",
      "unidade_comercial": "SC",
      "quantidade_comercial": 10,
      "valor_unitario_comercial": "22.45",
      "valor_bruto": "224.50",
      "unidade_tributavel": "SC",
      "quantidade_tributavel": "10.00",
      "valor_unitario_tributavel": "22.45",
      "origem": "0",
      "inclui_no_total": "1",
      "imposto": {
        "valor_total_tributos": 9.43,
        "icms": {
          "situacao_tributaria": "101",
          "modalidade_base_calculo": "3",
          "valor_base_calculo": "0.00",
          "modalidade_base_calculo_st": "4",
          "aliquota_reducao_base_calculo": "0.00",
          "aliquota": "0.00",
          "aliquota_final": "0.00",
          "valor": "0.00",
          "margem_valor_adicionado_st": "0.00",
          "reducao_base_calculo_st": "0.00",
          "base_calculo_st": "0.00",
          "aliquota_st": "0.00",
          "valor_st": "0.00"
        },
        "pis": {
          "situacao_tributaria": "01",
          "valor_base_calculo": 224.5,
          "aliquota": "1.65",
          "valor": "3.70"
        },
        "cofins": {
          "situacao_tributaria": "01",
          "valor_base_calculo": 224.5,
          "aliquota": "7.60",
          "valor": "17.06"
        }
      },
      "valor_desconto": 0,
      "valor_frete": 0,
      "valor_seguro": 0,
      "valor_outras_despesas": 0,
      "informacoes_adicionais_item": "Valor aproximado tributos R$: 9,43 (4,20%) Fonte: IBPT"
    }
  ],
  "icms_base_calculo": 0,
  "icms_valor_total": 0,
  "valor_produtos": 224.5,
  "valor_frete": 0,
  "valor_seguro": 0,
  "valor_desconto": 0,
  "valor_pis": 3.7,
  "valor_cofins": 17.06,
  "valor_outras_despesas": 0,
  "valor_total": 224.5,
  "frete": {
    "modalidade_frete": "0",
    "volumes": [
      {
        "quantidade": "10",
        "especie": null,
        "marca": "TESTE",
        "numero": null,
        "peso_liquido": 500,
        "peso_bruto": 500
      }
    ]
  },
  "cobranca": {
    "fatura": {
      "numero": "1035.00",
      "valor_original": "224.50",
      "valor_desconto": "0.00",
      "valor_liquido": "224.50"
    }
  },
  "pagamento": {
    "formas_pagamento": [
      {
        "meio_pagamento": "01",
        "valor": "224.50",
        "tipo_integracao": "2"
      }
    ]
  },
  "informacoes_adicionais_contribuinte": "PV: 3325 * Rep: DIRETO * Motorista:  * Forma Pagto: 04 DIAS * teste de observação para a nota fiscal * Valor aproximado tributos R$9,43 (4,20%) Fonte: IBPT",
  "pessoas_autorizadas": [
    {
      "cnpj": "96256273000170"
    }, {
      "cnpj": "80681257000195"
    }
  ]
}
```


## Retorno

```json
{
  "sucesso": true,
  "codigo": 100,
  "mensagem": "Autorizado o uso do NF-e",
  "chave": "50190813188739000110550010000012001581978549",
  "xml": "PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZ...VByb2M+",
  "pdf": "JVBERi0xLjMKMyAwIG9iago8PC9UeXBlIC9QYA+...GCg==",
  "data_hora_evento": "2019-08-30T11:25:22-04:00",
  "protocolo": "150190003925457",
  "status": "autorizado",
  "numero": "1200",
  "serie": "1"
}
```

|Parametro|Tipo|Descrição|Observação|
|:---|:---:|:---|:---|
|sucesso|boolean|Indica a condição da resposta|pode ser true ou false se algum problema ocorreu|
|codigo|string|Código de retorno|podem ser os codigos retornados pela SEFAZ ou algum codigo interno da API|
|mensagem|string|Descrição da resposta|podem ser descritivos retornados pela SEFAZ ou alguma mensagem de erro interno da API|
|chave|string|Chave do documento fiscal|44 digitos numericos|
|xml|string|XML protocolado do documento fiscal||
|pdf|string|PDF representação grafica do documento fiscal||
|data_hora_evento|string|data e hora do evento de autorização||
|protocolo|string|protocolo de autorizaçao||
|status|string|situação do documento|offline ou autorizado ou denegado ou cancelado ou indefinido|
|numero|string|numero do docuemento fiscal||
|serie|string|seire do documento fiscal||
