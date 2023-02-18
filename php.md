#PHP 8


## Resumo da história

 - Surgiu em 1994
 - Seu criado Ramus Lerdof
 - PHP - Personal Home Page
 - Trabalho com Orienteção a Objeto

**Framework**

 - Laravel
 - Sysmfony
 - Codeigniter

**CMS** - Content Management System / Sistema de Gerenciamento de Conteúdo
Gerenciar conteúdo em plataformas digitais

 - WordPress
 - Drupal
 - Magento
 - Sugarcrm
 - elgg
 - phpBB
 - Moodle

## Instalação

Pode-se instalar o **XAMPP** ou realizar a instalação via pacote **"APT"**.

```
$ apt install php8.1-cli
```

Para instalar o XAMPP Apache será necessário realizar o Download, navegar até a pasta onde esta o arquivo baixado e via terminal executar o seguinte comando, para dar permissões ao XAMPP
```
$ chmod 777 -R <NOME DO ARQUIVO>

//Depois

$ sudo ./<NOME DO ARQUIVO>  //Irá abrir o executável de instalação
```
Em seguida realizar a instalação do XAMPP

### Executar o XAMPP

Exitem duas maneiras de realizar a execução, para ambas as formas será necessário navegar até a pasta **/opt/lammp** e rodar o comando
  ```
  //LINHA DE COMANDO
  $ sudo ./xampp start 

  $ sudo ./xampp stop

  //APP EXECUTÁVEL
  $ sudo ./manager-linux-64.run
  ```

*OBS* Na forma de APP executável o terminal ficará preso a execução.

##Primeiro código

