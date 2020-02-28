---
layout: post
title:  "Imutabilidade"
date:   2020-02-26 17:18:58 -0300
categories: programacao js
---
Ao falar sobre imutabilidade o pensamento que nos vem a cabeça é: _simples, é só fazer cópia das variáveis ao invés de manipular a mesma_.  
De fato, a cópia ao invés do reúso dos objetos é um dos pilares da imutabilidade. Mas ao trabalharmos dessa forma, resolveremos o problema de mutação, mas criaremos outro: consumo de memória e processamento excessivos.

Considerando o primeiro cenário (cópia), é possível trabalhar da seguinte maneira:

{% highlight js %}
const arr = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
const arr2 = [...arr]
arr2[0] = 'x'
// arr2 => ['x', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
{% endhighlight %}

Ao fazer isso, internamente são criados dois arrays completamente diferentes, porém com alguns valores em comum.  
Não seria mais interessante se fosse possível fazer o que aparentemente fazemos com esse código: criar o `arr2` "baseado" em `arr`, dessa forma otimizando memória?

Sim, com certeza.  
Por sorte isso é possível \o/

Uma forma "simples" de fazer isso é dividindo o nosso array em pequenos arrays e dessa forma, juntar as partes necessárias.

{% highlight js %}
const ab = ['a', 'b']
const cd = ['c', 'd']
const ef = ['e', 'f']
const gh = ['g', 'h']

const arr2 = ['x', 'b', ...cd, ...ef, ...gh]
// arr2 => ['x', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
{% endhighlight %}

_Voilà_: temos agora o `arr2` construído sem precisarmos sobrecarregar a memória.

A solução funciona, mas dá um baita trabalho, não?

Por sorte, alguém já parou para pensar em uma solução para situações como essas (e com manipulação de objetos também!)

Em JavaScript existe uma lib chamada [Mori](https://swannodette.github.io/mori) que nos ajuda exatamente com isso.  
Ela faz diversas operações em arrays e objetos, mantendo a imutabilidade sem forçar memória ou processamento.  
Vamos ver o mesmo código que os anteriores, mas utilizando o mori:

{% highlight js %}
const mori = require('mori')

const arr = mori.vector('a', 'b', 'c', 'd', 'e', 'f', 'g', 'h')
const arr2 = mori.assoc(arr, 0, 'x')
// arr2 => ['x', 'b', 'c', 'd', 'e', 'f', 'g', 'h']
{% endhighlight %}


Legal, né?  
Mas.. qual é a mágica por trás dessa lib?
Bom, a solução é bem complexa, mas de uma maneira bem simplista, podemos pensar no acesso a cada posição do array através de árvore, organizada pelos hashs dos elementos. Aqui tem um vídeo da Anjana Vakil na jsconf.eu onde ela explica isso com mais detalhes.

{% include youtubePlayer.html id='Wo0qiGPSV-s' %}
