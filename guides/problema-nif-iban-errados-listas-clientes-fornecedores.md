---
title: "O problema dos NIF e IBAN errados nas listas de clientes e fornecedores"
description: "Porque é que os NIF e IBAN errados chegam às bases de dados, que consequências têm e como se detetam antes de causarem problemas, em Portugal e Espanha."
---

# O problema dos NIF e IBAN errados nas listas de clientes e fornecedores

## O problema

Quem trabalha com listas de clientes ou fornecedores conhece bem a situação: um ficheiro com centenas de linhas, importado de um formulário, de uma folha de cálculo antiga ou de um sistema de faturação diferente, e com dados que nunca foram verificados. Um número de identificação fiscal (NIF) com um dígito trocado, um IBAN com um espaço no sítio errado ou um caractere a mais são erros banais, mas podem produzir efeitos concretos: faturas devolvidas, pagamentos rejeitados, declarações com dados incorretos e tempo perdido a corrigir registos um a um.

O problema não está apenas em erros de digitação. Os dados chegam de muitas fontes: pedidos feitos por e-mail, cópias de documentos, importações de outros programas, preenchimentos manuais em balcão. Cada uma destas fontes introduz a sua forma de erro, e no fim a base de dados contém uma mistura difícil de controlar.

## Porque acontecem os erros

Os erros mais frequentes podem agrupar-se em poucas famílias:

- Dígitos trocados ou omitidos ao copiar um número de uma fatura para outra.
- Letras confundidas com números, por exemplo o O e o zero, ou o I e o um.
- Espaços e hífenes introduzidos de forma inconsistente, o que faz com que a mesma conta apareça escrita de três maneiras diferentes.
- Colunas deslocadas numa importação, de modo que o IBAN aparece no campo do NIF ou vice-versa.
- Dados antigos que estavam corretos quando foram recolhidos, mas que já não correspondem ao cliente atual.

Nem todos estes erros se detetam da mesma forma. Alguns podem ser verificados matematicamente, sem qualquer consulta externa. Outros só se detetam quando alguém confronta o documento original. Um guia prático deve separar bem estas duas situações.

## Como funciona a validação do NIF

O NIF, em Portugal, tem nove dígitos, e o último é um dígito de controlo calculado a partir dos oito anteriores. O cálculo típico consiste em multiplicar cada um dos oito primeiros dígitos por um peso decrescente de 9 a 2, somar os produtos, calcular o resto da divisão desse total por 11 e obter o dígito de controlo como a diferença entre 11 e esse resto. Se o resultado for 10 ou mais, o dígito de controlo considera-se zero.

O NIF espanhol também tem um componente de controlo, mas a sua forma depende do tipo de identificador. Por isso, a validação deve ser feita com a regra correta para cada caso, e não com um único algoritmo aplicado a tudo.

Importante: a validação do dígito de controlo confirma que o número é matematicamente possível. Não confirma que o NIF existe, que pertence à entidade indicada ou que está ativo. Um número pode passar na verificação e, ainda assim, estar errado se tiver sido copiado de outro cliente com a mesma estrutura.

## Como funciona a validação do IBAN

O IBAN é um identificador internacional de conta bancária. Em Portugal, tem 25 caracteres e começa por PT seguido de dois dígitos de controlo e de 21 dígitos. Em Espanha, tem 24 caracteres e começa por ES, seguido de dois dígitos de controlo e de 22 dígitos.

A verificação usa o método módulo 97, descrito na norma internacional. O procedimento é o seguinte:

1. Retirar os espaços e colocar tudo em maiúsculas.
2. Mover os quatro primeiros caracteres para o fim da cadeia.
3. Substituir cada letra pelo número correspondente, em que A vale 10, B vale 11 e assim sucessivamente até Z, que vale 35.
4. Calcular o resto da divisão do número resultante por 97.
5. Considerar válido se o resto for igual a 1.

Esta verificação deteta a maioria dos erros de digitação, porque um dígito trocado ou omitido altera o resto de forma quase sempre detetável. No entanto, tal como no NIF, não prova que a conta existe nem que pertence ao titular indicado.

## Passo a passo para validar uma lista

Uma forma prudente de tratar uma lista de registos é a seguinte:

1. Exportar ou preparar o ficheiro com um campo para o NIF e outro para o IBAN, identificando cada linha com um código interno, para que os resultados possam ser associados ao registo original.
2. Remover espaços desnecessários e uniformizar a escrita antes da validação, mas sem alterar os valores em si.
3. Validar o NIF de cada linha e registar o resultado.
4. Validar o IBAN de cada linha, indicando o país pelo prefixo.
5. Rever os registos inválidos, ler o motivo indicado para cada erro e confrontar com o documento original.
6. Corrigir na fonte, e não apenas no ficheiro de trabalho, para que o erro não reapareça na próxima importação.
7. Guardar o resultado final, com a data da validação, para ter registo do que foi verificado.

## Exemplo prático

Imagine-se um ficheiro com três fornecedores. O primeiro tem um NIF bem formado e um IBAN português com 25 caracteres que passa o módulo 97. O segundo tem um NIF com um dígito diferente do que o cálculo produziria, o que indica um erro de digitação ou de transcrição. O terceiro tem um IBAN espanhol com um dígito trocado, que falha o módulo 97 mesmo tendo o comprimento correto.

Neste caso, o resumo deveria indicar dois registos inválidos e um válido, e cada linha inválida deveria trazer o motivo concreto. A equipa administrativa sabe então exatamente onde concentrar a verificação, em vez de rever todo o ficheiro.

## O que a validação não garante

Convém ser explícito sobre os limites deste tipo de verificação:

- Não confirma que um NIF está registado nas finanças ou que está ativo.
- Não confirma que uma conta bancária existe, está aberta ou aceita débitos.
- Não confirma que o titular da conta é a entidade indicada no registo.
- Não substitui a verificação do documento original quando há dúvidas.

A validação serve para eliminar erros formais, que são os mais fáceis de evitar e os que mais tempo costumam fazer perder. Os restantes controlos continuam a depender de procedimentos da própria organização.

## Erros comuns

- Assumir que um registo validado está correto em todos os aspetos. A validação é uma primeira barreira, não uma garantia final.
- Corrigir apenas o ficheiro exportado e esquecer a base de dados de origem, o que faz o erro voltar na importação seguinte.
- Misturar regras de NIF português com regras espanholas, ou aplicar a validação de IBAN de um país a outro.
- Ignorar os espaços e hífenes introduzidos pelas pessoas, que tornam a mesma conta difícil de comparar.
- Não registar o que foi verificado e quando, o que dificulta explicar mais tarde a origem de um dado.

## Conclusão

Os NIF e IBAN errados são um problema pequeno em cada linha, mas acumulam-se quando as listas crescem. Separar a verificação formal, que pode ser feita em lote e com regras claras, da verificação documental, que exige confronto com a fonte, permite trabalhar com mais segurança e com menos esforço manual.

---

**Transparência:** este guia foi escrito com a ajuda de IA e verificado automaticamente antes de ser publicado. O autor vende uma ferramenta relacionada: [Validador em lote de NIF e IBAN (PT/ES)](https://buy.polar.sh/polar_cl_XI3OWNJDe5nSxN9PMv2Xt3swDVS2L5B3ELWoK31SBt3?utm_source=guides&utm_medium=organic&utm_campaign=offer-4&utm_content=problema-nif-iban-errados-listas-clientes-fornecedores). O conteúdo acima é útil sem a comprar.
