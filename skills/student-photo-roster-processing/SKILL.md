---
name: student-photo-roster-processing
description: Processar fichas digitalizadas de alunos em Word/PDF, localizar e recortar retratos, associá-los a listas de turma, melhorar imagens sem alterar rostos, nomear ficheiros e organizar pastas. Usar quando o utilizador pedir extração ou organização de fotografias de alunos a partir de fichas, cartões ou listas digitalizadas.
---

# Processamento de fotografias de alunos e listas de turma

## Princípios obrigatórios

- Tratar fichas, nomes, números de processo e fotografias — sobretudo de menores — como dados pessoais sensíveis. Preferir processamento local e não carregar, publicar ou enviar fontes para serviços externos sem autorização explícita.
- Nunca identificar ou associar alunos pela aparência, reconhecimento facial ou comparação de rostos. Associar apenas por evidência documental: nome escrito na ficha, lista nominal legível e, quando disponível, número de processo.
- Usar OCR como sugestão, não como confirmação. Se a escrita, a lista ou a correspondência forem incertas, manter a foto sem nome confirmado, assinalar a página para revisão e explicar a pendência.
- Preservar os originais. Criar cópias de saída; não alterar ficheiros-fonte.
- Não gerar, reconstruir, embelezar, substituir nem retocar localmente partes do rosto. Limitar melhorias a ajustes globais e reversíveis de luminosidade, contraste, balanço de cor, rotação e recorte, sem deformar feições.

## Fluxo de trabalho

1. **Inventariar fontes e destino.** Listar documentos, formatos, turmas, contagem de páginas e listas disponíveis. Confirmar o espaço de trabalho atual. Manter fontes intactas e separar entradas, intermediários e entregáveis (`inputs/`, `.work/`, `outputs/`) quando apropriado.
2. **Mapear páginas de forma verificável.** Para DOCX, não assumir que a ordem dos ficheiros `word/media/imageN` dentro do ZIP corresponde à ordem das páginas. Se o documento usar VML (`w:pict`/`v:imagedata`), seguir os `r:id` de `word/document.xml` e resolvê-los em `word/_rels/document.xml.rels`, preservando a ordem do XML. Para DrawingML, seguir `a:blip` e as relações equivalentes. Verificar a correspondência com páginas conhecidas ou folhas de contacto antes do lote.
3. **Separar fichas de páginas-lista.** Inspecionar folhas de contacto por turma e localizar listas, páginas em branco, duplicados e páginas rodadas. Excluir listas e páginas sem retrato da pasta final de alunos; registar exceções no manifesto. Não inferir que o número de página equivale ao número de alunos.
4. **Localizar e recortar o retrato.** Recortar a fotografia de cada ficha pela sua geometria; manter proporções e conteúdo suficiente da cabeça e ombros. Verificar orientação, margens e nitidez em amostras representativas e exceções. Não usar deteção de identidade facial para estabelecer correspondências.
5. **Associar nome e turma.** Ler o campo do nome na ficha e confrontá-lo com a lista nominal da mesma turma. Usar o número de processo como verificação adicional, não como substituto de uma fonte clara. Confirmar grafia e associação documental quando a fonte permitir. Guardar candidatos OCR num campo separado; nunca copiá-los automaticamente para o nome final sem revisão. Assinalar divergências, nomes repetidos, ausências e páginas não legíveis.
6. **Melhorar sem alterar o rosto.** Aplicar apenas operações determinísticas globais e suaves às cópias, por exemplo correção moderada de luminosidade/contraste e rotação. Preservar proporção, cor natural e expressão. Evitar filtros de beleza, restauração generativa, troca de fundo ou qualquer operação que crie ou remova traços faciais.
7. **Organizar e nomear.** Criar uma pasta por turma. Nomear fotos confirmadas como `Nome do aluno - Turma.jpg` (ou no formato pedido). Substituir caracteres inválidos do sistema operativo sem perder a grafia no manifesto. Para lotes pedidos integralmente mas ainda não confirmados, usar um marcador inequívoco como `A_CONFIRMAR - Turma - p001.jpg`; não disfarçar marcadores como nomes de alunos.
8. **Gerar controlo de revisão.** Incluir um manifesto com, pelo menos, turma, página de origem, nome confirmado, candidato OCR (se existir), número de processo confirmado/candidato, ficheiro de destino e estado. Criar `pendencias.csv` para associações, páginas-lista, divergências e ficheiros ausentes. Distinguir contagem de imagens/páginas de contagem de alunos.
9. **Validar antes de entregar.** Conferir amostras das fotos, abrir/decodificar todas as imagens, verificar que cada turma tem a pasta correta, procurar ficheiros em falta, nomes duplicados, listas indevidamente exportadas e saídas vazias. Comparar contagens do manifesto, pastas e ZIP. Testar a integridade do arquivo comprimido. Informar claramente o que ficou confirmado e o que permanece pendente.

## Lotes extensos ou OCR fraco

- Processar por turma e guardar resultados intermédios; não fazer um único lote irreversível.
- Se o OCR manuscrito falhar, testar uma amostra pequena com diferentes segmentações e recortes. Localizar o campo pela âncora impressa ou pela geometria da ficha e inspecionar visualmente o recorte antes de automatizar o lote.
- Se as listas só existirem como scans, transcrever ou rever por blocos e cruzar com as fichas. Pedir uma lista em Excel/CSV apenas quando a revisão segura a partir do scan não for viável.
- Não declarar o trabalho completo enquanto houver imagens por associar. Entregar um lote parcial só com indicação explícita das turmas e páginas por rever.

## Entrega

- Entregar pastas por turma e, se útil, um ZIP com os respetivos ficheiros.
- Incluir manifesto, pendências e uma nota breve sobre o método de melhoria aplicado, confirmando que não houve retoque facial.
- Ligar cada ficheiro final por caminho absoluto e resumir contagens confirmadas e pendentes. Não anexar scripts de diagnóstico ou folhas de contacto, salvo pedido do utilizador.
