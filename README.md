# wp-performance
Boas práticas para Performance no WordPress

O objetivo maior é fornecer uma experiência de frontend muito rápida e para isso é preciso engenharia avançada e não apenas "plugins".

A performance deve ser uma preocupação constante ao longo do desenvolvimento e não deve ser tratada como uma tarefa única. Este guia tem como objetivo reunir boas práticas de engenharia e ferramentas e ser um bom ponto de partida para melhorar a performance de um site WordPress (i.e. um site assente em WordPress.org com um tema feito de raiz ou pelo menos onde tem controle sobre o código).

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

Em termos de performance tudo passará por escolhas. Que alojamento? Que plugins? que versão do PHP? que versão da base de dados, etc etc. e que combinação de todas estas opções funciona melhor em termos de performance para o caso em questão e budget disponível. Isto assumindo que não existem questões de programação mal feitas pois código mal feito ou lento será problemático em qualquer infra-estrutura/configuração.

## Ordem de Prioridades

Para melhorar a performance no WordPress, recomenda-se a seguinte ordem de prioridades:

1. **Bom Alojamento**
2. **Boa Programação e Boas Práticas**
3. **Otimização de Imagens**
4. **Configuração de Caching**
5. **Otimização da Base de Dados**
6. **Medidas de Segurança**

Cada uma destas "camadas" constrói-se em cima da anterior. Se uma não tiver bem provavelmente fazer as outras pode não trazer melhorias significativas, e.g. se o alojamento ou a programação não estiverem bem tudo o resto será apenas "pôr batom no porco".

## Bom Alojamento

Um bom alojamento é fundamental para a performance do PHP/WordPress. Isso já sabemos. O que devemos ter em atenção?

- Excelente Time to First Byte (TTFB), [ver um leaderboard que compara diferentes opções](https://ismyhostfastyet.com/).
- Infraestrutura técnica avançada (preferência por [containers ao invés de VMs](https://pantheon.io/blog/why-we-built-pantheon-containers-instead-virtual-machines))
- Suporte para as versões de PHP mais recentes, ver [PHP Benchmarks: Real-World Speed Tests for Versions 8.1, 8.2, and 8.3](https://kinsta.com/blog/php-benchmarks/) 

Se não for viável mudar para um alojamento especializado em WordPress tentar garantir pelo menos que a versão do PHP é a mais recente e que os módulos de otimização no servidor estão ligados (dependente do alojamento estas opções serão diferentes mas o suporte poderá ajudar a verificar).

## Boas Práticas de Programação

Adotar boas práticas de programação e engenharia é essencial para garantir a performance e escalabilidade do site. Seguem algumas recomendações baseadas no [guia da 10up](https://10up.github.io/Engineering-Best-Practices/php/):

- **Lazy Loading:** Carregar os recursos (images, scripts, styles, ..) apenas quando necessário.
- **Async e Defer:** Utilizar atributos `async` e `defer` em scripts para melhorar o carregamento.
- **Delegar Tarefas Pesadas:** Mover tarefas pesadas para serviços externos ou micro-serviços, tirar o peso de cima do PHP/WordPress, e.g. scripts que precisem efetuar tarefas em bulk ou ter tempos de espera grandes. 
- **Pesquisa**: na pesquisa é uma boa prática delegar para um serviço externo e.g. Elasticsearch ou algo similar. A Cloudways já disponibiliza Elasticsearch de base, ver [este artigo](https://support.cloudways.com/en/articles/5120760-how-to-configure-elasticsearch-on-cloudways).
- **Carregamento Condicional de Scripts e Estilos:** Carregar scripts e estilos apenas onde são necessários, evitando carregamento desnecessário em todas as páginas.
- **Minimizar Queries ao Banco de Dados:** Evitar usar `posts_per_page => -1` e otimizar queries para melhorar a performance.

## Otimização de Imagens e Offloading

A otimização de imagens é crucial para reduzir o tempo de carregamento da página. Algumas práticas incluem:

- Utilizar formatos de imagem modernos como WebP.
- Redimensionar imagens para as dimensões exatas necessárias antes de fazer o upload.
- Compressão automática de imagens.

O offloading consiste em carregar as imagens através de um serviço externo. A Cloudflare permite converter as imagens em WebP e fazer o offload de media, ver e.g. [este artigo](https://themedev.net/blog/how-to-offload-wp-media-to-cloudflare-r2/).

Ferramenta recomendada: 
https://squoosh.app/

## Configuração de Caching

Configurar corretamente caching pode melhorar significativamente a performance. As boas práticas neste caso incluem:

- Utilizar um plugin de cache confiável (há muitas opções e será uma questão de preferência pessoal no final do dia).
- Configurar a cache do browser para armazenar recursos estáticos (é normalmente uma opção do plugin de cache).
- Implementar object cache com Redis ou Memcached (alguns alojamentos suportam já de base mas podem não estar ativo).

## Otimização da Base de Dados

A base de dados deve ser otimizada regularmente para manter a performance. Algumas dicas incluem:

- Minimizar o número de linhas na tabela `post_meta`.
- Manter debaixo de olho o número de linhas na tabela wp_options com "autoload=true", ver [este artigo](https://docs.pantheon.io/optimize-wp-options-table-autoloaded-data)
- Utilizar índices nas tabelas da base de dados.
- Remover dados obsoletos e revisões de posts antigas/não utilizadas.
- Limitar o número de revisões a serem guardadas na base de dados, ver [este artigo](https://wordpress.org/documentation/article/revisions/).

## Medidas de Segurança

A maior parte das pessoas não se apercebe que as medidas de segurança não só protegem o site como também podem melhorar a performance (e.g. aliviando o servidor). Algumas boas práticas incluem:

- Utilização de Cloudflare ou outra Web firewall (WAF) para bloquear e filtrar tráfego malicioso antes de chegar ao servidor.
- Se não for possível utilizar uma WAF pode utilizar plugins de segurança como Wordfence ou Sucuri contudo a performance poderá ser afectada pois o processamento destes plugins ocorre dentro do PHP/WordPress.

## Ferramentas Úteis

- **Plugin Query Monitor:** Permite fazer debugging às queries que são feitas à base de dados.
- **GTmetrix e Google PageSpeed Insights:** Para análise de performance e sugestões de melhorias.
- **New Relic:** Para monitoring de performance, nomeadamente tempos de resposta, logs, etc. 

## Conclusão

A performance de um site WordPress é um esforço contínuo que envolve boas práticas de programação, configuração adequada de caching, otimização de imagens e medidas de segurança. 

O objetivo maior é ter uma experiência de frontend muito rápida e para isso precisamos de engenharia avançada e não apenas "plugins".









