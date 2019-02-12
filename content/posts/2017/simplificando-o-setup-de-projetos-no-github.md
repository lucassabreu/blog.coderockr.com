---
title: "Simplificando o Setup de Projetos no GitHub"
description: "Na Coderockr iniciamos e assumimos vários projetos, sejam para clientes que nos contratam ou para ações internas, e normalmente o GitHub acaba sendo a ferramenta escolhida para eles..."
author: "Lucas Abreu"
date: 2017-01-10
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "GitHub", "Ferramentas"]
---

Na Coderockr iniciamos e assumimos vários projetos, sejam para clientes que nos contratam ou para ações internas, e normalmente o GitHub acaba sendo a ferramenta escolhida para eles.

E ao longo dos anos acabamos definindo uma estrutura para controlarmos as nossas *issues*, usando as seguintes *labels:*

![Conjunto de labels utilizadas no [Coderockr Way](https://blog.coderockr.com)](https://cdn-images-1.medium.com/max/2000/1*5BH5MxYLTaFzEQVWPJtezQ.png)*Conjunto de labels utilizadas no [Coderockr Way](https://blog.coderockr.com)*

É um conjunto bem simples, mas que deixa bem claro as etapas, prioridades, tipos e estados das tarefas, de forma que fica bem fácil de acompanhá-las.

Em todos os projetos cadastramos essas *labels*. Mas mesmo que você esteja de bom humor, vai ser um trabalho chato e moroso. E é bem provável que esqueça alguma delas, e precise revisar a lista para garantir que estão todas lá.

![Esqueci o “Stage: Testing”? Achei…](https://cdn-images-1.medium.com/max/2000/1*JXvJ5FXr6RM63cOix1eEdA.gif)*Esqueci o “Stage: Testing”? Achei…*

Pensando nessa monotonia e para economizar algum tempo de setup nos projetos resolvemos criar um script que fizesse esse processo, adicionando as labels a um projeto que fosse informado.

O script acabou ficando bem mais simples que o esperado graças a simplicidade da API do GitHub, todo ele foi feito com cURL e alguns loops no bash para as labels.

Até preparamos ele para não precisar ser baixado/instalado, basta dar um cURL direto do repositório do GitHub que ele pede os dados que precisa.

![nem precisa baixar](https://cdn-images-1.medium.com/max/2000/1*1IUgqZ45GVjfc4VYKw_tAA.png)*nem precisa baixar*

Resolvemos deixar o script que criamos em um repositório público no GitHub para quem estiver procurando uma solução parecida (ou que agora acha que vale a pena criar um também).

O script e como usá-lo estão [aqui](https://github.com/Coderockr/coderockr-way-github-setup).

Conclusão, investir um pouco de tempo para entender as ferramentas que você usa, não só te poupa tempo a longo prazo, quanto também gera uns scripts legais de compartilhar


Criamos um novo script para fazer o mesmo setup para projetos no GitLab, pode visualizar o script [aqui](https://github.com/Coderockr/coderockr-way-github-setup/blob/master/coderockr-way-gitlab-setup.bash).
