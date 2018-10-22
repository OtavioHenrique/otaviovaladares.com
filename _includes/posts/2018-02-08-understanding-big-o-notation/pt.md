*Big O* é uma famosa notação da ciência da computação, porém esse tópico ainda da medo em muitas pessoas. Alguns programadores não tem um curso completo em Ciência da Computação e nunca viram este tópico e alguns que mesmo fazendo o curso muitas vezes acabam não tendo um completo entendimento do assunto.

Nesse texto eu vou explicar o que realmente é *Big O* e o que você realmente precisa saber.

Eu vou explicar o básico da notação *Big O* com exemplos escritos em Ruby.

*Gosto de escrever exemplos em Ruby, pois acho a linguagem mais fácil de ser lida por qualquer pessoa que já programe algo*

**Isso não pretende ser um guia completo de *Big O*, se você quer saber tudo sobre, no final do texto vou dar referências de como continuar seu estudo.**

# 1. Introdução

## Esse texto não é recomendado para

- Pessoas com um conhecimento sólido de *Big O*
- Pessoas que querem saber tópicos avançados de *Big O*

## O que vai ser explicado nesse post

- O que é *Big O*
- As notações mais comuns
- Exemplos simples
- Alguns Benchmarks

## 1.1 Primeiro de tudo, o que é Big O?

*Big O* é uma linguagem e métrica (podemos dizer assim) que usamos quando falamos sobre growth rates e eficiência de algoritimos. Podem ser usados para descrever o comportamento de um algoritimo em termos de crescimento no numero de oprações conforme o número de elementos aumenta, com essa técnica nós podemos ver como nosso algoritimo se comporta com muitos dados e operações.

