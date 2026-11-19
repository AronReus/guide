---
title: "O problema dos NIF e IBAN errados em listas de clientes e fornecedores"
description: "Porque é que erros de NIF e IBAN em listas de clientes e fornecedores são comuns, quais são as suas consequências e como uma verificação em lote, com o motivo de cada erro, ajuda a detetá-los antes de chegarem ao processamento."
---

# O problema dos NIF e IBAN errados em listas de clientes e fornecedores

## Onde começa o problema

Em muitos gabinetes de contabilidade e pequenas empresas, as listas de clientes e fornecedores são construídas ao longo de anos. Os dados entram por formulários em papel, por correio eletrónico, por ficheiros copiados de outros sistemas e por digitação manual. Cada uma destas origens introduz oportunidades para erros: um dígito trocado, um espaço a mais, uma letra no sítio errado, um número truncado ao copiar.

Dois dos campos mais sensíveis são o número de identificação fiscal (NIF) e o identificador bancário internacional (IBAN). O NIF aparece em faturas, declarações e comunicações com a administração fiscal. O IBAN aparece em ordens de pagamento, débitos diretos e reembolsos. Um erro num destes campos raramente se nota no momento em que é introduzido. Costuma aparecer semanas depois, quando uma fatura é devolvida, um pagamento é rejeitado ou uma declaração é notificada com inconsistências.

## Porque é que o erro passa despercebido

O NIF e o IBAN têm uma propriedade útil: contêm um dígito ou par de dígitos de controlo calculado a partir dos restantes. Essa redundância permite detetar muitos erros de digitação sem consultar base de dados nenhuma. Se alguém escreve um número com um dígito trocado, na maioria dos casos o cálculo do controlo deixa de bater certo.

No entanto, esta verificação não acontece automaticamente. Os programas de faturação nem sempre validam o campo ao gravar, sobretudo quando os dados foram importados de uma folha de cálculo. Uma lista com centenas de linhas é, na prática, revista por olho, e o olho humano não faz contas de módulo 11 ou de módulo 97.

## Como funciona a validação do NIF português

O NIF português tem nove dígitos. O primeiro dígito indica o tipo de entidade (por exemplo, pessoas singulares e coletivas começam por valores diferentes), e o último é o dígito de controlo.

O cálculo é o seguinte:

1. Multiplica-se cada um dos primeiros oito dígitos por um peso decrescente: 9, 8, 7, 6, 5, 4, 3 e 2.
2. Somam-se os produtos.
3. Calcula-se o resto da divisão da soma por 11.
4. Subtrai-se esse resto a 11. Se o resultado for 10 ou 11, o dígito de controlo é 0. Caso contrário, é o próprio resultado.

Exemplo: para 12345678, a soma ponderada é 9×1 + 8×2 + 7×3 + 6×4 + 5×5 + 4×6 + 3×7 + 2×8 = 156. O resto de 156 dividido por 11 é 2. Logo, 11 − 2 = 9, e o NIF completo é 123456789. Se a lista contiver 123456780, o último dígito não corresponde ao cálculo e a linha deve ser assinalada.

## Como funciona a validação do NIF espanhol

O NIF espanhol de pessoas singulares tem oito dígitos seguidos de uma letra de controlo. A letra é obtida calculando o resto da divisão do número por 23 e consultando uma tabela fixa de letras, na ordem T, R, W, A, G, M, Y, F, P, D, X, B, N, J, Z, S, Q, V, H, L, C, K, E.

Exemplo: 12345678 dividido por 23 dá resto 14, e a 14.ª posição da tabela é Z. O NIF correto é 12345678Z. Se a letra estiver errada, a linha é marcada.

Convém ter em atenção que o NIF espanhol de pessoas singulares não é o mesmo que o NIE (número de identidade de estrangeiro), que tem outro formato. Ao preparar uma lista, deve separar-se claramente o que se pretende validar.

## Como funciona a validação do IBAN

O IBAN combina um código de país, dois dígitos de controlo e o identificador da conta. Para Portugal, tem 25 caracteres e começa por PT. Para Espanha, tem 24 caracteres e começa por ES.

A validação por módulo 97 segue estes passos:

