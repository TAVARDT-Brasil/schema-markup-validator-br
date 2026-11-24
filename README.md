# Validador de Schema Markup e Dados Estruturados | TAVARDT Brasil

[![Validador Schema](https://img.shields.io/badge/TAVARDT-Schema.org%20Validator-5b3cf5.svg)](https://tools.tavardt.com/br/validador-schema-markup/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)

Auditoria tecnica de marcacao semantica Schema.org em formato JSON-LD e Microdata, validacao de tipos, entidades e elegibilidade para Rich Snippets.

Desenvolvido e mantido pela divisao de engenharia semantica e indexacao tecnica da **TAVARDT Brasil**.

***

## Capacidades de Validacao Semantica

* **Extracao Abrangente de Dados Estruturados**: Inspecao automatica de blocos JSON-LD (`application/ld+json`) e marcacao semantica em Microdata.
* **Analise de Entidades Principais**: Reconhecimento de tipos centrais como Organization, LocalBusiness, WebSite, Article, Product, FAQPage e BreadcrumbList.
* **Checagem de Sintaxe e Integridade**: Deteccao de erros de parsing JSON, URLs invalidas e atributos obrigatorios ausentes para exibicao em Rich Snippets.
* **Pontuacao de Qualidade Semantica**: Metrica tecnica de 0 a 100 com recomendacoes estruturadas de aprimoramento semantico.

***

## Instalacao e Execucao

Requer Python 3.8 ou superior sem dependencias externas.

```bash
git clone https://github.com/TAVARDT-Brasil/schema-markup-validator-br.git
cd schema-markup-validator-br
python schema_validator.py exemplo.com.br
```

Para gerar a saida em formato JSON:

```bash
python schema_validator.py exemplo.com.br -json
```

***

## Diagnostico Completo na Web

Para validacao visual e teste em lote de estruturas de dados:

* **Validador de Schema Markup Oficial**
* **[Portal Corporativo TAVARDT Brasil](https://ag.tavardt.com/br/)**

***

## Consultoria em Arquitetura Semantica e SEO

Para projetos de estruturacao de dados complexos e conformidade com Diretrizes de Dados Estruturados e Boas Praticas SEO, contate os engenheiros da **TAVARDT Brasil**.
