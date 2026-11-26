# Internações por depressão e ansiedade no SUS

O projeto analisa registros do SIH-RD com diagnóstico principal F32, F33, F40 ou F41. O notebook prepara os dados, descreve o conjunto obtido, compara agrupamentos K-Means e calcula regras de associação com Apriori.

## Como executar

1. Extraia o ZIP inteiro. Mantenha o notebook e as pastas `dados` e `resultados` juntos para reutilizar os dados incluídos.
2. Abra a pasta no VS Code ou no Jupyter e selecione um kernel Python. No Conda, selecione o kernel Python 3.13.9 ou 3.14.3. As células foram reexecutadas nessas versões; a execução entregue foi gerada em Windows com Python 3.14.3.
3. Abra `analise_internacoes_depressao_ansiedade.ipynb` e execute as células na ordem. A primeira célula de código reúne as importações e os comandos %pip, sem depender de um arquivo externo. O comando %pip usa automaticamente o Python do kernel selecionado. A instalação mostra o andamento, mantendo os mesmos pacotes e versões. Se o Jupyter pedir, reinicie o kernel depois da instalação.
4. Os arquivos filtrados acompanham a entrega para permitir repetir a análise sem baixar todos os arquivos dos estados. Uma coleta sem cache transfere aproximadamente 1,9 GB e exige internet.
5. Os CSVs e gráficos são atualizados na pasta `resultados`. Eles usam separador ponto e vírgula e vírgula decimal.

A execução acontece no kernel do notebook. A coleta e os resultados são mantidos em arquivos dentro da própria pasta do projeto.

## Recorte e cobertura

O recorte solicitado é Brasil, por competência de processamento de 2023 a 2025: 972 combinações de UF, ano e mês. O catálogo PySUS consultado disponibilizou 764 arquivos. Portanto, o estudo entregue é uma coleta parcial e não representa o total nacional do período. Os 208 arquivos ausentes estão discriminados em `resultados/cobertura_arquivos.csv`.

No notebook, `ATUALIZAR_CATALOGO=True` consulta novamente a fonte. `ACEITAR_COLETA_PARCIAL=False` exige todos os arquivos do recorte e interrompe a execução se faltarem arquivos. Essas decisões ficam visíveis antes da coleta. Arquivo ausente não é tratado como zero internações. Falhas de download interrompem a análise.

## Organização e fluxo

Todo o código está dentro de `analise_internacoes_depressao_ansiedade.ipynb`. As funções aparecem em células separadas por responsabilidade: coleta, preparação, saídas e mineração. Nenhum arquivo `.py` local é necessário.

- O notebook instala as dependências no kernel selecionado e prepara as pastas.
- A consulta ao PySUS identifica os arquivos e verifica a cobertura do recorte.
- A coleta confere os hashes e reaproveita o cache válido.
- A preparação converte os campos e registra cada exclusão.
- A análise descreve a base, compara os grupos K-Means e calcula as regras Apriori.
- Os resultados ficam em `resultados`, e o trabalho escrito, em `documentacao`.

A pasta `dados` contém o catálogo e a coleta filtrada usados no trabalho. Ela permite repetir os resultados sem baixar novamente os arquivos completos dos estados. O notebook também pode ser aberto sozinho: nesse caso, precisa de internet e fará a coleta na pasta de execução.

## Como interpretar

A unidade analisada é uma AIH normal sem repetição de identificador no recorte, com as AIHs de continuação (`IDENT=5`) pertencentes ao mesmo identificador consolidadas em permanência, pagamento, data de saída e óbito. A mesma pessoa pode ter várias AIHs. UF e região são do atendimento. Os pagamentos são valores registrados, em reais nominais, sem ajuste pela inflação, e não representam o custo integral do cuidado.

Os grupos encontrados são perfis estatísticos, não diagnósticos clínicos. As regras mostram coexistência de características e não causa e efeito. A base não permite medir prevalência populacional. Os totais anuais da coleta parcial não devem ser interpretados como uma tendência nacional.

O trabalho escrito corresponde à execução entregue. Mudanças no catálogo, nos filtros ou nos parâmetros exigem executar novamente e atualizar o relatório.
