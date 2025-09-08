# 📊 Highway [VisioAi]: Índice de Saúde Financeira das Lojas HighWay  - Relatório [Diário de Bordo]


## 🛠️ Contexto Geral do Problema + Insights:
De forma resumida, era tratar os dados provindos de bases CSV para poder criar um índice de saúde com informações que eu julgo interessantes e valiosas para poder alimentar um dashboard no Looker.


Entrando mais no problema, foram 5 arquivos .CSV recebidos com algumas colunas, minha primeira reação foi subir estes dados no banco de dados que a empresa utiliza, criei a conta no Google BigQuery e descobri como colocava os dados lá.


Depois comecei a explorar os dados de cada tabela de forma individual e comparar o schema disponibilizado dentro do arquivo inicial do notion.


Após entender um pouco mais sobre as tabelas, comecei a pegar todas elas com as queries presentes no notebook main e transformar em dataframe.


Enfrentei problemas ao dar join (merge) nas tabelas, estava fazendo sem agrupar por identificador no primeiro momento pois não sabia que existiam mais de um identificador (fk_receipt_identifier) para cada tabela, então sumarizei (agrupei) os dados por ele.


Utilizei a query para validar se isso realmente fazia sentido:
SELECT fk_receipt_identifier, COUNT(fk_receipt_identifier) AS qtd
FROM minha_tabela_do_bq
GROUP BY fk_receipt_identifier;


Então ao rodar isso em todas, consegui chegar a essa conclusão.


Apenas foi diferente na tabela de torque, pois os resultados estavam multiplicando e gerando cerca de 79 milhões de linhas, por isso este é o único que é agrupado por df_torque_resultado e operation_date, e por isso que existe um tratamento com as datas que é diferenciado e removido depois, pois foi apenas para isso.


Mas eu utilizei todas as tabelas neste join.


No primeiro momento a ideia era apenas tratar os dados e padronizar o máximo possível, sem responder perguntas ainda.


## 🛠️ Fonte e preparação de dados:
Resolvi subir os dados brutos no BQ apenas para ter mais contato com ele, saber como funciona e explorar mesmo, nada além disto.


Resolvi subir todas as tabelas, as com dados brutos e após tratar todas as tabelas também, inclusive as duas tabelas de índice de saúde, além de centralizar todas as tabelas em um único lugar acessível (sem ser o meu computador), também queria testar a conexão do Looker com o BQ, sei que deveria ser nativo e mais intuitivo, mas não senti diferenças entre subir um .XLSX, .CSV ou conectar ao BQ, talvez na velocidade de carregamento, mas nada muito grande.


## 🛠️ Construção do Índice de Saúde Financeira:
Ah sim, a maior parte do projeto.


Bom, depois de pegar os dados tratados do BQ, me deparei com um problema, de que não conseguiria gerar os índices direto no Looker, pois não é possível fazer várias agregações com o mesmo campo nos campos calculados, então resolvi tratar no python mesmo, mas para evitar problemas e facilitar a minha vida na hora de criar os dados no Looker, achei melhor criar dois datasets, um calculando as informações gerais, com todas as datas fornecidas e outro mensal.


Como a métrica principal é o Torque, me baseei (e muito) nessa métrica, tanto que no dashboard acredito que esteja um tanto quanto repetitivo.


Mas, enfim, comecei agrupando os valores de faturamento, quantidade de itens, descontos fornecidos, torque, torque de delivery e torque em loja, agrupei esses valores por loja e depois por mês e loja.


Acredito que coloquei o valor dos descontos apenas para ter uma métrica diferente, acho interessante ver os valores que o dono da loja "deixou de ganhar", embora os gráficos mostram que a grande maioria das compras não foram feitas com cupom, mas neste momento eu ainda não tinha essas informações.


## 🛠️ Ranking:
Foram descobertas legais que eu tive, além da citada acima, as lojas que estão no topo do ranking geral, tendem a se manter no topo mês a mês. Pode parecer óbvio, mas a competição entre as lojas é pouca, é nítido qual é a melhor e qual é a que precisa de mais atenção, então a loja localizada no Adidas Shopping está no topo em quase todos os meses e a loja da Praça Ekin, está precisando de um pouco (muito) mais atenção do que as outras.


Mas, o foco dessas lojas são outros, nenhuma outra loja gera tanto torque de delivery quanto a loja da Praça Ekin, ou seja, a sua clientela não está exatamente fisicamente no local, então pode ser interessante investir mais no delivery da loja, promover promoções para aquela loja, deixar ela aberta em horários diferenciados (visto que quem pede comida online, não tem hora e nem perfil), promover um bem-estar melhor para os entregadores, afinal são eles que fazem isso acontecer também ou até mesmo contratar entregadores próprios da loja para que os pedidos cheguem mais rápidos.


Outra coisa, a loja do Adidas Shopping é um caso de estudo interessante, pois ela vende MUITOS itens, mais que o dobro do segundo colocado, isso no período de Junho de 2025, mas ela é a segunda loja do ranking, ficando atrás da loja da Av. Nova Balança, isso parece estranho de primeiro momento, mas acontece que eles estão vendendo mais itens mas com menor valor por item, isso pode ser 'culpa' de diversos fatores, pode ser que tenham cupons mais aplicados em itens mais caros, embora na loja da Av. Nova Balança, o valor dos cupons seja maior, pode ser que seja um total de valores menores e a loja do Adidas Shopping, esteja aplicando os cupons somente em itens mais caros, isso poderia justificar o menor valor dos cupons no geral, mas mais itens vendidos e torque mais baixo.


## 🛠️ Recomendações:
Olha, a curto prazo eu diria foque na loja da Praça Ekin, comparada as outras ela está respirando por aparelhos, mas não é um caso sem solução, pelos dados podemos ver que o delivery pode ser a solução, apesar de ser diferente do padrão das outras lojas que possuem mais pessoas físicas no local, pode ser que essa loja vire modelo quando o assunto é delivery, talvez uma boa seja que o preço do delivery seja o mesmo da loja, caso seja feito pelo entregador da loja e não em plataformas de terceiros, isso para captar clientes e também para ser diferente e também entender mais o mercado.


## 🛠️ Limitações e próximos passos:
Acho que não faltou nada na verdade, muitos dados também podem acabar atrapalhando, para soluções detalhadas das lojas de ações a serem tomadas, estes dados estão perfeitos.


Se tivesse mais dados de tempo, acredito que poderíamos ver os efeitos da pandemia nas lojas e ver onde que a loja da Praça Ekin começou a alavancar no delivery, poderíamos entender os motivos e as tendências desse crescimento e também, se desse, os motivos geográficos. Pode ser que essa loja faça mais delivery pois o acesso de pessoas é limitado, mas de moto não é, ela pode estar localizada perto das avenidas principais ou de escritórios, enfim, estes dados seriam apenas para uma análise 100% detalhada dos motivos de uma loja específica, de modo geral, acredito que os dados presentes estejam ideais.