*Se você quiser, pode ler uma explicação longa e mais profunda [aqui](https://en.wikipedia.org/wiki/Big_O_notation)*

## 1.2 Por que isso é tão importante? Eu nunca vou usar isso.

Um monte de métodos e estrutura de dados de muitas linguagens de programação são descritos usando Big O, como por exemplo a busca binária que no ruby é chamada ´array#bsearch´.

Primeiro de tudo, se você sempre trabalha com pequenas quantidades de *data* e operações, provavelmente você nunca vai precisar usar isso muito a fundo. Porém é bom entender os conceitos básicos de algoritimos, pois você pode ajudar você a tomar boas decisões de design no futuro.

Hoje em dia estamos na era do *Big Data*, toneladas de informações e operações são realidades. Você vai provavelmente fazer algoritimos pensando na complexidade de tempo e espaço.

Também é legal entender pois muitos métodos da sua linguagem de programação favorita tem diferentes complexidades, com *Big O* você pode entender melhor cada um.

*Também é importante lembrar que muitas big tech companys como Google, Facebook e Amazon fazem perguntas sobre Big O nas suas entrevistas, isso é tão frequente que o famoso livro Cracking The Code Interview tem um tópico discutindo apenas isso.*

## 2. Aprendendo

## 2.1 Visualizando a complexidade

Usando Big O nos temos um monte de possibilidades de notações, a lista mais comum de runtimes/notaões é: 

| Notation   | Short Description | Denomination |
|------------|-------------| ------------ |
| O(1)       | A melhor solução possível | Constante |
| O(log n)   | Quase bom, árvore binária e o algoritimo mais famoso O(log n) | Logaritimica |
| O(N)       | Quando andamos por todos os dados. Solução "OK" | Linear |
| O(n log n) | Não é uma solução ruim. Algoritimo mais famoso é Merge Sort. | nlogn |
| O(N^2)      | Horrivel, vamos ver o exemplo a seguir | Quadratico |
| O(2^n)      | Horrivel, algoritimo mais famoso é o quicksort             | Exponential |
| O(N!)      | A solução mais horrível, fatorial é quando você testa cada solução possível | Fatorial |

![Big O complexy chart](https://s3.amazonaws.com/garagelabio/big-o/bigo.png)

## 2.2 Time or Space?

Você pode usar Big O para descrever tanto tempo quanto espaço, mas o que isso significa? Simples, um algoritimo que tem um runtime *O(N)* pode crescer o espaço usado a *O(N^2)*.

## 2.3 Tempo para o que?

 Estrutura de dados e algoritimos podem ter tempos diferentes para diferentes ações, por exemplo: Um array tem *O(1)* para acesso, porém é *O(N)* para procura (dependendendo de qual algoritimo você usar), tudo depende do escopo do seu código.

 [Nesse site você pode ver a complexidade de cada um dos mais famoso algoritimos e estruturas de dados](http://bigocheatsheet.com/)

 ## 2.4 Big O, Big Omega, Big Theta?

 Nesse post eu não vou falar sobre *Big Omega* e *Big Theta* por duas razões:
 
 - Isso é um texto simples e eu não quero mergulhar fundo em conceitos mais avançados.
 - A industria tende a usar mais a notaão *Big O*.

 # 3. Mãos a massa

 ## 3.1 Apresentando o nosso problema

 Na sua vida você ja pensou que algumas ações do nosso dia demoram muito tempo para serem feitas, e que você pode fazer algumas coisas simples para reduzir o tempo perdido? Vamos usar um exemplo simples nesse texto, um dicionario, quanto tempo você precisa para achar uma palavra nele? Depende do jeito que você procurar.

 *"A segunda edição do 20º Oxford English Dictionary tem 171.476 palavras em uso e 47,156 palavras obsoletas."*

 Isso é um monte de coisa certo? Vamos imaginar que nos estamos construindo um algoritimo para procurar por uma palavra.

 #### Code

 *Para fins educacionais, todos exemplos vão usar um array de números, não strings*

 Vamos criar um array que iremos usar em todos exemplos, esse array vai ser nosso dicionario, mas no lugar de palavras ele vai ter numeros. A palavra "zoo" vai ser a última palavra do nosso dicionario o que no caso seria o numero 1000000. Como você pode ver a seguir:

 ```ruby
dictionary = []

0.upto(1000000) do |number|
  dictionary << number
end
 ```

 Agora temos uma array com um milhão de numeros. Vamos assumir que esse array de numeros é o nosso dicionario e que a palavra de número 1000000 seria a palavra "zoo" que estamos procurando.

 ### 3.1.1 Primeira Solução O(N)

 Considerando que esse dicionario está em ordem alfabética, quanto tempo você vai demorar para achar a palavra "zoo" (our 1000000) se você for palavra por palavra? 

 Sim, para char a nossa última palavra nós vamos perder muito tempo, você vai precisar ler todas palavras. Esses cenários quando andamos por todas informações, usando *Big O* nos chamamos de *O(N)*, pois, o algoritimo vai demorar N tempo para achar a solução, onde N é o numero de palavras.

 #### Code

 Vamos assumir que vamos andar palavra por palavra até achar a palavra que estamos procurando:

 ```ruby
def o_n(dictionary)
  dictionary.each do |word|
    word if word == 1000000
  end
end

o_n(dictionary)
```

Isso é bem comum e não é uma solução muito ruim, mas também não é boa. Em termos de dificuldade para entender também não é tão complexo, muitos exemplos de algoritimos *O(N)* no nosso dia a dia vem de algoritimos de loop em arrays.

### 3.1.2 Segunda Solução *O(1)*

*O(1)* é um algoritimo que independente do tamanho do input ele sempre vai demorar o mesmo tempo para executar, o tempo dele é constante, o maior exemplo dele sem duvidas é acessar um array, no caso do nosso exemplo do dicionario seria como se você já soubesse onde está a palavra:

```ruby
def o_one(dictionary)
  dictionary[1000000]
end

o_one(dictionary)
```

*O(1)* sempre é a melhor solução, e você provavelmente usa isso todos os dias, o uso mais comum é o tempo de acesso em um [Hash](https://en.wikipedia.org/wiki/Hash_function).

Como muitas linguagens, Ruby já tem hash implementado.

#### Code

```ruby
hash = {
  color: "red"
}

hash[:color]

# => "red"
```

### 3.1.3 Terceira Solução *O(log n)*

Algumas pessoas tem muita dificuldade em entender esse conceito de *O(log N)*, mas eu espero que com esse exemplo você possa entender.

Vamos assumir que você não sabe onde a palavra está localizada, mas você save que se procurar olhando palavra por palavra(*O(N)*) você vai demorar muito, o que você vai fazer? Você pode começar abrindo o dicionário em uma página aleatória e procurar se as palavras dessa página vem antes ou depois da palavra que você está procurando, se for depois, você abre em uma página depois e continua assim.. repetindo até achar a palavra que está procurando.

Se você já estudou algoritimos você sab que isso na computação é bem famoso na [busca binária](https://en.wikipedia.org/wiki/Binary_search_algorithm) e ela também é o exemplo mais famoso de algoritimo *O(log n)*.

Vamos ver o código:

#### Code

Para esse exemplo eu vou usar uma simples implementação da busca binária (não recursiva).

```ruby
def binary_search(dictionary, number)
  length = dictionary.length - 1
  low = 0

  while low <= length
    mid = (low + length) / 2

    return dictionary[mid] if dictionary[mid] == number

    if dictionary[mid] < number
      low = mid + 1
    else
      length = mid - 1
    end
  end

  "Number/Word not found"
end

puts binary_search(dictionary, 1000000)

```

Para indentificar um algoritimo que é *O(log n)* você pode seguir dois passos:

- O próximo elemento que o algoritimo vai performar uma ação é um de algumas possibilidades, e apenas um vai ser escolhido.
- Os elementos de cada ação performada são digitos de N

*Você pode ver mais [aqui](https://stackoverflow.com/questions/2307283/what-does-olog-n-mean-exactly)*

A busca binária é um ótimo exemplo disso pois a cada tentativa é cortado pela metade o numero de elementos restantes.

Como muitas linguagens Ruby já tem a busca binária implementada na classe array, como `Array#bsearch`

```ruby
array = [1,2,3,4,5,6,7,8,9,10]

array.bsearch {|x| x >= 9 }

# => 9 
```

Você pode ler mais [aqui](http://ruby-doc.org/core-2.2.0/Array.html#method-i-bsearch)

[Se você quer entender o porque da notação se chamar O(log N) clica aqui](https://hackernoon.com/what-does-the-time-complexity-o-log-n-actually-mean-45f94bb5bfbf)

*Se você acha que isso não é o suficiente em explicação, siga os links que eu passo como referencia, não vou entrar em tópicos de matemática pois isso se trata de um texto para iniciantes.*

### 3.1.4 Quarta Solução *O(N^2)*

Imagina que você está procurando palavra por palavra igual no *O(N)* mas por alguma razão você precisa fazer sua busca de novo, isso é o *O(N^2)*. Com esse exemplo aplicado ao caso do dicionário é dificil entender pois não faz muito sentido, mas no código é mais facil.

O trick é basicamente pensar que é passar duas vezes por todas informações presentes, basicamente um loop dentro de um loop, sendo assim seria *N*N* o que daria *O(N^2)*.

#### Code

```ruby
def o_n2(dictionary)
  dictionary.each do |word|
    dictionary.each do |word2|
      word if word == 1000000
    end
  end
end

o_n2(dictionary)
```

Não fique apavorado, isso é bem comum, especialmente quando estamos começando que fazemos muitas vezes o famoso `quick sort` para ordernar um array, mas esse código não tem uma boa performace para grandes inputs. A melhor coisa a fazer é tentar reduzir ele a *O(log n)* que quase sempre da para fazer.

### 3.1.5 Quinta Solução *O(n log n)*

Essa é a solução mais dificil para mim, e eu admito que perdi algumas horas procurando na internet porque as definições que achei não foram satisfatorias (E não tinha muitas respostas na internet).

A maneira mais simples que encontrei de explicar foi:

*O(n log n) é basicamente quando você roda N vezes uma ação que custa O(log n)* 

Ok, isso é obvio, mas faz sentido. Vamos examinar:

Em uma arvore binária, inserir um novo nó a ela (em uma arvore balanceada) é *O(log n)*. Inserir uma lista de N elementos em uma arvore vai ser *O(n log n)*.

Muitas vezes algoritimos *O(n log n)* podem ser resultado da otimização de um algoritimo quadratico *(N^2)*, o exemplo mais famoso desse tipo de algoritimo é o *merge sort*.

Para essa definição eu infelizmente não consegui encaixar na historia do dicionario nada. Eu definitivamente não consigo pensar em  nada que iria fazer sentido na história do dicionaro. 

*Se você acha que isso não foi suficiente nas referencias você vai encontrar links para entender melhor.*

### 3.1.6 Sexta Solução *O(2^N)*

*O(2^N)* é quando um algoritimo é exponencial, é uma definição complexa assim como *O(n log n)*. Esse tipo de problema é quando um numero de instruções crescem exponencialmente, é comum achar isso em algoritimos recursivos, ou em alguns algoritimos de busca.

*Quando temos uma função recursiva que faz diversas chamadas, o runtime vai ser O(2^N)*

Se você procurar na internet você provavelmente vai ver muitas pessoas falando que o algoritimo de fibonacci recursivo é um bom exemplo:

```ruby
def fibonacci(number)
  return number if number <= 1

  fibonacci(number - 2) + fibonacci(number - 1)
end

puts fibonacci(10)
```

Para achar um *O(2^N)* você pode começar seguindo uma regra:

- *O(2^N)* é um algoritimo que o crescimento dobra a cada adição no input. (Exponencial)

Para esse exemplo também não irá ter exemplo na história do dicionario, sorry.

### 3.1.7 Sétima Solução *O(N!)*

A solução mais horrivel. Mas infelizmente não vou ter exemplo na história do dicionario também, e não quero pular de cabeça na parte matemática.

*O(N!) é toda vez que você calcula todas permutações de um array de tamanho N.*

O exeplo mais famoso dessa notação é resolver o problema do caixeiro viajante com brute-force(testando todas soluções possíveis). (Um monte de sites como a wikpedia também usam esse exemplo).

#### Code

No Ruby temos um método que retorna todas permutações de um array.

```ruby
array = [1,2,3]

array.permutation(3).to_a

# => [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]
```

Isso é *O(N!)*

# 4. Benchmarking

Vamos examinar alguns benchmark de alguns exemplos discutindos e comprar, para entender melhor e ver que a notação realmente faz sentido.

Nessa seção vou usar os algoritimos dos exemplos anteriores e usar o modulo do Ruby [Benchmark](http://ruby-doc.org/stdlib-2.0.0/libdoc/benchmark/rdoc/Benchmark.html) para comprar os tempos de execução.

## 4.1 *O(1)* vs *O(N)*

Para esse exemplo vamos usar o mesmo array dos exemplos anteriores com um milhão de numeros.

```ruby
0.upto(1000000) do |number|
  dictionary << number
end
```

Então se a gente rodar os algoritimos criados no capitulo anterior:

```ruby
Benchmark.bm { |bench| bench.report("o(1)") { o_one(dictionary) } }
Benchmark.bm { |bench| bench.report("o(n)") { o_n(dictionary) } }

#       user     system      total        real
# o(1)  0.000000   0.000000   0.000000 (  0.000005)
#       user     system      total        real
# o(n)  0.060000   0.000000   0.060000 (  0.052239)

```

Uma diferença bem pequena, e é por isso que *O(N)* no grafico de complexidade não é tão ruim assim, com um milhão de numeros ele precisou de apenas `0.052239s`, mas e se a gente subir nosso dataset para 15 milhões de numeros? A solução *O(N)* vai ser boa? Vamos ver:

```ruby
#       user     system      total        real
# o(1)  0.000000   0.000000   0.000000 (  0.000004)
#       user     system      total        real
# o(n)  0.750000   0.000000   0.750000 (  0.741515)
```

O tempo da solução *O(N)* cresceu algo em torno de 12.5x comparado com o array de um milhão de numeros, enquanto a solução *O(1)* continuou constante.

## 4.2 *O(N)* vs *O(N^2)*

Primeiro, vamos criar de novo o nosso array, mas desta vez com um pouco menos de numeros:

```ruby
data = []

0.upto(20000) do |number|
  data << number
end
```

Sim, duzentos mil é o suficiente para essa exemplo, porque *O(N^2)* é muito ruim, e se aumentarmos nosso array você provavelmente vai usar 100% da sua cpu no processo do Ruby e o sistema operacional vai matar esse processo.

Vamos examinar o runtime dos algoritimos que fizemos de *O(N)* e *O(N^2)* no array criado acima:

```ruby
Benchmark.bm { |bench| bench.report("o(n)") { o_n(data) } }
Benchmark.bm { |bench| bench.report("o(n^2)") { o_n2(data) } }

#       user     system      total        real
# o(n)  0.010000   0.000000   0.010000 (  0.000963)
#       user     system      total        real
# o(n^2) 19.550000   0.000000  19.550000 ( 19.560149)
```

Essa diferença é muito grande, e é ai que o *Big O* aparece, o nosso algoritimo *O(N^2)* demorou 19.5 segundos para executar.

*Estamos falando do comportamento do algoritimo em função do crescimento do input e das operações.*

Ok, você vai me falar que certamente 200 mil inputs é muito alto para o dia a dia da maioria das pessoas, e se a gente testar com 5 mil numeros?

```ruby
       user     system      total        real
o(n)  0.000000   0.000000   0.000000 (  0.000252)
       user     system      total        real
o(n^2)  1.330000   0.000000   1.330000 (  1.329480)
```

O código demorou 1.3 segundos para ser executado e temos que admitir que 5 mil inputs não é algo grande, ainda mais se tratando de numeros. Isso em uma aplicação final pode ser muito, pois vamos supor que seu cliente espere `1.329480` três vezes por dia, isso em uma semana vai dar `27.9s`, isso apenas  trabalhando com 5 mil numeros, imagina com mais.

Então em casos de *O(N^2)* temos que tentar reduzir eles para *O(n log n)* como falado mais a cima, e o mais importante, evitar usar algoritimos lentos como o quick sort em grandes quantidades de input.

## 4.2 *O(N!)*

Isso é um teste bem legal pois se a gente usar um milhão de numeros, no meu computador pode demorar algo em torno de 10 horas, e se usarmos cinco mil (igual no exemplo passado) vai demorar um bom tempo também, então para mostrar o *O(N!)* vou usar apenas 500 numeros para ver a demora.

Nesse exemplo vou usar o método `Array#permutation`, do Ruby.

```ruby
dictionary = []

0.upto(500) do |number|
  dictionary << number
end

Benchmark.bm { |bench| bench.report("o(!n)") { dictionary.permutation(3).to_a } }
```

E o output final é assustador:

```ruby
#       user     system      total        real
# o(!n) 25.530000   1.650000  27.180000 ( 28.155015)
```

`28.1s` com 500 numeros.

# 5. Encerrando

Eu não vou fazer benchmark de todos algoritimos e estrutura de dados (pois seria impossivel), mas eu recomendo que se você se interessou continue seus estudos em direção aos tópicos avançados.

## 5.1 What's next?

Se você querser um rockstar quando se trata de *Big O* keep studying.

Você pode ler:

- [Introduction to Algorithms, Third Edition](https://mitpress.mit.edu/books/introduction-algorithms)
- [Cracking the Coding Interview](https://www.amazon.com/Cracking-Coding-Interview-Gayle-McDowell/dp/0984782850/ref=as_li_ss_tl?ie=UTF8&linkCode=sl1&tag=careercup-ctciwebsite-20&linkId=173f3d8878a1d7f0d131a85fbfc9f67f) 

Como já falado, esse último livro tem um capitulo falando apenas de Big O para entrevistas!

Você também pode seguir os links que vou passar a seguir para continuar estudando por você mesmo.

## 6. Final thoughts

Esse post foi longo e eu sei disso! Mas eu acredito que entender muito bem os principios do Big O é muito bom para Engenheiros de Software não apenas para o dia-a-dia mas também para as entrevistas de emprego.

Se você tem alguma pergunta que eu possa ajudar, por favor pergunte! Pode me enviar email (otaviopvaladares@gmail.com) ou me mandar perguntas no [twitter](https://twitter.com/ValadaresOtavio) ou comentar esse post.

# 7. Reference

- [How to benchmark ruby](http://mitrev.net/ruby/2015/08/28/benchmarking-ruby/)
- [Whats does O(log n) means](https://stackoverflow.com/questions/2307283/what-does-olog-n-mean-exactly)
- [Good docs from one university](http://www.dca.fee.unicamp.br/~ting/Courses/ea869/faq1.html#item7)
- [How to calculate Big O](https://stackoverflow.com/questions/3255/big-o-how-do-you-calculate-approximate-it)
- [Linear time vs Quadratic Time](https://stackoverflow.com/questions/18023576/linear-time-v-s-quadratic-time)
- [How to calculate complexity of recursive functions](https://stackoverflow.com/questions/13467674/determining-complexity-for-recursive-functions-big-o-notation)
- [O(log n), why is that name?](https://hackernoon.com/what-does-the-time-complexity-o-log-n-actually-mean-45f94bb5bfbf)
- [Little bit more about O(log n)](https://stackoverflow.com/questions/5201013/on-log-log-n-time-complexity)
- [And more about O(log n)](https://stackoverflow.com/questions/36318159/big-on-log-n-explanation)
- [Time complexity wikpedia](https://en.wikipedia.org/wiki/Time_complexity)
- [Good video about](https://www.youtube.com/watch?v=MyeV2_tGqvw)