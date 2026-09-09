# Chapitre 4 -- ERC-7821 : executer des lots d appels

`erc7821/Execute.ts` et `erc7821/Calls.ts` implementent l encodage et le
decodage de la fonction `execute(bytes32 mode, bytes executionData)`,
l interface standard ERC-7821 pour soumettre un lot d appels (batch calls) a
un smart account en une seule transaction.

Le module definit trois modes d execution predefinis sous forme de
constantes `bytes32` : `mode.default` (execution simple d un lot d appels),
`mode.opData` (le meme lot accompagne de donnees operationnelles
additionnelles, par exemple pour une autorisation hors-chaine), et
`mode.batchOfBatches` (plusieurs lots imbriques, executes ensemble). Ce
mode est identifiable par son prefixe fixe
`0x0100000000007821000200...`, ou le segment `7821` dans le motif de bytes
n est pas un hasard : il code visuellement le numero de l ERC directement
dans l identifiant de mode, une convention lisible pour qui inspecte le
mode brut sur un explorateur de blocs.

`decodeData` recoit les donnees encodees de l appel `execute`, decode
d abord la paire `(mode, executionData)` via l ABI de la fonction
(`abiFunction`), puis delegue a `Calls.decode` le decodage du contenu reel
des appels -- en lui passant un indicateur `opData` derive du mode
(vrai si le mode n est pas `mode.default`), pour savoir si des donnees
operationnelles supplementaires doivent etre extraites en plus du tableau
d appels. `decodeBatchOfBatchesData` suit une logique similaire mais pour
le mode imbrique : apres avoir isole l `executionData`, il decode un
tableau `bytes[]` ou chaque element est lui-meme un lot d appels
serialise separement, refletant la structure "batch of batches" du mode
correspondant.
