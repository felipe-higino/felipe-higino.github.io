---
lng_pair: id_asmdefs1
title: Usando Assembly Definitions na Unity
category: Unity
tags: [Unity, Tools, Architecture]
img: ":id_asmdefs1/asmdef-asmref-thumb.png"
date: 2022-09-10 00:24:44 +0000
---

Por quê você deveria usar `Assembly Definitions` no seu projeto Unity? E por que você NÃO deveria?

Para quem não tem tempo de ler tudo (infelizmente 🙁), aqui vai um resumo:

## TL;DR

### Por quê sim
- garante granularidade nas featuras
- melhor gerenciamento de dependências (melhor arquitetura de código)
- melhor testabilidade (integração com testes unitários)
- reduz o tempo de compilação (diminui exponencialmente os tempos de iteração)

### Por quê não
- o uso excessivo pode aumentar o uso de memória em runtime
- familiaridade do time
    - pode causar conflitos de merge
    - não entender como funcionam dependências de dlls

### WIP