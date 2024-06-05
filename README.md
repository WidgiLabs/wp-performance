# wp-performance
Boas práticas para Performance no WordPress

A performance é uma preocupação constante e não deve ser tratada como uma tarefa única. Este guia tem como objetivo reunir boas práticas e ferramentas para melhorar a performance de um site WordPress (i.e. um site assente em WordPress.org com um tema feito de raiz ou pelo menos onde tem controle sobre o código).

## Índice
1. [Introdução](#introdução)
2. [Ordem de Prioridades](#ordem-de-prioridades)
3. [Bom Alojamento](#bom-alojamento)
4. [Boas Práticas de Programação](#boas-práticas-de-programação)
5. [Otimização de Imagens](#otimização-de-imagens)
6. [Configuração de Caching](#configuração-de-caching)
7. [Otimização da Base de Dados](#otimização-da-base-de-dados)
8. [Medidas de Segurança](#medidas-de-segurança)
9. [Ferramentas Úteis](#ferramentas-úteis)
10. [Conclusão](#conclusão)

## Introdução

A performance de um site WordPress envolve várias etapas e boas práticas. Não se trata apenas de ativar plugins de cache e otimizar imagens. É necessário adotar uma abordagem holística e contínua, integrando boas práticas de engenharia e utilizando ferramentas adequadas.

## Ordem de Prioridades

Para melhorar a performance no WordPress, recomenda-se a seguinte ordem de prioridades:

1. **Bom Alojamento**
2. **Boa Programação e Boas Práticas**
3. **Otimização de Imagens**
4. **Configuração de Caching**
5. **Otimização da Base de Dados**
6. **Medidas de Segurança**

## Bom Alojamento

Um bom alojamento é fundamental para a performance do WordPress. Isso já sabemos. O que devemos ter em atenção?

- Excelente Time to First Byte (TTFB)
- Infraestrutura técnica avançada (preferência por [containers ao invés de VMs](https://pantheon.io/blog/why-we-built-pantheon-containers-instead-virtual-machines))
- Suporte para as versões de PHP mais recentes 
- Certificados SSL gratuitos

## Boas Práticas de Programação

Adotar boas práticas de programação é essencial para garantir a performance e escalabilidade do site. Seguem algumas recomendações baseadas no [guia da 10up](https://10up.github.io/Engineering-Best-Practices/php/):

- **Lazy Loading:** Carregar recursos apenas quando necessário.
- **Async e Defer:** Utilizar atributos `async` e `defer` em scripts para melhorar o carregamento.
- **Delegação de Tarefas:** Mover tarefas pesadas para serviços externos, como utilizar Elasticsearch para busca.
- **Carregamento Condicional de Scripts e Estilos:** Carregar scripts e estilos apenas onde são necessários, evitando carregamento desnecessário em todas as páginas.
- **Minimizar Queries ao Banco de Dados:** Evitar usar `posts_per_page => -1` e otimizar queries para melhorar a performance.

## Otimização de Imagens

A otimização de imagens é crucial para reduzir o tempo de carregamento da página. Algumas práticas incluem:

- Utilizar formatos de imagem modernos como WebP.
- Redimensionar imagens para as dimensões exatas necessárias antes de fazer o upload.
- Usar plugins como WP Smush ou EWWW Image Optimizer para compressão automática de imagens.

## Configuração de Caching

Configurar corretamente caching pode melhorar significativamente a performance do seu site. As práticas incluem:

- Utilizar um plugin de cache confiável.
- Configurar a cache do browser para armazenar recursos estáticos.
- Implementar object cache com Redis ou Memcached.

## Otimização da Base de Dados

A base de dados deve ser otimizada regularmente para manter a performance. Algumas dicas incluem:

- Minimizar o número de linhas na tabela `post_meta`.
- Manter debaixo de olho o número de linhas na tabela wp_options em que o autoload esteja a true, ver [este artigo](https://docs.pantheon.io/optimize-wp-options-table-autoloaded-data)
- Utilizar índices adequados nas tabelas do banco de dados.
- Remover dados obsoletos e revisões de posts não utilizados.

## Medidas de Segurança

A maior parte das pessoas não se apercebe mas medidas de segurança não só protegem o site como também podem melhorar a performance (e.g. aliviando o servidor). Algumas boas práticas incluem:

- Utilização de Cloudflare.
- Implementar uma firewall web (WAF) para bloquear e filtrar tráfego malicioso antes de chegar ao servidor.
- Utilizar plugins de segurança como Wordfence ou Sucuri.

## Ferramentas Úteis

- **Query Monitor:** Permite fazer debugging às queries que são feitas à base de dados.
- **GTmetrix e Google PageSpeed Insights:** Para análise de performance e sugestões de melhorias.
- **New Relic:** Para monitoring de performance, nomeadamente tempos de resposta, logs, etc. 

## Conclusão

A performance de um site WordPress é um esforço contínuo que envolve boas práticas de programação, configuração adequada de caching, otimização de imagens e medidas de segurança. 

O objetivo maior é ter uma experiência de frontend muito rápida e para isso precisamos de engenharia avançada e não apenas "plugins".