Os documentos do PHP via XAMPP no linux ficam gravados no caminho **/opt/lampp/htdocs/**, aqui serão gravados os arquivos necessários a execução da aplicação.

## Documentação

*site* https://www.php.net/

## Variáveis e Constantes

```
<?php
    $variavel = "Declaração de uma variável";

    define("estadoBrasileiros", "SP, PR, BA, ES, RJ ,,,"); // Declaração de uma constante

    echo $variavel //Para exibir o resultado de uma variável

    echo valorDia //Para exibir o resultado de uma constante
?>
```

**Array**
É uma estrutura de dados que armazena uma coleção de elementos de tal forma que cada um dos elementos possa ser identificado por, pelo menos, um índice ou uma chave

 - Armazena múltiplos valores em uma única variável
 - Array Númericos
 - Array Asociativos
 - Array MultiDimensionais

```
<?php

    $carros = ['Ferrari', 'BMW', 'Mercedes'];

    print_r($carros);

?>
```

Array asociativo
```
<?php

  $endereco = [
    'cep' => '1123123132131',
    'numero' => '123123',
    'cidade' => 'São Paulo',
    'estado' => 'SP'
  ];

?>

```

```
<?php

	$enderecoPessoas = [
		'pessoa1' => array(
			'cep' => '123123-123',
			'cidade' => 'São Paulo'
		),
		'pessoa2' => array(
			'cep' => '321321-321',
			'cidade' => 'Paraná'
		)
		];

	print_r($enderecoPessoas['pessoa1']['cep']);

?>
```

```
<?php
    $cursos = array("PHP", "JAVA", "Python", "C");

    print_r($cursos);
    print_r($cursos[2]);
?>
```
Retorno do print
Array
(
    [0] => PHP
    [1] => JAVA
    [2] => Python
    [3] => C
)

Retorno do segundo print
Python


### Escopo Global e Local de uma variável

```
<?php

    // Escopo local neste caso e função precisa retornar um valor e chamamos a função para executar o programa
    function soma() {
        $x = 10 + 40;
        return $x;
    }

    echo soma();

?>

<?php
    // Escopo global, a variável é declarada fora da função e para ser utilizada é necessário declarar "global" e o nome da variável desejada.
    $a = 50;

    function soma2() {
        global $a;
        $x = $a + 40;
        return $x;
    }

    echo soma2();
?>
```

## Tipos de dados

**Número inteiros**
 - Interger (utilizado para armazenar números menores)
 - Long (utilizados para armazenar números maiores)

  **Função var_dump()** apresenta o valor da variável que for passada como parâmetro e apresenta também o tipo da variável

**Data**
 - echo date("d/m/Y"); //Roda o dia atual, isso para o mês "m" ou "M" para iniciais do mês e "y" e/ou "Y" para o ano

**Hora**
Por padrão irá apresentar o fusiorário dos EUA para usar o Brasileiro é necessário usar uma função para configurar

```
<?php
      date_default_timezone_set('America/Sao_Paulo');
      $data = date ("d/m/Y H:i:s¨);
      echo $data
?>
```

## Função ECHO e PRINT

**Função print** suporta apenas uma string
**Função echo** permite mais de uma string


## Rodar um servidor local do PHP

```
$ PHP -S localhost:8080
```


## Operadores

### Operadores lógicos e booleanos

$a and $b // E
$a or $b // OU
$a xor $b //XOR quado um dos dados for verdadeiro, porém não ambos os dados

!$a //Não
$a && $b //E
$a || $b //OU

A diferença em usar o símbolos "&&" ou "||" ao invés de "and" ou "or" respectivamente, é a ordem com que o PHP irá executar o código, neste caso por ordem de precedência, será exetucado primeiro os comandos que contém os símbolos.

```
<?php

        $bool = true && false; //false
        var_dump($bool);

        $bool = true and false; //true neste caso o PHP primero executou o comando de atribuição de valor para a variável e não resolveu a expressão
        $bool = (true and false); // Desta forma será resolvido primeiro o que está entre parenteses
        var_dump($bool);

        var_dump( 7 == 7 and 9 < 7);
?>
```

### Operadores condicionais

IF ELSE ELSEIF

```
<?php
    $nota = 6;

    if ($nota >= 7) {
        echo "Aluno(a) aprovado!";
      }else {
        echo "Aluno(a) reprovado!";
      }
?>
```

Podemos escrever essa condição em apenas uma linha
```
<?php

  $nota = 6;

  echo $nota >= 7 ? "Aluno(a) aprovado!" : "Aluno(a) reprovado!"
?>
```

```
<?php
    $nota = 6;
    if ($nota >= 7) {
        echo "Aluno(a) aprovado!";
      } else if (($nota < 7) && ($nota => 4)) {
        echo "Aluno(a) está em recuperação!"
      }
      else {
        echo "Aluno(a) reprovado!";
      }
?>
```

### Switch Case

Avalia apenas a condição de **igualdade**
O **break** é importante para que o switch pare a execução, caso o condição seja atendida.
A utilização do **default** não é obrigatória.


```
<?php
    
    $sorteio = rand(0,5);

    echo "Valor sorteado: " . $sorteio;
    switch($sorteio){
      case 0:
            echo " => Você ganhou 2 pontos";
            break;
      case 1:
            echo " => Você perdeu 1 ponto";
            break;
      case 3:
            echo " => Você ganhou um bônus! parabéns ganhou 3 pontos";
            break;
      default:
            echo " => Jogue novamente";
    }
?>
```


## Laços de Repetição

Comando de Incremento / Decremento

 * ++$a Pré-incremento > Incrementa $a em um, e então retorna $a;
 * $a ++ Pós-incremento > Retorna $a, e então incrementa $a em um;
 * --$a Pré-decremento > Decrementa $a em um, e então retorna $a;
 * $a-- Pós-decremento > Retorna $a, e então decrementa $a em um.

 *Obs* Tendo uma variável de valor **null** o Decremente não tem valor nenhum, porém o Incremento adicionar valor a variável.

 
 **Estrutura de repetição: FOR**
Quando sabemos o quantidade de repetição necessária

```
<?php

        $arrayNumeros = [3,5,6,1,2,7];

        $qtd_elementos_array = count($arrayNumeros);
        var_dump($qtd_elementos_array); //Comando para saber a quandade de informações que possui no array

        for ($i = 0; $i < $qtd_elementos_array; $i++) {
                print_r($arrayNumeros[$i]);
        }

?>
```
