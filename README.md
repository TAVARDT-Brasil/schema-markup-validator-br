# Validador de Schema Markup e Dados Estruturados

[![Validador Schema](https://img.shields.io/badge/TAVARDT-Schema.org%20Validator-5b3cf5.svg)](https://tools.tavardt.com/br/validador-schema-markup/)
[![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

Mecanismo de validação rigorosa de dados estruturados para Schema.org em formatos JSON-LD e Microdata, verificando relações de entidades, completude de atributos e elegibilidade a rich snippets.

A ferramenta extrai estruturas semânticas de páginas web em produção para detectar erros de sintaxe JSON, esquemas incompletos e omissão de propriedades obrigatórias exigidas por rastreadores e motores de resposta de IA. Desenvolvido sem dependências externas, opera nativamente em console Python e pipelines de CI/CD.

***

## Capacidades de Validação Semântica e Entidades Suportadas

Motores de busca e sistemas de recuperação neural dependem de dados estruturados explícitos para desambiguar organizações, verificar autoria e exibir resultados enriquecidos. Erros sintáticos ou ausência de propriedades essenciais levam à desqualificação de snippets e menor visibilidade orgânica.

* **Extração Abrangente de Dados Estruturados**: Varredura automatizada de blocos JSON-LD (`application/ld+json`) e detecção de marcações legadas em Microdata (`itemscope`, `itemtype`).
* **Suporte a Grafos Semânticos**: Processamento nativo de estruturas complexas com `@graph`, resolvendo nós interconectados de entidades em aplicações modernas.
* **Classificação de Entidades Schema.org**: Validação de propriedades requeridas nos principais modelos: Organization, LocalBusiness, WebSite, Article, NewsArticle, BlogPosting, Product, BreadcrumbList e FAQPage.
* **Auditoria de Sintaxe e Integridade**: Aplicação de penalidades dedutivas para JSON malformado (-20 pontos), ausência total de JSON-LD (-40 pontos), falta de entidades identificáveis (-30 pontos) e ausência de atributos recomendados (-5 pontos cada).
* **Pontuação Quantitativa de Qualidade Semântica**: Índice de 0 a 100 pontos categorizando a conformidade técnica em OPTIMAL, WARNING ou CRITICAL com orientações corretivas prioritárias.

***

## Matriz de Inspeção de Entidades Semânticas

<table>
  <thead>
    <tr>
      <th align="left">Classe da Entidade</th>
      <th align="left">Propriedades Obrigatórias</th>
      <th align="left">Regra de Dedução</th>
      <th align="left">Impacto em Rich Results e Recuperação</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>Organization</strong></td>
      <td><code>name</code>, <code>url</code></td>
      <td>-5 Pontos por propriedade ausente</td>
      <td>Identifica a entidade corporativa e estabelece autoridade canônica no grafo de conhecimento.</td>
    </tr>
    <tr>
      <td><strong>LocalBusiness</strong></td>
      <td><code>name</code>, <code>address</code></td>
      <td>-5 Pontos por propriedade ausente</td>
      <td>Essencial para indexação local, mapas e relevância geográfica em buscas comerciais.</td>
    </tr>
    <tr>
      <td><strong>WebSite</strong></td>
      <td><code>name</code>, <code>url</code></td>
      <td>-5 Pontos por propriedade ausente</td>
      <td>Habilita a caixa de pesquisa de sitelinks e define o escopo do domínio corporativo.</td>
    </tr>
    <tr>
      <td><strong>Article / NewsArticle</strong></td>
      <td><code>headline</code>, <code>author</code></td>
      <td>-5 Pontos por propriedade ausente</td>
      <td>Qualifica para o carrossel de Principais Notícias e fundamenta respostas em LLMs.</td>
    </tr>
    <tr>
      <td><strong>BlogPosting</strong></td>
      <td><code>headline</code>, <code>author</code></td>
      <td>-5 Pontos por propriedade ausente</td>
      <td>Atribui autoria e cronologia editorial para publicações e artigos de conteúdo.</td>
    </tr>
    <tr>
      <td><strong>Product</strong></td>
      <td><code>name</code>, <code>offers</code></td>
      <td>-5 Pontos por propriedade ausente</td>
      <td>Ativa rich snippets de preços, disponibilidade de estoque e avaliações de e-commerce.</td>
    </tr>
    <tr>
      <td><strong>BreadcrumbList</strong></td>
      <td><code>itemListElement</code></td>
      <td>-5 Pontos por propriedade ausente</td>
      <td>Exibe a trilha de navegação hierárquica nos resultados de busca no lugar da URL bruta.</td>
    </tr>
    <tr>
      <td><strong>FAQPage</strong></td>
      <td><code>mainEntity</code></td>
      <td>-5 Pontos por propriedade ausente</td>
      <td>Gera menus expansíveis de perguntas e respostas nos resultados orgânicos.</td>
    </tr>
  </tbody>
</table>

***

## Instalação e Guia Rápido de Execução

Requer Python 3.8 ou superior utilizando exclusivamente bibliotecas padrão (`sys`, `re`, `json`, `ssl`, `argparse`, `urllib.request`, `urllib.error`). Nenhuma dependência externa ou instalação via pip é necessária.

Clone o repositório e execute a auditoria contra um domínio alvo:

```bash
git clone https://github.com/TAVARDT-Brasil/schema-markup-validator-br.git
cd schema-markup-validator-br
python schema_validator.py exemplo.com.br
```

Para gerar a saída estruturada em formato JSON para automações:

```bash
python schema_validator.py exemplo.com.br -j
```

Inspecione domínios definindo um tempo limite customizado:

```bash
python schema_validator.py https://exemplo.com.br -j -t 12
```

Exemplo de payload JSON retornado:

```json
{
  "target": "https://exemplo.com.br",
  "score": 95,
  "status": "OPTIMAL",
  "json_ld_blocks": 1,
  "entities": [
    "Organization",
    "WebSite"
  ],
  "microdata_detected": false,
  "issues": [
    "Entity 'Organization' is missing recommended property: url"
  ],
  "recommendations": [
    "Add missing property 'url' to 'Organization' entity."
  ]
}
```

Escala de classificação do status:

* **Pontuação 85 a 100 (OPTIMAL)**: Marcação estruturada de alta qualidade com cobertura completa de entidades e ausência de erros sintáticos.
* **Pontuação 65 a 84 (WARNING)**: Omissões pontuais de propriedades ou cobertura incompleta que podem limitar a conquista de rich results.
* **Pontuação 0 a 64 (CRITICAL)**: Sintaxe JSON corrompida, ausência de dados estruturados ou blocos ilegíveis exigindo correção imediata.

***

## Perguntas Frequentes (FAQ Técnico)

### Por que o formato JSON-LD é preferido em relação ao Microdata para rich snippets e indexação por IA?
O JSON-LD isola os dados estruturados em blocos independentes, permitindo manutenção centralizada sem interferir no HTML visível aos usuários. Além disso, o Google e os rastreadores de IA recomendam JSON-LD porque ele pode ser injetado dinamicamente e processado sem necessidade de renderização de layout.

### Como o validador processa matrizes `@graph` em implementações complexas de JSON-LD?
Ao identificar a chave `@graph`, o validador percorre recursivamente o array de subobjetos, extraindo cada entidade declarada e submetendo seus atributos às regras de validação. Isso garante a análise completa de nós semânticos interligados.

### Quais penalidades algorítmicas são aplicadas diante de falhas de marcação estruturada?
A ferramenta deduz 40 pontos caso nenhum bloco JSON-LD seja identificado, 20 pontos por bloco com sintaxe JSON corrompida e 30 pontos se nenhuma entidade reconhecível existir. Além disso, cada propriedade recomendada ausente reduz 5 pontos da pontuação base de 100.

### Como o script audita os dados semânticos sem carregar um navegador headless completo?
O utilitário emite requisições HTTP GET pontuais com cabeçalho de User-Agent próprio, obtendo o código HTML bruto da página. Expressões regulares ágeis extraem o conteúdo das tags de script, que são processadas pelo interpretador nativo de JSON em frações de segundo.

### Como esteiras de CI/CD podem automatizar a validação de Schema Markup com o formato JSON?
Engenheiros podem invocar o script `schema_validator.py` com o parâmetro `-j` em esteiras automatizadas e inspecionar a saída via jq. É possível definir regras para bloquear deploys caso a pontuação caia abaixo de 85 ou caso surjam erros no array `issues`.

***

## Consultoria em Arquitetura Semântica e Grafos de Conhecimento

Para modelagem de dados estruturados, engenharia de grafos de conhecimento e conformidade com padrões da W3C no Brasil, consulte os especialistas da **TAVARDT Brasil**.

***

## Links Úteis

* **Portal Corporativo:** https://ag.tavardt.com/br/
* **Suíte de Diagnósticos:** https://tools.tavardt.com/br/
* **Contato Técnico:** https://ag.tavardt.com/br/contato/

***

### Licença
Distribuído sob a Licença MIT.

***

© 2026 TAVARDT. Todos os direitos reservados.