1. Remove-se qualquer espaço do IBAN.
2. Passam-se os primeiros quatro caracteres (código do país e dígitos de controlo) para o fim da cadeia.
3. Substitui-se cada letra pelo número correspondente, em que A é 10, B é 11 e assim sucessivamente até Z, que é 35.
4. Calcula-se o resto da divisão do número resultante por 97.
5. Se o resto for 1, o IBAN é matematicamente consistente.

Este teste deteta erros de digitação, dígitos trocados e muitas omissões. Não garante, porém, que a conta exista nem que pertença ao titular indicado. É uma verificação de forma, não de existência.

## Exemplos práticos de erros comuns

Alguns erros aparecem repetidamente em listas reais:

- Espaços e separadores: IBANs escritos em grupos de quatro caracteres são válidos depois de os espaços serem removidos, mas podem falhar se a coluna for importada com os espaços como parte do valor.
- Zeros à esquerda perdidos: ao abrir um ficheiro numa folha de cálculo, um NIF que começa por zero pode perder esse zero e passar a ter oito dígitos.
- Letras confundidas: o O e o 0, ou o I e o 1, são trocas frequentes ao digitar à mão.
- Dígitos transpostos: escrever dois dígitos em ordem trocada é um erro que o cálculo de controlo apanha, mas que facilmente passa na leitura.
- Campos deslocados: uma importação com colunas desalinhadas pode colocar um IBAN no campo do NIF. A validação marca-o como inválido, o que ajuda a identificar o problema de estrutura.

## O que uma verificação em lote deve devolver

Para ser útil, uma verificação em lote precisa de mais do que um simples sim ou não. Deve indicar, para cada linha, qual o campo afetado e qual o motivo do erro, por exemplo, dígito de controlo incorreto ou comprimento inválido. Sem essa informação, a equipa tem de adivinhar onde está o problema.

É também útil um resumo com o número total de registos, o número de válidos e o número de inválidos. Esse resumo serve de indicador rápido sobre a qualidade da lista antes de qualquer processamento.

Por fim, o resultado deve poder ser guardado num ficheiro CSV, para que a equipa possa filtrar as linhas com erro, corrigi-las na origem e voltar a verificar.

## Um fluxo de trabalho prudente

Uma abordagem prática pode seguir estes passos:

1. Exportar a lista do sistema de origem em formato de texto ou CSV, mantendo os campos separados.
2. Verificar o NIF e o IBAN em lote, separando as duas colunas.
3. Rever as linhas marcadas, confirmando o dado original em documentos do cliente ou fornecedor.
4. Corrigir na origem, e não apenas no ficheiro de trabalho, para evitar que o erro volte numa próxima importação.
5. Repetir a verificação e guardar o resultado para registo.

## Limites que convém conhecer

A validação do dígito de controlo não substitui a confirmação junto do titular. Um NIF pode ser matematicamente correto e pertencer a outra pessoa, e um IBAN pode passar no módulo 97 sem corresponder a uma conta ativa. Estas verificações reduzem erros de digitação, mas não substituem a diligência habitual de quem gere os dados.

A ferramenta cobre o NIF português e espanhol, e o IBAN de Portugal e Espanha. Outros países, outros tipos de identificador e outros formatos de conta ficam fora do seu âmbito, e os resultados para esses casos não devem ser interpretados como validação.

## Conclusão

Erros em NIF e IBAN são discretos, custosos quando descobertos tarde e fáceis de evitar com uma verificação feita logo na origem. O essencial é perceber como funcionam os dígitos de controlo, reconhecer os erros mais comuns e manter um processo que obrigue a rever as linhas assinaladas antes de os dados seguirem para faturação ou pagamentos.

---

**Transparência:** este guia foi escrito com a ajuda de IA e verificado automaticamente antes de ser publicado. O autor vende uma ferramenta relacionada: [Validador em lote de NIF e IBAN (PT/ES)](https://buy.polar.sh/polar_cl_XI3OWNJDe5nSxN9PMv2Xt3swDVS2L5B3ELWoK31SBt3?utm_source=guides&utm_medium=organic&utm_campaign=offer-4&utm_content=problema-nif-iban-errados-listas-clientes-fornecedores). O conteúdo acima é útil sem a comprar.
