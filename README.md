# Documentação de Testes de Software

Clarisse Scofield Lenzoni | 2018054699

## Moment
Uma biblioteca de datas JavaScript para analisar, validar, manipular e formatar datas. Veremos alguns exemplos de teste dessa biblioteca de datas muito utilizada nas mais diversas aplicações em JS.
Algo muito interessante dessa biblioteca é o fato de possuir mais de 160.000 testes feitos e nenhum falho, de acordo com o site oficial. Essa biblioteca apresenta uma grande variedade de funções disponíveis para os usuários, pode-se verificar mais detalhes na documentação: [Documentação](https://momentjscom.readthedocs.io/en/latest/)

O código aberto está disponível no repositório do Github: https://github.com/moment/moment que foi utilizado de base para o desenvolvimento do trabalho e os exemplos de teste.

As funções aqui escolhidas foram variadas entre as diferentes áreas da biblioteca a fim de abarcar uma maior variedade de comparações. O exemplo 1 corresponde ao Time from now - Display, o exemplo 2 corresponde a uma query de verificação do tempo e, por fim, o exemplo 3 corresponde a uma manipulação das datas.

### Exemplo 1: fromNow
Moment implementa uma função chamada fromNow usada para calcular o tempo a partir de agora no Node.js. Esta função retorna o tempo, gasto a partir da data que é passada no parâmetro.

**Sintaxe:** moment().fromNow (booleano);
**Parâmetros:** esta função possui um parâmetro booleano opcional que é passado para obter o valor sem o sufixo.
**Valor de retorno:** esta função retorna a data calculada.

O teste para essa função é o seguinte:

~~~javascript
test('fromNow', function (assert) {
    assert.equal(
        moment().add({ s: 30 }).fromNow(),
        "oor 'n paar sekondes",
        'in a few seconds'
    );
    assert.equal(moment().add({ d: 5 }).fromNow(), 'oor 5 dae', 'in 5 days');
});
~~~
Esse teste verifica a função fromNow utilizando do assert.equal para duas condições (vale mencionar que o assert.equal retorna erro caso o teste retorne algo diferente do esperado). Primeiro o teste verifica a diferença para 30 segundos e a diferença de 5 dias a partir da data de agora.

### Exemplo 2: isAfter

Essa função verifica se um momento é após outro momento. O primeiro argumento será analisado como um momento, se já não for assim. 

Se você quiser limitar a granularidade a uma unidade diferente de milissegundos, passe as unidades como o segundo parâmetro.
Como o segundo parâmetro determina a precisão, e não apenas um único valor a ser verificado, usar dia irá verificar ano, mês e dia.

Exemplos de uso:
~~~javascript
moment('2010-10-20').isAfter('2010-10-19'); // true
moment('2010-10-20').isAfter('2010-01-01', 'year'); // false
moment('2010-10-20').isAfter('2009-12-31', 'year'); // true
moment().isAfter(); // false - se nada é passado como parâmetro, o default é o tempo atual.
~~~

Diversos testes são realizado para essa função, os testes para cada unidade e suas respectivas respostas esperadas. Para análise, escolhe-se o teste de "is after year" em que a unidade passada para comparação é o ano. 

O teste para essa função específica é o seguinte:
~~~javascript
test('is after year', function (assert) {
    var m = moment(new Date(2011, 1, 2, 3, 4, 5, 6)),
        mCopy = moment(m);
    assert.equal(
        m.isAfter(moment(new Date(2011, 5, 6, 7, 8, 9, 10)), 'year'),
        false,
        'year match'
    );
    assert.equal(
        m.isAfter(moment(new Date(2010, 5, 6, 7, 8, 9, 10)), 'years'),
        true,
        'plural should work'
    );
    assert.equal(
        m.isAfter(moment(new Date(2013, 5, 6, 7, 8, 9, 10)), 'year'),
        false,
        'year is later'
    );
    assert.equal(
        m.isAfter(moment(new Date(2010, 5, 6, 7, 8, 9, 10)), 'year'),
        true,
        'year is earlier'
    );
    assert.equal(
        m.isAfter(moment(new Date(2011, 0, 1, 0, 0, 0, 0)), 'year'),
        false,
        'exact start of year'
    );
    assert.equal(
        m.isAfter(moment(new Date(2011, 11, 31, 23, 59, 59, 999)), 'year'),
        false,
        'exact end of year'
    );
    assert.equal(
        m.isAfter(moment(new Date(2012, 0, 1, 0, 0, 0, 0)), 'year'),
        false,
        'start of next year'
    );
    assert.equal(
        m.isAfter(moment(new Date(2010, 11, 31, 23, 59, 59, 999)), 'year'),
        true,
        'end of previous year'
    );
    assert.equal(
        m.isAfter(moment(new Date(1980, 11, 31, 23, 59, 59, 999)), 'year'),
        true,
        'end of year far before'
    );
    assert.equal(
        m.isAfter(m, 'year'),
        false,
        'same moments are not after the same year'
    );
    assert.equal(+m, +mCopy, 'isAfter year should not change moment');
});
~~~

Nesse teste, diversas condições são testadas utilizando-se do mesmo assert.equal anterior, em que o retorno é falso caso os parâmetros passados não sejam iguais. Note que na primeira linha do teste é determinado uma data que será utilizada para comparação da função isAfter, define-se o ano de 2011. Cada um dos testes feitos irá comparar retornando true ou false para cada ano passado em diferentes condições: anos muito antigos, o ano anterior, o ano seguinte, etc.

### Exemplo 3: startOf

Essa função muda o momento original configurando-o para o início de uma unidade de tempo.

Exemplo de uso da função:

~~~javascript
moment().startOf('year');    // set to January 1st, 12:00 am this year
moment().startOf('month');   // set to the first of this month, 12:00 am
moment().startOf('quarter');  // set to the beginning of the current quarter, 1st day of months, 12:00 am
moment().startOf('week');    // set to the first day of this week, 12:00 am
moment().startOf('isoWeek'); // set to the first day of this week according to ISO 8601, 12:00 am
moment().startOf('day');     // set to 12:00 am today
moment().startOf('date');     // set to 12:00 am today
moment().startOf('hour');    // set to now, but with 0 mins, 0 secs, and 0 ms
moment().startOf('minute');  // set to now, but with 0 seconds and 0 milliseconds
moment().startOf('second');  // same as moment().milliseconds(0);
~~~

Assim como para as outras funções, vários testes de verificação são feitos para a função startOf. Um dos testes que será analisado verifica o "start of year".

Teste para verificação do início do ano com essa função:
~~~javascript
test('start of year', function (assert) {
    var m = moment(new Date(2011, 1, 2, 3, 4, 5, 6)).startOf('year'),
        ms = moment(new Date(2011, 1, 2, 3, 4, 5, 6)).startOf('years'),
        ma = moment(new Date(2011, 1, 2, 3, 4, 5, 6)).startOf('y');
    assert.equal(+m, +ms, 'Plural or singular should work');
    assert.equal(+m, +ma, 'Full or abbreviated should work');
    assert.equal(m.year(), 2011, 'keep the year');
    assert.equal(m.month(), 0, 'strip out the month');
    assert.equal(m.date(), 1, 'strip out the day');
    assert.equal(m.hours(), 0, 'strip out the hours');
    assert.equal(m.minutes(), 0, 'strip out the minutes');
    assert.equal(m.seconds(), 0, 'strip out the seconds');
    assert.equal(m.milliseconds(), 0, 'strip out the milliseconds');
});
~~~

Esse teste é autoexplicativo, basta verificar que cada um dos assert.equal utilizam de funções de get, como m.year() que retorna o valor do ano para comparação com o resultado obtidos no startOf declarado nas linhas superiores. O mesmo segue nas linhas e comparações seguintes. Se o resultado for o esperado o teste tem o sucesso, se não retorna a mensagem seguinte como acontece em todos os outros testes também apresentados.
