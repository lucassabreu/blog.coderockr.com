---
title: "Um Ambiente Simples Usando Kubernetes e OpenShift Next Gen — Parte 4"
description: "Este post é a quarta parte de uma série sobre o básico necessário para usar o Kubernetes..."
author: "Lucas Abreu"
date: 2017-03-10
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "Kubernetes", "Ferramentas"]
aliases: [
    "/um-ambiente-simples-usando-kubernetes-e-openshift-next-gen---parte-4-d012d6eb5344",
    "/um-ambiente-simples-usando-kubernetes-e-openshift-next-gen-parte-4-d012d6eb5344",
]
---

Este post é a quarta parte de uma série sobre o básico necessário para usar o Kubernetes, caso você não tenha lido o post anterior recomendo lê-lo e depois voltar aqui para não ficar perdido.

* Parte 1 — Conceitos Básicos: [clique aqui](https://blog.coderockr.com/um-ambiente-simples-usando-kubernetes-e-openshift-next-gen-parte-1-d012d6eb5344)

* Parte 2 — Construindo o Ambiente: [clique aqui](https://blog.coderockr.com/um-ambiente-simples-usando-kubernetes-e-openshift-next-gen-parte-2-30aea15a74fb)

* Parte 3 — Volumes Persistentes: [clique aqui](https://blog.coderockr.com/um-ambiente-simples-usando-kubernetes-e-openshift-next-gen-parte-3-a36e01e920cb)

Como citei no [post anterior](https://blog.coderockr.com/um-ambiente-simples-usando-kubernetes-e-openshift-next-gen-parte-3-a36e01e920cb) ainda existe um ponto de desconforto no ambiente, que é o fato das senhas e usuários estarem expostos diretamente nas configurações. O Kubernetes oferece uma solução para esse problema os [**Secrets](https://kubernetes.io/docs/user-guide/secrets/)**.

E agora irei mostrar como adicioná-los ao projeto.

Caso não tenha mais os fontes até o estado do post anterior, ou prefira acompanhar o meu andamento, pode pode pegá-los aqui: [https://github.com/lucassabreu/openshift-next-gen/tree/v2](https://github.com/lucassabreu/openshift-next-gen/tree/v2); ou executar:

    git clone -b v2 \
        https://github.com/lucassabreu/openshift-next-gen.git

### Secrets

Existem algumas formas de criar e usar os mesmos, criá-los diretamente de arquivos, ou usando configurações, e expô-los aos contêineres usando volumes ou variáveis de ambiente.

Para essa aplicação vou utilizar um YAML para definir um Secret e vou modificar os Pods para alimentarem as variáveis de ambiente com eles. A estrutura básica do Secret é como segue:

<iframe src="https://medium.com/media/a65d659f5cd184914db4294f0153184e" frameborder=0></iframe>

Nele estou criando o Secret mysql-secrets e definindo quatro chaves que representam as três variáveis do MySQL e uma do servidor HTTP. No lugar do <hash base64> deve ir o conteúdo do segredo em Base 64, que pode ser gerado usando o comando echo -n "meusegredo" | base64 -w0.

Eu não gostei muito da ideia de guardar o Base 64 dentro da definição do Secret, então fiz a seguinte modificação no meu mysql-secrets.yml:

<iframe src="https://medium.com/media/4369ac891dd94f889ed672ac343d7fb3" frameborder=0></iframe>

E quando vou aplicar o Secret no Kubernetes uso este script:

<iframe src="https://medium.com/media/882fb10a89a8f8f2aa3daf3b349cf643" frameborder=0></iframe>

Esse script cria uma senha aleatória para o root e usa duas variáveis de ambiente para definir o usuário e senha do MySQL, faz o Base 64 deles, injeta eles no arquivo via sed no Secret e aplica no Kubernetes com oc apply -f - que irá ler a saída do sed e aplicá-la. Na hora de executar fica assim:

    $ export DATABASE_USER=appoint
    $ export DATABASE_PASSWORD=123
    $ ./env-set-oc.sh
    secret "mysql-secrets" configured

Altero os Deployments para considerarem o Secret que criei:

<iframe src="https://medium.com/media/fb7981553e796636b0de8682c4b0e7c8" frameborder=0></iframe>

A alteração consiste de trocar a chave value das variáveis por valueFrom e apontar para as chaves corretas dentro do Secret.

Depois que aplica as mudanças os Deployments vão identificá-las e trocar os Pods por novos. E passaram a utilizar os Secrets informado nas variáveis para eles.

Ao final dessa séria, a conclusão que posso chegar é que o Kubernetes exige um conjunto razoavelmente grande de configurações para podermos servir uma aplicação, mas são arquivos simples de se entender e muito bem [documentados](https://kubernetes.io/docs/reference/) o que facilitou bastante o processo, e não me fez sentir o peso dessa quantidade.

Gostou do post e achou útil? Dê um **like **❤ abaixo para ajudar na divulgação e para que mais pessoas tenham acesso **😄**, e obrigado por me acompanhar até aqui ️!
