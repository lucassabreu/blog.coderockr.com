---
title: "Um Ambiente Simples Usando Kubernetes e OpenShift Next Gen — Parte 3"
description: "Este post é a terceira parte de uma série sobre o básico necessário para usar o Kubernetes..."
author: "Lucas Abreu"
date: 2017-03-09
draft: false
categories: ["Desenvolvimento"]
tags: ["Desenvolvimento", "Kubernetes", "Ferramentas"]
aliases: [
    "/um-ambiente-simples-usando-kubernetes-e-openshift-next-gen---parte-3-d012d6eb5344",
    "/um-ambiente-simples-usando-kubernetes-e-openshift-next-gen-parte-3-d012d6eb5344",
]
---

Este post é a terceira parte de uma série sobre o básico necessário para usar o Kubernetes, caso você não tenha lido o post anterior recomendo lê-lo e depois voltar aqui para não ficar perdido.

* Parte 1 — Conceitos Básicos: [clique aqui](https://blog.coderockr.com/um-ambiente-simples-usando-kubernetes-e-openshift-next-gen-parte-1-d012d6eb5344)

* Parte 2 — Construindo o Ambiente: [clique aqui](https://blog.coderockr.com/um-ambiente-simples-usando-kubernetes-e-openshift-next-gen-parte-2-30aea15a74fb)

* Parte 4 — Segredos: [clique aqui](https://medium.com/@lucassabreu/um-ambiente-simples-usando-kubernetes-e-openshift-next-gen-parte-4-665505dbba59#.akd139siq)

Como comentei no [post anterior](https://blog.coderockr.com/um-ambiente-simples-usando-kubernetes-e-openshift-next-gen-parte-2-30aea15a74fb) existem alguns problemas no ambiente que construí, e o princípial deles é que os Pods não totalmente efêmeros, ou seja, se eu adicionar novos dados nele, no momento que o Pod fosse destruído os dados iriam junto e sem backup !

E agora iremos tratar esse primeiro problema. Caso não tenha mais os fontes até o estado do post anterior, ou prefira acompanhar o meu andamento, pode pode pegá-los aqui: [https://github.com/lucassabreu/openshift-next-gen/tree/v1](https://github.com/lucassabreu/openshift-next-gen/tree/v1); ou executar:

    git clone -b v1 \
        https://github.com/lucassabreu/openshift-next-gen.git

### Volumes Persistentes

Podemos testar esse problema conectando no Pod e adicionando alguns dados e então destruindo ele para ver o efeito. Vou adicionar um registro sobre para Homens no Sábado, pois é um dia sem nenhuma informação e facilita a visualização.

![Antes… sem dados](https://cdn-images-1.medium.com/max/2000/1*FYVpOeVGfqY7G-gZsNt0ZA.png)*Antes… sem dados*

Para acessar o Pod usa-se o comando oc rsh <pod-name>, e para encontrar o nome do Pod posso usar o comando oc get pods -l <selector>, então é só acessar o MySQL e inserir os dados:

<iframe src="https://medium.com/media/55c02f282924e6ab8e58c3e9bcf19ecb" frameborder=0></iframe>

Entrando novamente na aplicação e indo em “Sunday”, tenho um gráfico com dados para os Homens.

![isso se o seu contêiner não morrer no caminho](https://cdn-images-1.medium.com/max/2000/1*eHFCHyzMZmAfpDSHYRMEmw.png)*isso se o seu contêiner não morrer no caminho*

Para concluir o teste, basta apagar o Pod com oc delete pods -l name=db-pod ou oc delete pod db-deployment-xyz, esperar o Pod ser recriado e então ver que as alterações nos dados se foram:

![:’(](https://cdn-images-1.medium.com/max/2000/1*FYVpOeVGfqY7G-gZsNt0ZA.png)*:’(*

Para resolver esse problema o Kubernetes possui os [**Persistent Volume Claims (PVC)](https://kubernetes.io/docs/user-guide/persistent-volumes/) **que permitem definir volumes que existem fora do ciclo de vida de um Pod, ou seja, mesmo que todos os Pods sejam destruídos, o PVC irá manter os dados em si.

Podemos utilizar vários tipos de volumes em um PVC para armazenar os dados, no caso do OpenShift o padrão é [EBS](https://kubernetes.io/docs/user-guide/persistent-volumes/#aws), que são volumes armazenados dentro do [AWS da Amazon](https://aws.amazon.com/), mas existe a opção de usar volumes do Google Cloud, do Azure, Locais, etc; no Kubernetes.

Mas no momento o OpenShift esta ofertando apenas o EBS. Abaixo esta a definição do PVC:

<iframe src="https://medium.com/media/ca2a8bce9c526199cb07061e5c2dda0f" frameborder=0></iframe>

Depois de um momento o OpenShift irá criar um volume e disponibilizá-lo, agora é preciso vincular ele com os db-pods, para isso basta alterar os volumes no db-deployment:

<iframe src="https://medium.com/media/27079efa8eee93cd39840231724b2ebf" frameborder=0></iframe>

Duas coisas foram alteradas no db-deployment:

* O nome do volume mudou, isso é necessário porque estamos fazendo uma mudança de tipo de volume, e o Deployment não consegue alterar o tipo, mas se temos um novo, então tudo bem.

* Adicionei a tag persistentVolumeClaim no volume novo e apontei para o PVC que criei agora a pouco.

Executo o comando oc apply -f db-deployment.yml e o Deployment irá destruir os Pods antigos e criar novos usando o PVC.

Agora se replicarmos os comandos de para incluir registros e destruir o Pod do MySQL, quando o Deployment recriar o Pod ele manterá os dados.

Outro ponto que esta desconfortável no meu ambiente é o fato das senhas e usuários estarem expostas diretamente nas configurações. O Kubernetes oferece uma solução para esse problema, que irei abordar no próximo post.

Próximo Post: <Em Breve>

Gostou do post e achou útil? Dê um **like **❤ abaixo para ajudar na divulgação e para que mais pessoas tenham acesso **😄**, e volte amanhã para acompanhar essa série sobre Kubernetes !
