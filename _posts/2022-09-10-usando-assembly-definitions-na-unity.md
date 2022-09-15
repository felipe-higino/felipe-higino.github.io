---
lng_pair: id_asmdefs1
title: Usando Assembly Definitions na Unity
category: Unity
tags: [Unity, Tools, Architecture]
img: ":id_asmdefs1/asmdef-asmref-thumb.png"
date: 2022-09-10 00:24:44 +0000
---

[next article]:https://google.com
[asmdef Unity documentation]:https://docs.unity3d.com/Manual/ScriptCompilationAssemblyDefinitionFiles.html
[Unity Test Framework]:https://docs.unity3d.com/Packages/com.unity.test-framework@1.1/manual/edit-mode-vs-play-mode-tests.html

## TL;DR

### Por quê devo usar *assembly definitions*?
- aumenta a granularidade nas features
- melhor gerenciamento de dependências (melhor arquitetura de código)
- melhor testabilidade (integração com testes unitários)
- reduz o tempo de compilação (diminui exponencialmente os tempos de iteração)

### Por quê não devo usar *assembly definitions*?
- o uso excessivo pode aumentar o consumo de memória em runtime
- familiaridade do time
    - pode causar conflitos de merge
    - não entender como funcionam dependências de dlls

## O que é .dll

`.dll` (dynamic link libraries) são arquivos que contém referências para o código do programa em questão.

Na Unity, a **principal dll** chama-se `Assembly-Csharp.dll`. Por padrão, os scripts criados pelo usuário pertencem a essa `.dll`, como é possível observar no inspector de qualquer script criado da maneira tradicional:

![Unity Assembly-Csharp](:id_asmdefs1/assembly-csharp.drawio.png)

## O que é .asmdef

A Unity permite que o usuário crie suas próprias `.dll`s e isole seus códigos do restante da aplicação. Para isso, é necessário que o usuário crie um asset `.asmdef`.

Assets `.asmdef` notificam a Unity que todos os scripts da pasta (e subpastas) em que ele está contido devem ser compiladas em uma `.dll` separada.

Esses assets podem ser criados pelo editor, clicando com o botão direito na pasta desejada e selecionando *Create/Assembly Definition*:

![create assembly definition](:id_asmdefs1/create-asmdef.gif)

> Note como o inspector muda o Filename de `Assembly-Csharp.dll` para `Custom.dll`

## E qual é a vantagem disso?

Criar código para jogos é uma tarefa complexa. 😲

E fica cada vez mais complexa com as constantes adições de códigos, refatorações e remoções de *features*.

### Vantagem 1: Dividir e Conquistar

Dividir o projeto em pequenas partes (no caso, em pequenas `.dll`s) ajuda a diminuir a complexidade dessa tarefa, ao passo que permite melhorar o gerenciamento de dependências e a divisão de responsabilidades – *o que certamente está bastante alinhado com os princípios SOLID*.

Quando um `.asmdef` é criado, o código é isolado de todo o restante da aplicação. 

Para utilizar lógicas externas (e criar uma dependência), basta referenciar outro `.asmdef` através da janela:

![Dependency *assembly definitions*](:id_asmdefs1/dependencies-asmdef.gif)

Visualmente, esse é o antes e depois do *grafo de dependências* entre as `.dll`s:

![Dependency graph *assembly definitions*](:id_asmdefs1/dependency-graph.drawio.png)

É possível remover a dependência do `Assembly-Csharp.dll`, e também referenciar `.dll`s pré-compiladas. Esses tópicos serão abordados no próximo artigo [Entendendo o inspector dos .asmdefs][next article]

### Vantagem 2: Reduzir o tempo de compilação

Sabe aquela sensação que o projeto da Unity começa a ficar lento com o tempo? 

Pois é, muito provavelmente é porque que toda a base de código está centralizada na `Assembly-Csharp.dll` – e a cada mudança, **TODOS OS SCRIPTS** são recompilados.

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


Caso algum script da pasta ***"minhaPasta1"*** seja modificado, apenas a dll ``minhaDll1.dll`` será recompilada, enquanto ``minhaDll2.dll`` e ``minhaDll3.dll`` permanecerão intactas.

Com isso, poupa-se o tempo de recompilar vários scripts desnecessariamente.

## O que são .asmrefs

Colocar todos os scripts dentro de uma pasta pode dificultar a organização do seu projeto. 

Para resolver esse problema, existem os assets `.asmref` (***Assembly Definition References***), onde é possível incluir qualquer pasta do projeto na `.dll` desejada.

Para criar `.asmref`s, basta selecionar *Create/Assembly Definition Reference* e referenciar o `.asmdef` desejado:

![How to create Assembly References](:id_asmdefs1/asmref.gif)

> Aqui, qualquer script na pasta ***MyFolder3*** será compilado na `.dll` `Custom2.dll`

## Conclusão

*Assembly Definitions* e Assembly Definition References são um excelente recurso para produtividade e organização de código. 

Várias features desse sistema não foram abordadas neste artigo. Por esse motivo, seguem alguns links úteis para quem queira entender mais sobre o assunto:

Para ter uma visão completa sobre o sistema, acesse a documentação oficial da Unity sobre *Assembly Definitions* [neste link][asmdef Unity documentation]

Para entender como *Assembly Definitions* são integrados com testes unitários, acesse a documentação do *Test Framework* [neste link][Unity Test Framework]

Acesse o próximo artigo – [Entendendo o inspector dos .asmdefs][next article] para conhecer mais sobre outras funcionalidades interessantes dos `.asmdef`s.
