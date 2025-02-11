<?php
function gerarAposta($jogo, $numDezenas) {
    // Define as regras de cada jogo
    $regras = [
        "Mega-Sena" => [6, 20, 60, 5.00],
        "Quina" => [5, 15, 80, 2.00],
        "Lotomania" => [50, 50, 100, 2.50],
        "Lotofácil" => [15, 20, 25, 2.50]
    ];
    
    
    if (!isset($regras[$jogo])) {
        echo "Jogo inválido!\n";
        return null;
    }
    
    
    list($minDezenas, $maxDezenas, $totalNumeros, $precoBase) = $regras[$jogo];
    
    // Verifica se o número de dezenas está dentro do intervalo permitido
    if ($numDezenas < $minDezenas || $numDezenas > $maxDezenas) {
        echo "Número de dezenas inválido! Para $jogo, escolha entre $minDezenas e $maxDezenas números.\n";
        return null;
    }
    
    
    $numerosDisponiveis = range(1, $totalNumeros);
    $aposta = array_rand(array_flip($numerosDisponiveis), $numDezenas);
    sort($aposta);
    
    
    $custo = $precoBase * ($numDezenas / $minDezenas);
    
    return [$aposta, $custo];
}

function exibirMenu() {
    echo "Bem-vindo ao gerador de apostas! :)\n";
    echo "Escolha um jogo:\n";
    echo "1 - Mega-Sena\n2 - Quina\n3 - Lotomania\n4 - Lotofácil\n";
}

function main() {
    exibirMenu();
    
   
    $opcoes = ["1" => "Mega-Sena", "2" => "Quina", "3" => "Lotomania", "4" => "Lotofácil"];
    $escolha = readline("Digite o jogo que deseja jogar! ");
    
    
    if (!isset($opcoes[$escolha])) {
        echo "Opção inválida!\n";
        return;
    }
    
    $jogo = $opcoes[$escolha];
    $numApostas = (int)readline("Quantas apostas você quer gerar? ");
    $numDezenas = (int)readline("Quantas dezenas deseja escolher para $jogo? ");
    
    echo "\nApostas geradas para $jogo:\n";
    $totalGasto = 0;
    
    
    for ($i = 0; $i < $numApostas; $i++) {
        $resultado = gerarAposta($jogo, $numDezenas);
        if ($resultado) {
            list($aposta, $custo) = $resultado;
            echo "Aposta " . ($i + 1) . ": " . implode(", ", $aposta) . " - Você gastou: R$ " . number_format($custo, 2, ',', '.') . "\n";
            $totalGasto += $custo;
        }
    }
    
    echo "\nTotal gasto: R$ " . number_format($totalGasto, 2, ',', '.') . "\n";
}

main();
