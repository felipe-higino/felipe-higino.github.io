---
lng_pair: id_asmdefs1
title: Usando Assembly Definitions na Unity
category: Unity
tags: [Unity, Tools, Architecture]
img: ":id_asmdefs1/asmdef-asmref-thumb.png"
date: 2022-09-10 00:24:44 +0000
---

## TL;DR

### Por quê devo usar assembly definitions?
- garante granularidade nas featuras
- melhor gerenciamento de dependências (melhor arquitetura de código)
- melhor testabilidade (integração com testes unitários)
- reduz o tempo de compilação (diminui exponencialmente os tempos de iteração)

### Por quê não devo usar assembly definitions?
- o uso excessivo pode aumentar o uso de memória em runtime
- familiaridade do time
    - pode causar conflitos de merge
    - não entender como funcionam dependências de dlls

## O que é .dll

`.dll` (dynamic link libraries) são arquivos que contém referências para o código do programa em questão.

Na Unity, a **principal dll** chama-se `Assembly-Csharp.dll`. Por padrão, os scripts criados pelo usuário pertencem a essa `.dll`, como é possível observar no inspector de qualquer script criado da maneira tradicional:

![unity Assembly-Csharp](:id_asmdefs1/assembly-csharp.drawio.png)

## O que é .asmdef

A Unity permite que o usuário crie suas próprias `.dll`s e isole seus códigos do restante da aplicação, e isso é feito através de um asset `.asmdef`.

Assets `.asmdef` notificam a Unity que todos os scripts da pasta (e subpastas) em que ele está contido devem ser compiladas em uma `.dll` separada.

Esses assets podem ser criados pelo editor, clicando com o botão direito na pasta desejada e selecionando *Create/Assembly Definition*:

![create assembly definition](:id_asmdefs1/create-asmdef.gif)
> Note como o inspector muda o Filename de `Assembly-Csharp.dll` para `Custom.dll`

## E qual é a vantagem disso?

Criar código para jogos é uma tarefa complexa. 😲

E fica cada vez mais complexa com as constantes adições de códigos, refatorações e remoções de *features*.

### Vantagem 1: Dividir e Conquistar

Ao dividir o projeto em pequenas partes (no caso, em pequenas `.dll`s) ajuda a diminuir a complexidade dessa tarefa, ao passo que permite melhorar o gerenciamento de dependências e a divisão de responsabilidades – *o que certamente está bastante alinhado com os princípios SOLID*.

### Vantagem 2: Reduzir o tempo de compilação

Sabe aquela sensação que o projeto da Unity começa a ficar lento com o tempo? Pois é, muito provavelmente é porque que toda a base de código está centralizada na `Assembly-Csharp.dll` – e a cada mudança, **TODOS OS SCRIPTS** são recompilados.

Com `.dll`s menores, a situação muda. Imagine a seguinte situação:

``` txt
📁Assets
    📁minhaPasta1
        // vários scripts aqui
        🔹minhaDll1.asmdef
    📁minhaPasta2
        // vários scripts aqui
        🔹minhaDll2.asmdef
    📁minhaPasta3
        // vários scripts aqui
        🔹minhaDll3.asmdef
```

Caso algum script da pasta ***"minhaPasta1"*** seja modificado, apenas a dll ***"minhaDll1.dll"*** será recompilada, enquanto ***"minhaDll2.dll"*** e ***"minhaDll3.dll"*** permanecerão intactas.

Com isso, poupa-se o tempo de recompilar vários scripts desnecessariamente.

## O que são .asmrefs

<!-- 
TODO:
links unity
tradução para inglÊs
call-to-action do post sobre opções do assembly definition
 -->
<!-- Entendendo opções dos .asmdefs (outro artigo) -->
