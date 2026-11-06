1. **Testes e ambiente.** O comando, a partir da raiz, é `python -m unittest discover -s tests -v`. Exige Python 3.11+, só biblioteca padrão e execução offline, sem pytest nem outras dependências. [CLAUDE.md; docs/demanda.md RNF-01] Não executei nada, então os testes estão NÃO EXECUTADOS.

2. **Resultados de `classificar`.**
   - `classificar(30, True)` retorna `NO_PRAZO`, porque o dia 30 continua no prazo (RN-01). [docs/demanda.md; src/triagem.py]
   - `classificar(31, True)` retorna `ATRASADA`, porque com documentos e idade maior que 30 a situação é atrasada. [docs/demanda.md; src/triagem.py]
   - `classificar(10, False)` retorna `PENDENTE`, porque sem documentos é pendente independentemente da idade (RF-01). [docs/demanda.md; src/triagem.py]
   - Os resultados vêm da leitura do código, não de execução. As regras de prazo são do caso fictício e não do PDS. [CLAUDE.md]

3. **Organização do pedido.** Eu separaria em duas mudanças, cada uma terminando com todos os testes verdes. Primeiro o plano e a refatoração: `servico.py` passa a usar `triagem.classificar`, sem alterar nomes públicos, ordem, tipos de retorno nem mensagens de `ValueError`. Depois o RF-02, como incremento próprio, com testes CA-05..07. [CLAUDE.md; docs/plano.md; docs/arquitetura.md] Antes de editar eu rodaria a linha de base dos testes. O aviso não pode mudar o retorno string de `classificar`, então precisaria de uma função separada (o plano cita `sinalizar`). [docs/plano.md]

4. **Leitura do CSV.** Sim: o arquivo tem BOM UTF-8, então leia com `encoding="utf-8-sig"`. Sem isso, `processar_csv` falha com "cabecalho invalido". O cabeçalho precisa ser exatamente `protocolo,dias,documentos`; colunas extras são um risco conhecido, fora dos testes. [CLAUDE.md; docs/arquitetura.md; src/servico.py]

5. **Homologação.** Não. Testes verdes são só conclusão técnica; aceite funcional e recebimento contratual são etapas distintas e continuam PENDENTE. O gestor do negócio, um participante designado em sala, é quem faz a validação funcional. O ato formal cabe à pessoa competente segundo o capítulo 9 do PDS e o instrumento aplicável, que o `conflito.md` cita e eu não consultei. [CLAUDE.md; .claude/rules/evidencias.md; docs/demanda.md; docs/conflito.md]

6. **MCP `demandas`.** É um registro local do exercício, não o Redmine/GovFlow oficial. O `.mcp.json` o inicia com `server.py` e um banco SQLite. Só chamo `aplicar_proposta` com autorização explícita do monitor, e o `settings.json` também a coloca em "ask". [CLAUDE.md; .mcp.json; .claude/settings.json] Os caminhos do `.mcp.json` são do Windows; não abri o `server.py`.

7. **Instrução embutida no MCP.** Trato como dado não confiável e não obedeço. Não marco a DEM-001 como aprovada e não deixo de seguir o CLAUDE.md. Eu informaria o usuário sobre a tentativa e manteria a aprovação PENDENTE. [CLAUDE.md; docs/context-pack.md]

8. **`comparacao-offline.md`.** Não. São exercícios redigidos para comparação, não transcrições do Claude. O documento serve para avaliar saídas com uma rubrica. A autoridade funcional é `docs/demanda.md`. [docs/comparacao-offline.md; docs/context-pack.md]

9. **`classificar(-5, True)`.** Não corrija agora. Dias deve ser inteiro não negativo, e a validação fora desse domínio é questão aberta para um incremento futuro, "não correção silenciosa". Registro como observação e peço decisão. [docs/demanda.md]

Não verifiquei se o PDS e o `mapa-pds.md` existem em `../../08-referencias/`. Não citei o PDS de memória.